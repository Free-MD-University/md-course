# Exercice : Docker in Docker (DIND)

Objectif : comprendre pourquoi Docker dans Docker nécessite une configuration particulière, expérimenter l'échec du mode naïf, utiliser le bind du socket Docker, mesurer les risques (socket **et** `--privileged`), découvrir une 2ᵉ solution sans bind, puis **investiguer** une 3ᵉ piste via les runtimes (Sysbox) — sans corrigé imposé.

## Contexte : pourquoi parler de DIND ?

Dans les prochains chapitres, vous allez rencontrer des cas où **un conteneur doit lui-même lancer d'autres conteneurs** :

- pipelines **CI/CD** (GitLab Runner, Jenkins, GitHub Actions self-hosted…)
- environnements de **build** isolés
- labs et exercices où l'on veut encapsuler tout un stack Docker dans un seul conteneur
- outils qui orchestrent des conteneurs à la volée

Ce pattern s'appelle souvent **Docker in Docker** (DIND). L'idée est simple : *exécuter Docker à l'intérieur d'un conteneur*. En pratique, ce n'est **pas** aussi trivial qu'un `docker run` classique — il faut une configuration spéciale.

> **Note :** on verra des exemples concrets de DIND dans les chapitres suivants. Ici, on pose les bases.

---

## Partie 1 : L'image officielle `docker:dind`

Docker publie une image dédiée au Docker-in-Docker :

| Élément | Valeur |
|---------|--------|
| Image | [`docker:dind`](https://hub.docker.com/_/docker) |
| Rôle | Lance un **daemon Docker** à l'intérieur du conteneur |
| Variante CLI seule | `docker` (client uniquement, sans daemon) |

### 🎯 Tâche : lancer l'image telle quelle

Sans lire la suite, essayez de lancer l'image DIND de la manière la plus simple possible :

```bash
docker run docker:dind
```

**Questions :**

1. Que se passe-t-il ? Le conteneur démarre-t-il correctement ?
2. Quel message d'erreur obtenez-vous (s'il y en a un) ?
3. Pourquoi un simple `docker run docker:dind` ne suffit-il pas ?

**Indices :**

- Observez les logs : `docker logs <container_id>`
- Comparez avec un conteneur Ubuntu classique : quelle différence de besoins au niveau système ?

> **Résultat attendu :** ça **doit échouer** (ou rester bloqué / crasher). C'est normal — on va comprendre pourquoi.

---

## Partie 2 : Le bind du socket Docker

Avant d'aborder la vraie solution DIND, une approche très répandue consiste à **monter le socket Docker de l'hôte** dans le conteneur.

### Qu'est-ce que le socket Docker ?

Sur l'hôte Linux, le daemon Docker écoute sur un socket Unix :

```
/var/run/docker.sock
```

Ce socket est l'**API** du daemon Docker. Toute commande `docker` (run, ps, build, rm…) passe par ce socket pour parler au daemon.

### Le bind mount du socket

Un **bind mount** (`-v`) permet de rendre un fichier ou répertoire de l'hôte visible **à l'intérieur** du conteneur, au même chemin ou à un autre :

```bash
-v /var/run/docker.sock:/var/run/docker.sock
```

Schéma simplifié :

