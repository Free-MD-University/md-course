# Exercice : Réseaux Docker - Communication inter-conteneurs

Objectif : Comprendre le fonctionnement des réseaux Docker, la résolution DNS, et la communication entre conteneurs sur différents réseaux.

## Partie 1 : Découverte des réseaux Docker

### Qu'est-ce qu'un réseau Docker ?

Un réseau Docker permet aux conteneurs de communiquer entre eux de manière isolée et sécurisée. Docker crée un environnement réseau virtualisé où chaque conteneur peut avoir sa propre adresse IP.

### Types de réseaux Docker :

1. **bridge** (par défaut) : Réseau privé isolé sur l'hôte
2. **host** : Utilise directement le réseau de l'hôte
3. **none** : Aucun accès réseau
4. **overlay** : Pour les clusters multi-hôtes
5. **macvlan** : Assigne une adresse MAC au conteneur

### 🎯 Tâches de découverte :

**À vous de jouer !** Trouvez les commandes pour :

1. **Lister tous les réseaux Docker existants**
   - Combien de réseaux sont créés par défaut ?
   - Quels sont leurs noms ?

2. **Inspecter le réseau 'bridge' par défaut**
   - Quelle est la plage d'adresses IP utilisée ?
   - Y a-t-il des conteneurs déjà connectés ?
   - Quelle est l'adresse de la passerelle ?

## Partie 2 : Créer votre premier réseau personnalisé

### 🎯 Tâches à accomplir :

1. **Créer un réseau bridge personnalisé** nommé `mon-reseau-test`
   - Utilisez le driver bridge
   - Vérifiez que le réseau a bien été créé

2. **Comparer les configurations**
   - Inspectez votre nouveau réseau
   - Comparez la configuration avec le réseau bridge par défaut
   - Quelles sont les différences de plages d'IP ?

3. **Défi bonus :** Créer un réseau avec une configuration personnalisée
   - Subnet : 192.168.100.0/24
   - Gateway : 192.168.100.1
   - Nom : `reseau-custom`

## Partie 3 : Créer les conteneurs Ubuntu

### 🎯 Objectifs :

1. **Créer deux conteneurs Ubuntu :**
   - `ubuntu-1` connecté à votre réseau `mon-reseau-test`
   - `ubuntu-2` connecté au même réseau
   - Les deux doivent tourner en mode détaché et interactif

2. **Installer les outils nécessaires dans chaque conteneur :**
   - `openssh-server` (pour SSH)
   - `iputils-ping` (pour ping)
   - `net-tools` (pour netstat)
   - `dnsutils` (pour nslookup, dig)

**Indices :**
- Mode détaché + interactif : option `-dit`
- Installation : pensez à `apt update` d'abord
- Exécution dans conteneur : commande `docker exec`

3. **Configurer SSH sur ubuntu-2 :**
   - Démarrer le service SSH
   - Changer le mot de passe root vers `monmotdepasse123`
   - Configurer SSH pour permettre la connexion root avec mot de passe
   - Vérifier que SSH écoute sur le port 22

**Indices :**
- Configuration SSH : fichier `/etc/ssh/sshd_config`
- Paramètres à modifier : `PermitRootLogin` et `PasswordAuthentication`
- Changer mot de passe : commande `chpasswd`

4. **Obtenir les adresses IP de chaque conteneur**
   - Trouvez l'IP d'ubuntu-1
   - Trouvez l'IP d'ubuntu-2
   - Listez toutes les connexions réseau de votre réseau personnalisé

## Partie 4 : Tests de connectivité et DNS

### 🎯 Expérimentations à mener :

1. **Test de connectivité par adresse IP**
   - Depuis ubuntu-1, envoyez 3 pings vers ubuntu-2 en utilisant son adresse IP
   - Le ping fonctionne-t-il ? Pourquoi ?