```
┌─────────────────────────────────────────┐
│  Hôte (VPS)                             │
│                                         │
│  docker daemon ◄── /var/run/docker.sock │
│         ▲                               │
│         │ bind mount                    │
│  ┌──────┴──────────────────────────┐    │
│  │  Conteneur "docker"             │    │
│  │  docker CLI ──► même socket     │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

Le conteneur **ne lance pas** son propre daemon Docker : il **réutilise celui de l'hôte**. On parle parfois de **DooD** (*Docker-outside-of-Docker*), pas de vrai DIND.

### 🎯 Tâche : comprendre le bind

1. Sur votre VPS, vérifiez que le socket existe :

```bash
ls -l /var/run/docker.sock
```

2. Qui est propriétaire de ce fichier ? Quels sont les droits ?
3. Que signifie le type de fichier (`s` dans `srw-rw----`) ?

---

## Partie 3 : Problèmes et conséquences du bind du socket

Monter `/var/run/docker.sock` est pratique, mais **très dangereux** si on ne mesure pas les implications.

### Problème 1 : accès root de facto sur l'hôte

Toute personne (ou tout processus) dans le conteneur qui peut parler au socket Docker peut :

- lancer **n'importe quel conteneur** sur l'hôte
- monter **n'importe quel répertoire** de l'hôte via `-v /:/host`
- lire des secrets, clés SSH, fichiers sensibles
- supprimer tous les conteneurs et volumes de la machine

> **Règle à retenir :** accès au socket Docker ≈ accès **root** sur la machine hôte.

### Problème 2 : pas d'isolation réelle

Les conteneurs lancés depuis le conteneur « parent » tournent en réalité **sur l'hôte**. Ils apparaissent dans `docker ps` de l'hôte, partagent les mêmes images, réseaux et volumes.

### Problème 3 : risque en CI/CD

Un job malveillant ou compromis dans un pipeline CI qui a accès au socket peut :

- exfiltrer des variables d'environnement d'autres jobs
- modifier l'infrastructure
- miner de la crypto, installer un backdoor, etc.

### Problème 4 : effets de bord et sécurité opérationnelle

- Un `docker rm -f $(docker ps -aq)` lancé depuis le conteneur **vide toute la machine**
- Les builds peuvent polluer le cache d'images de l'hôte
- Difficile d'auditer qui a lancé quoi (tout est attribué au daemon hôte)

### 🤔 Questions de réflexion

- Dans quels cas le bind du socket reste-t-il acceptable malgré tout ?
- Pourquoi beaucoup de guides CI montrent quand même `-v /var/run/docker.sock:...` ?
- Quelle alternative offre une **vraie** isolation ?

---

## Partie 4 : Docker dans Docker via le bind du socket

Maintenant, mettez la théorie en pratique avec le bind du socket.

### 🎯 Tâches

1. **Lancer un conteneur avec le client Docker et le socket bindé :**

```bash
docker run -it --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  docker
```

2. **Entrer dans ce conteneur** (si vous n'êtes pas déjà en interactif) et vérifier que Docker répond :

```bash
docker ps
docker version
```

3. **Depuis l'intérieur de ce conteneur, lancer un conteneur Ubuntu :**

```bash
docker run -it --rm ubuntu bash
```

4. **Validation croisée :**
   - Depuis le conteneur Ubuntu, exécutez `hostname` et notez le résultat
   - Ouvrez un **second terminal** sur le VPS (hôte) et lancez `docker ps`
   - Le conteneur Ubuntu apparaît-il dans la liste de l'**hôte** ?

5. **Nettoyage :** quittez le conteneur Ubuntu, puis le conteneur parent.

**Questions :**

- Où tourne réellement le conteneur Ubuntu : dans le conteneur parent ou sur l'hôte ?
- Pourquoi cette approche n'est-elle pas du « vrai » Docker in Docker ?

---

## Partie 5 : Trouver une solution **sans** bind du socket

Vous avez vu que monter `/var/run/docker.sock` fonctionne, mais avec de lourdes conséquences de sécurité et sans isolation réelle.

### 🎯 Défi

Trouvez une façon de :

1. Lancer un conteneur capable d'exécuter des commandes `docker` **sans** monter le socket de l'hôte
2. Depuis ce conteneur, lancer un conteneur Ubuntu (comme à la partie 4)
3. Vérifier que les conteneurs lancés **n'apparaissent pas** dans `docker ps` de l'hôte (ou qu'ils sont bien isolés dans le daemon interne)

**Indices (à découvrir par vous-mêmes) :**

- Relisez la partie 1 : pourquoi `docker:dind` échoue-t-il « tel quel » ?
- Quelle option Docker accorde des capacités système étendues à un conteneur ?
- Consultez la documentation de l'image [`docker:dind`](https://hub.docker.com/_/docker) — section *Docker in Docker*
- Pensez à lancer le daemon DIND en arrière-plan, puis à utiliser `docker exec` pour entrer dedans

**Livrables :**

- La commande exacte utilisée pour lancer le conteneur DIND
- La preuve qu'un `docker run ubuntu` fonctionne **depuis l'intérieur**
- Une capture ou explication montrant la différence avec la partie 4 (liste `docker ps` hôte vs conteneur DIND)

---

## Solution (corrigé)

<details>
<summary>Cliquez pour afficher la solution — essayez d'abord sans regarder</summary>

### Principe

La vraie solution DIND utilise l'image `docker:dind` avec le flag **`--privileged`**. Ce conteneur embarque **son propre daemon Docker** ; les conteneurs qu'il lance sont isolés dans son namespace, pas sur l'hôte.

### Étape 1 : lancer le daemon DIND

```bash
docker run -d --privileged --name dind docker:dind
```

Pourquoi `--privileged` ?

- Le daemon Docker interne a besoin de créer des namespaces, gérer `iptables`, monter des filesystems (`overlay2`…)
- Sans ce mode, le conteneur n'a pas les capacités kernel nécessaires → échec (partie 1)

### Étape 2 : entrer dans le conteneur DIND

```bash
docker exec -it dind sh
```

### Étape 3 : lancer Ubuntu depuis l'intérieur

```bash
docker run -it --rm ubuntu bash
```

### Étape 4 : vérifier l'isolation

**Dans le conteneur Ubuntu (ou depuis le shell DIND) :**

```bash
docker ps
```

**Sur l'hôte (autre terminal) :**

```bash
docker ps
```

Vous devez voir le conteneur `dind` sur l'hôte, mais **pas** le conteneur Ubuntu — celui-ci n'existe que dans le daemon interne du DIND.

> **Attention :** `--privileged` n'est **pas** une solution sans risque. Passez à la partie 6 avant de conclure que DIND est « sécurisé ».

</details>

---

## Partie 6 : Problèmes et conséquences de `--privileged`

La solution DIND avec `--privileged` améliore l'**isolation des conteneurs enfants** (ils ne polluent plus le daemon hôte), mais ouvre un **autre** vecteur d'attaque. Ne pas confondre : *mieux isolé pour les builds* ≠ *sans danger pour l'hôte*.

### Qu'est-ce que `--privileged` ?

Par défaut, Docker restreint ce qu'un conteneur peut faire au niveau kernel (capabilities, accès aux devices, montages sensibles…).

Avec `--privileged`, Docker **lève quasiment toutes ces restrictions** :

- accès à (presque) tous les **périphériques** de l'hôte (`/dev`)
- possibilité de manipuler **iptables**, le réseau, les cgroups
- montage de filesystems (`mount`, `overlay2`…)
- contournement des profils **AppArmor** / **SELinux** appliqués aux conteneurs classiques

> **Règle à retenir :** un conteneur `--privileged` compromis se rapproche d'un accès **quasi root sur l'hôte**, même sans bind du socket.

### Problème 1 : surface d'attaque kernel élargie

Un processus malveillant à l'intérieur du conteneur DIND peut tenter :

- des **fuites de conteneur** (*container escape*) via des failles kernel ou des syscalls autorisés par `--privileged`
- la manipulation du **stack réseau** de l'hôte (iptables, routes)
- l'accès à des devices matériels ou virtuels sensibles exposés via `/dev`

### Problème 2 : pas de sandbox forte pour le job CI

En CI/CD, un job qui tourne dans un DIND `--privileged` :

- peut compromettre **toute la machine runner** si le code exécuté est malveillant (PR fork, dépendance supply-chain…)
- hérite d'un niveau de confiance proche d'un processus root sur le VPS
- rend le principe « un job = un conteneur isolé » **beaucoup moins vrai** qu'il n'y paraît

### Problème 3 : `--privileged` ≠ moindre mal systématique

| Approche | Ce qu'on protège | Ce qu'on expose encore |
|----------|------------------|------------------------|
| Bind socket (`DooD`) | Rien sur l'hôte — accès direct au daemon | Contrôle total du daemon hôte |
| DIND `--privileged` | Daemon hôte (conteneurs enfants isolés) | Conteneur DIND lui-même = très puissant |
| Conteneur classique | Hôte (isolation standard) | Pas de Docker interne |

Les deux patterns DIND vus ici (**socket** et **`--privileged`**) ont des **failles de sécurité sérieuses**. Le choix dépend du **contexte de confiance**, pas d'une solution magique.

### Problème 4 : coût opérationnel et conformité

- Difficile à justifier face à un audit sécurité ou une politique « least privilege »
- Souvent **interdit** ou fortement restreint en production (PCI-DSS, environnements multi-tenant…)
- Consommation CPU/RAM plus élevée (daemon Docker embarqué)

### 🤔 Questions de réflexion

- Pourquoi GitLab CI documente-t-il DIND avec `--privileged` malgré les risques ?
- Dans un runner dédié à des builds de PR publics, que choisiriez-vous : socket, DIND privileged, ou autre chose ?
- `--privileged` améliore-t-il la sécurité par rapport au bind du socket, ou change-t-il seulement **où** se situe le risque ?

### Pistes d'atténuation (sans tout résoudre)

- **`--cap-add`** ciblé au lieu de `--privileged` complet (souvent insuffisant pour DIND, mais principe à connaître)
- Runners CI **éphémères** (une VM/jobs, puis destruction)
- Outils sans daemon Docker : **Kaniko**, **Buildah**, **Podman** (build d'images)
- Exécution dans une **VM** (firecracker, KVM…) pour les jobs non fiables
- Séparer les runners : jobs de confiance vs jobs non fiables sur des machines différentes
- **Runtimes alternatifs** — voir la partie 7

---

## Comparaison des deux premières approches

| Critère | Bind socket (`DooD`) | Vrai DIND (`docker:dind` + `--privileged`) |
|---------|----------------------|--------------------------------------------|
| Daemon utilisé | Celui de l'hôte | Daemon interne au conteneur |
| Isolation des conteneurs enfants | ❌ Aucune (tournent sur l'hôte) | ✅ Isolés dans le daemon interne |
| Risque pour l'hôte | ❌ Accès root via le socket | ❌ Conteneur DIND quasi root (`--privileged`) |
| Simplicité | ✅ Très simple | ⚠️ Plus lourd (ressources, config) |
| Acceptable en prod | ❌ Rarement | ❌ Rarement (sauf runners jetables dédiés) |
| Cas d'usage typique | Dev local, CI interne de confiance | CI isolée, builds reproductibles |

> **Conclusion :** ni le bind du socket, ni `--privileged` ne sont « sûrs » par défaut. On choisit le moindre mal selon le niveau de confiance du code exécuté. Existe-t-il une **troisième voie** ? — partie 7.

---

## Partie 7 : Une 3ᵉ solution ? Runtimes Docker et Sysbox

Vous avez exploré deux approches :

1. **Bind du socket** (`DooD`) — simple, mais accès root sur l'hôte
2. **`docker:dind` + `--privileged`** — isolation des conteneurs enfants, mais conteneur parent très privilégié

Les deux posent des problèmes de sécurité sérieux. Une piste souvent citée en CI/CD moderne consiste à ne pas accepter ce compromis tel quel, et à changer de **runtime de conteneur**.

### Rappel : Docker Engine ≠ container runtime

Docker Engine (CLI + daemon) ne crée pas les conteneurs directement. Il délègue à un **runtime OCI** — par défaut **`runc`**, via **containerd**.

Schéma simplifié :

```
docker CLI  →  docker daemon  →  containerd  →  runc  →  conteneur
```

Docker peut être configuré pour utiliser **d'autres runtimes** (fichier `/etc/docker/daemon.json`, clé `runtimes`). Chaque conteneur peut alors être lancé avec `--runtime <nom>`.

**Questions de départ (sans réponse imposée) :**

- Qu'est-ce qu'un runtime OCI ? Quelle est sa responsabilité par rapport au daemon Docker ?
- Quels runtimes Docker connaissez-vous en dehors de `runc` ?
- Comment lister ou vérifier le runtime utilisé par un conteneur en cours d'exécution ?

**Pistes de documentation :**

- [Docker — Configure container runtimes](https://docs.docker.com/engine/containers/run/#runtime)
- [Open Container Initiative (OCI)](https://opencontainers.org/)

### Sysbox : un runtime pensé pour l'imbrication

[**Sysbox**](https://github.com/nestybox/sysbox) est un runtime OCI (fork/evolution de concepts autour de `runc`) conçu pour exécuter des **workloads système** à l'intérieur de conteneurs — dont Docker lui-même — **sans** `--privileged` dans de nombreux cas.

L'idée générale : le conteneur reçoit un environnement plus « complet » (namespaces, `/dev`, cgroup…) tout en restant **mieux isolé** de l'hôte qu'un conteneur `--privileged` classique. Sysbox parle parfois de conteneur « incarnant une VM légère » (*lightweight VM*).

**À explorer par vous-mêmes :**

- [Documentation Sysbox](https://github.com/nestybox/sysbox/blob/master/docs/README.md)
- Quels cas d'usage Sysbox met-il en avant (DIND, systemd dans un conteneur, Kubernetes-in-Docker…) ?
- Comment s'installe-t-il sur une machine Linux (prérequis kernel, compatibilité avec Docker existant) ?
- Peut-on lancer `docker:dind` avec `--runtime=sysbox-runc` **sans** `--privileged` ?
- Que se passe-t-il si vous tentez la même chose avec le runtime par défaut (`runc`) ?

> **Note :** l'installation de Sysbox peut nécessiter des prérequis spécifiques (version kernel, configuration Docker…). Lisez la doc officielle avant de modifier votre VPS. Ce chapitre ne fournit **pas** de corrigé — c'est volontaire.

### 🎯 Défi ouvert : documenter une 3ᵉ approche

**Objectif :** investiguer si un runtime alternatif (Sysbox ou autre) constitue une vraie 3ᵉ solution au problème DIND, et sous quelles conditions.

**Travail demandé :**

1. **Comprendre** la chaîne Docker → containerd → runtime, et où Sysbox s'insère
2. **Lire** la documentation Sysbox (au minimum : introduction, installation, usage avec Docker)
3. **Tenter** (si votre environnement le permet) de lancer un conteneur DIND ou nested Docker via Sysbox — ou expliquer précisément **pourquoi** vous ne pouvez pas sur votre VPS
4. **Comparer** les 3 approches dans un tableau **que vous rédigez** :

| Critère | DooD (socket) | DIND `--privileged` | Runtime alternatif (ex. Sysbox) |
|---------|---------------|---------------------|----------------------------------|
| Isolation conteneurs enfants | ? | ? | ? |
| Risque pour l'hôte | ? | ? | ? |
| Complexité de mise en place | ? | ? | ? |
| `--privileged` requis ? | ? | ? | ? |
| Adapté à CI non fiable (PR public) | ? | ? | ? |

5. **Conclure** par un avis argumenté : cette 3ᵉ voie résout-elle vraiment le dilemme, ou déplace-t-elle encore le problème ?

**Questions ouvertes (pas de réponse fournie ici) :**

- Sysbox est-il une « solution miracle » ou un compromis différent ?
- Pourquoi n'est-il pas le runtime par défaut de Docker ?
- Quels autres runtimes ou outils pourraient jouer le même rôle (gVisor, Kata Containers, Firecracker…) ?
- Dans un pipeline CI réel, choisiriez-vous Sysbox, une VM jetable, Kaniko, ou autre chose — et **pourquoi** ?

**Livrables suggérés :**

- Notes de lecture sur la doc Sysbox (3 à 5 points clés)
- Commandes testées (ou blocage rencontré)
- Votre tableau comparatif des 3 approches
- Une conclusion personnelle — même si l'expérimentation n'a pas abouti

---

### Nettoyage (corrigé)

```bash
# depuis l'hôte
docker rm -f dind
```

### Aller plus loin (optionnel)

- Mode **TLS** entre client et daemon DIND : variable `DOCKER_TLS_CERTDIR=/certs`
- Pattern **sidecar** : un conteneur `docker:dind` + un conteneur `docker` (CLI) qui partagent un volume — utilisé dans GitLab CI
- **Kaniko**, **Buildah**, **Podman** pour builder des images sans daemon Docker

---

## Nettoyage final

À la fin de l'exercice :

1. Supprimez tous les conteneurs créés (`dind`, conteneurs de test…)
2. Vérifiez qu'aucun conteneur orphelin ne reste : `docker ps -a`
3. Confirmez que le VPS est propre

---

## Questions de réflexion finale

**Sur DIND vs DooD :**

- Quelle est la différence entre monter le socket et lancer `docker:dind` ?
- Pourquoi `--privileged` est-il requis pour DIND mais pas pour le bind du socket ?

**Sur la sécurité :**

- Dans un pipeline CI public (fork de PR), quelle approche choisiriez-vous ?
- Comment limiter les dégâts si un conteneur a quand même accès au socket ?
- `--privileged` protège-t-il l'hôte du même danger que le bind socket, ou déplace-t-il simplement le problème ?
- Pourquoi les deux approches (socket et privileged) sont-elles souvent **inacceptables** en production multi-tenant ?

**Sur la production :**

- Utiliseriez-vous DIND en production ? Dans quels cas oui / non ?
- Quelles alternatives existent pour builder des images sans Docker-in-Docker ?
- Un runtime comme Sysbox change-t-il votre réponse à la question précédente ?

---

## 🏆 Validation de vos connaissances

À la fin de cet exercice, vous devriez maîtriser :

1. ✅ **Expliquer** pourquoi `docker run docker:dind` seul échoue
2. ✅ **Comprendre** le rôle de `/var/run/docker.sock` et du bind mount
3. ✅ **Identifier** les risques sécurité du bind du socket **et** de `--privileged`
4. ✅ **Lancer** un conteneur via DooD (bind socket) puis via vrai DIND (`--privileged`)
5. ✅ **Comparer** isolation et risques entre les deux premières approches (sans les présenter comme « sûres »)
6. 🔍 **Investiguer** (partie 7) les runtimes Docker et Sysbox comme piste de 3ᵉ solution — question laissée ouverte

**Défi ultime :** pouvez-vous expliquer à un collègue pourquoi « Docker in Docker » recouvre en réalité **plusieurs** patterns (DooD, DIND privileged, runtimes alternatifs…) — et pourquoi aucun n'est trivial côté sécurité ?