2. **Test de résolution DNS par nom**
   - Depuis ubuntu-1, envoyez 3 pings vers ubuntu-2 en utilisant son **nom** `ubuntu-2`
   - Le ping fonctionne-t-il ?
   - Vérifiez la résolution DNS avec `nslookup`
   - Examinez les fichiers `/etc/hosts` et `/etc/resolv.conf` d'ubuntu-1

3. **Connexion SSH entre conteneurs**
   - Connectez-vous depuis ubuntu-1 vers ubuntu-2 via SSH en utilisant l'adresse IP
   - Connectez-vous depuis ubuntu-1 vers ubuntu-2 via SSH en utilisant le nom DNS
   - Mot de passe : `monmotdepasse123`

### 🤔 Questions de réflexion DNS :

Après vos tests, répondez à ces questions :
- **Comment fonctionne la résolution DNS dans Docker ?**
- **Sur quelle adresse IP le serveur DNS Docker écoute-t-il ?**
- **Que se passe-t-il si vous utilisez le réseau bridge par défaut au lieu d'un réseau personnalisé ?**
- **Les noms de conteneurs sont-ils automatiquement des entrées DNS ?**

**Défi :** Utilisez la commande `dig` pour interroger directement le serveur DNS Docker.

## Partie 5 : Isolation réseau et multi-réseaux

### 🎯 Expériences d'isolation :

1. **Créer un second réseau isolé**
   - Nom : `reseau-isole`
   - Driver : bridge
   - Créer un troisième conteneur `ubuntu-3` sur ce réseau uniquement
   - Installer les outils de base (ping, net-tools, dnsutils)

2. **Tester l'isolation**
   - Depuis ubuntu-1, essayez de pinguer ubuntu-3 par nom
   - Depuis ubuntu-1, essayez de pinguer ubuntu-3 par IP
   - **Résultat attendu :** Les pings doivent échouer. Pourquoi ?

3. **Connecter un conteneur à plusieurs réseaux**
   - Connectez ubuntu-1 au réseau `reseau-isole` (en plus de `mon-reseau-test`)
   - Vérifiez les connexions réseau d'ubuntu-1
   - Testez maintenant : ubuntu-1 peut-il pinguer ubuntu-3 ?
   - Examinez les interfaces réseau et les routes d'ubuntu-1

### 🎯 Scénarios de validation :

Testez ces scénarios et expliquez les résultats :

1. **Ubuntu-1 peut-il communiquer avec ubuntu-2 ET ubuntu-3 ?**
2. **Ubuntu-2 peut-il communiquer avec ubuntu-3 ?** (doit échouer)
3. **La résolution DNS fonctionne-t-elle sur tous les réseaux ?**

**Expérience avancée :** Déconnectez ubuntu-1 du `reseau-isole` et vérifiez qu'il ne peut plus atteindre ubuntu-3.

## Partie 6 : Communication avec la machine hôte

### 🎯 Objectif : Comprendre le binding de ports et l'accès à l'hôte

Dans cette section, nous allons explorer comment les conteneurs peuvent :
1. **Exposer des services** vers la machine hôte (et donc vers l'extérieur)
2. **Accéder à des services** qui tournent sur l'hôte
3. **Communiquer entre réseaux** via l'hôte

### 🔧 Exercice pratique : Nginx et communication via l'hôte

#### Étape 1 : Créer deux réseaux séparés

**Votre mission :**
1. Créez deux réseaux Docker complètement isolés :
   - `reseau-web` pour le serveur nginx
   - `reseau-client` pour le conteneur client

2. Vérifiez l'isolation entre ces réseaux (les conteneurs ne peuvent pas se voir directement)

#### Étape 2 : Conteneur nginx avec binding de port

**À réaliser :**
1. **Créer un conteneur nginx** :
   - Nom : `nginx-server`
   - Réseau : `reseau-web`
   - **Port binding** : Exposez le port 80 du conteneur vers le port 8080 de l'hôte
   - Image : `nginx:latest`

2. **Personnaliser la page nginx** :
   - Remplacez la page par défaut par votre propre contenu
   - Affichez un message comme "Hello from nginx-server on reseau-web"
   - **Indice :** Le fichier par défaut nginx est dans `/usr/share/nginx/html/index.html`

3. **Vérifications à effectuer :**
   - Le serveur nginx répond-il sur `http://localhost:8080` depuis votre machine hôte ?
   - Pouvez-vous voir votre message personnalisé ?

#### Étape 3 : Conteneur client sur réseau séparé

**Votre défi :**
1. **Créer un conteneur ubuntu client** :
   - Nom : `ubuntu-client`
   - Réseau : `reseau-client` (différent du serveur nginx)
   - Installer `curl` et les outils réseau nécessaires

2. **Tests de connectivité directe** (qui doivent échouer) :
   - Depuis `ubuntu-client`, essayez de pinguer `nginx-server` par nom
   - Depuis `ubuntu-client`, essayez d'accéder à nginx par son IP interne
   - **Résultat attendu :** Échec total (isolation réseau)

3. **Accès via l'hôte** (qui doit fonctionner) :
   - Depuis `ubuntu-client`, accédez au serveur nginx en passant par l'hôte
   - **Questions à résoudre :**
     - Quelle IP utiliser pour accéder à l'hôte depuis le conteneur ?
     - Sur quel port faire la requête ?
     - La page nginx s'affiche-t-elle correctement ?

#### Étape 4 : Exploration des adresses hôte

**Découvertes à faire :**
1. **Identifier l'adresse de l'hôte** vue depuis les conteneurs :
   - Examinez les routes réseau dans chaque conteneur
   - Trouvez l'IP de la passerelle (gateway)
   - **Question :** Cette IP est-elle la même dans les deux réseaux ?

2. **Tests de connectivité vers l'hôte :**
   - Depuis chaque conteneur, pingez l'IP de l'hôte
   - Vérifiez que les deux conteneurs peuvent atteindre l'hôte
   - Testez l'accès au port 8080 de l'hôte depuis les deux conteneurs

**Indices pour trouver l'IP hôte :**
- Commande pour voir les routes : `ip route`
- Commande pour voir les passerelles : `ip route show default`
- L'hôte Docker est généralement accessible via l'IP de la passerelle du réseau bridge

#### Étape 5 : Expérimentations avancées

**Défis supplémentaires :**

1. **Service discovery via l'hôte :**
   - Créez un deuxième service nginx sur un port différent (ex: 8081)
   - Le conteneur client peut-il accéder aux deux services via l'hôte ?

2. **Binding multiple :**
   - Bindez le même conteneur nginx sur plusieurs ports hôte (8080, 8090)
   - Vérifiez l'accès depuis différents conteneurs clients

3. **Test réseau host :**
   - Créez un conteneur avec `--network host`
   - Comment ce conteneur voit-il les autres conteneurs ?
   - Peut-il accéder directement aux services sur l'hôte ?

### 🤔 Questions de compréhension

Après avoir terminé l'exercice, réfléchissez à :

1. **Port binding :**
   - Quelle est la différence entre `EXPOSE` et le flag `-p` ?
   - Que se passe-t-il si deux conteneurs tentent de binder le même port hôte ?
   - Comment lister tous les ports bindés sur l'hôte ?

2. **Accès à l'hôte :**
   - Pourquoi l'IP de l'hôte vue depuis le conteneur n'est-elle pas toujours 127.0.0.1 ?
   - Comment un conteneur peut-il accéder à un service qui tourne sur l'hôte ?
   - Quels sont les risques de sécurité du network host ?

3. **Patterns d'architecture :**
   - Quand utiliser le binding de ports vs la communication inter-conteneurs ?
   - Comment concevoir une architecture où des services isolés communiquent via l'hôte ?
   - Quels sont les avantages/inconvénients de cette approche ?

## Partie 7 : Défis finaux et nettoyage

### 🏆 Défi architectural complet

Créez cette architecture complexe :
- **3 réseaux :** `frontend`, `backend`, `database`
- **4 conteneurs :** `web`, `api`, `cache`, `db`
- **1 service nginx** exposé sur l'hôte (port 8080)
- **Règles de connectivité :**
  - `web` uniquement sur `frontend`, accessible via l'hôte
  - `api` sur `frontend` ET `backend`
  - `cache` et `db` uniquement sur `backend`
  - Tous les services peuvent accéder à nginx via l'hôte

**Validations finales :**
- `web` ne peut PAS atteindre directement `db`
- `web` PEUT atteindre `api`
- `api` PEUT atteindre `db`
- Tous les conteneurs peuvent accéder à nginx via l'IP hôte:8080
- nginx est accessible depuis votre navigateur

### 🧹 Nettoyage complet

À la fin de l'exercice :
1. Arrêtez et supprimez tous les conteneurs créés
2. Supprimez tous les réseaux personnalisés créés
3. Vérifiez que tous les ports bindés sont libérés
4. Confirmez que le nettoyage est complet

## Questions de réflexion finale

### 🤔 Après avoir terminé l'exercice, réfléchissez à ces questions :

**Sur le DNS Docker :**
- Pourquoi le réseau bridge par défaut ne supporte-t-il pas la résolution DNS par nom ?
- Comment Docker maintient-il la cohérence DNS quand des conteneurs sont ajoutés/supprimés ?
- Que se passe-t-il si deux conteneurs ont le même nom sur des réseaux différents ?

**Sur la sécurité réseau :**
- Comment empêcher complètement la communication entre deux groupes de conteneurs ?
- Peut-on limiter la bande passante entre conteneurs ?
- Quels sont les risques de sécurité avec les multi-réseaux ?

**Sur l'architecture :**
- Quels sont les cas d'usage pratiques d'un conteneur sur plusieurs réseaux ?
- Comment concevoir une architecture microservices avec les réseaux Docker ?
- Comment gérer les routes quand un conteneur a plusieurs interfaces réseau ?

## Cas d'usage à explorer (optionel)

Si vous voulez aller plus loin, essayez de créer ces architectures :

**Scénario 1 : E-commerce**
- Réseau `dmz` : reverse proxy
- Réseau `web` : frontend + API
- Réseau `data` : API + base de données + cache
- Le frontend ne doit jamais accéder directement aux données

**Scénario 2 : Environnements multiples**
- Créez des réseaux séparés pour `dev`, `test`, `staging`
- Chaque environnement doit être complètement isolé
- Comment gérer les services partagés (monitoring, logging) ? 

## 🏆 Validation de vos connaissances

### À la fin de cet exercice, vous devriez maîtriser :

1. ✅ **Créer et gérer** des réseaux Docker personnalisés
2. ✅ **Comprendre** le fonctionnement du DNS intégré Docker
3. ✅ **Isoler** des conteneurs sur différents réseaux
4. ✅ **Connecter** un conteneur à plusieurs réseaux simultanément
5. ✅ **Diagnostiquer** les problèmes de connectivité réseau
6. ✅ **Concevoir** une architecture réseau sécurisée

### 🎯 Auto-évaluation :

**Pouvez-vous expliquer :**
- La différence entre le réseau bridge par défaut et un réseau personnalisé ?
- Pourquoi la résolution DNS fonctionne différemment selon le type de réseau ?
- Comment isoler complètement des groupes de conteneurs ?
- Les avantages et inconvénients de connecter un conteneur à plusieurs réseaux ?

**Pouvez-vous réaliser :**
- Créer un réseau avec une configuration IP personnalisée ?
- Diagnostiquer un problème de connectivité entre conteneurs ?
- Concevoir une architecture multi-tiers avec isolation réseau ?
- Migrer des conteneurs d'un réseau vers un autre sans interruption ?

### 📝 Livrables suggérés :

1. **Schéma de votre architecture finale** (frontend/backend/database)
2. **Log des tests de connectivité** prouvant l'isolation
3. **Liste des commandes découvertes** et leur utilité
4. **Document de troubleshooting** avec les problèmes rencontrés et solutions

**Défi ultime :** Pouvez-vous créer cette architecture et la valider sans regarder aucune documentation ?