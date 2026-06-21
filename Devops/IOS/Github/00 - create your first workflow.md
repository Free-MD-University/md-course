# Exercice : Créer votre premier GitHub Workflow

Objectif : mettre en place un premier pipeline GitHub Actions, comprendre ce qu'est un runner `ubuntu-latest`, estimer ses coûts, et limiter la durée d'exécution d'un job.

Documentation officielle : [GitHub Actions — Quickstart](https://docs.github.com/en/actions/writing-workflows/quickstart)

---

## Partie 1 : Créer le workflow

Dans **votre projet** (repository GitHub), créez votre premier workflow GitHub Actions.

### Contraintes

- Le workflow doit se déclencher **à chaque push** sur le repository
- Le job doit s'exécuter sur un runner **`ubuntu-latest`**
- Le job doit exécuter la commande :

```bash
echo "hello-world"
```

### 🎯 Tâches

1. Créer le fichier de workflow au bon emplacement dans le repository (quel chemin ? quel nom de fichier ?)
2. Définir le **trigger** `push`
3. Définir un job avec `runs-on: ubuntu-latest`
4. Ajouter une step qui exécute `echo "hello-world"`
5. Commit, push, puis vérifier l'exécution dans l'onglet **Actions** de GitHub

**Questions :**

- Où voyez-vous les logs du job sur GitHub ?
- Le message `hello-world` apparaît-il bien dans les logs ?
- Quelle est la différence entre un **workflow**, un **job** et une **step** ?

**Indices :**

- Emplacement standard : `.github/workflows/`
- Extension du fichier : `.yml` ou `.yaml`
- Une step `run` exécute une commande shell dans le runner

---

## Partie 2 : Comprendre `ubuntu-latest`

Quand vous écrivez `runs-on: ubuntu-latest`, vous ne louez pas « une Ubuntu » vague — GitHub provisionne une **machine virtuelle éphémère** (runner hébergé par GitHub) avec des caractéristiques précises.

### Qu'est-ce que `ubuntu-latest` ?

- **`ubuntu-latest`** est un **label** (étiquette), pas une machine fixe
- Il pointe vers la **dernière image Ubuntu stable** maintenue par GitHub (actuellement Ubuntu 24.04 — vérifiez la doc à jour)
- L'image est recréée et mise à jour régulièrement par GitHub ([runner-images](https://github.com/actions/runner-images))
- Le runner est **jetable** : créé au début du job, détruit à la fin

> **Attention :** `-latest` évolue dans le temps. Pour des builds reproductibles, on pinne parfois `ubuntu-24.04` au lieu de `ubuntu-latest`.

### Capacités du runner (specs)

Les specs dépendent du **type de repository**. Consultez la [référence officielle des runners hébergés](https://docs.github.com/en/actions/reference/runners/github-hosted-runners).


**Questions à explorer :**

1. Votre projet est-il public ou privé ? Quelles specs s'appliquent à votre cas ?
2. Que contient l'image `ubuntu-latest` ? (langages, outils pré-installés) — consultez le readme de l'image Ubuntu 24.04 dans le repo `actions/runner-images`
3. Ces ressources sont-elles partagées ou dédiées à votre job ?
4. Que se passe-t-il si votre job dépasse les 14 GB de disque ?

### Schéma simplifié

```
push sur GitHub
      │
      ▼
GitHub Actions (orchestrateur)
      │
      ▼
Provisionne un runner ubuntu-latest (VM éphémère)
      │
      ▼
Exécute vos steps (echo "hello-world")
      │
      ▼
Runner détruit — environnement perdu
```

---

## Partie 3 : Coût d'un runner `ubuntu-latest`

### Repository public

- Les minutes GitHub Actions sur runners hébergés standard sont **gratuites** (usage raisonnable, sous réserve des [limites d'usage](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration))
- Vous payez surtout en **temps d'attente** si les jobs s'empilent (concurrence limitée)

### Repository privé

- Chaque plan GitHub inclut un quota de **minutes gratuites** par mois (Free, Pro, Team, Enterprise…)
- Au-delà du quota : facturation **à la minute** selon le type de runner et l'OS
- Linux consomme un multiplicateur ×1 ; Windows ×2 ; macOS ×10 (pour le calcul des minutes incluses)

**Questions :**

1. Combien de minutes gratuites inclut votre plan GitHub ?
2. Combien coûte une minute de runner Linux 2-core au-delà du quota ? (consultez [GitHub Actions billing](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions))
3. Votre job `echo "hello-world"` dure quelques secondes — combien coûte-t-il en pratique ?
4. Pourquoi un job macOS coûte beaucoup plus cher qu'un job `ubuntu-latest` ?

---

## Partie 4 : Limiter le runtime à 10 minutes

Par défaut, un job GitHub Actions peut tourner jusqu'à **360 minutes** (6 heures) avant d'être annulé automatiquement. C'est souvent trop long : un job bloqué consomme des minutes et retarde le feedback.

### 🎯 Tâche

Modifiez votre workflow pour que le job **ne puisse pas dépasser 10 minutes**. Si le job dépasse cette limite, GitHub doit l'**annuler automatiquement**.

**Questions :**

1. À quel **niveau** du fichier YAML applique-t-on cette limite : workflow, job, ou step ?
2. Quelle est la **clé YAML** à utiliser ?
3. Peut-on définir des timeouts différents par step ?
4. Quelle est la valeur **maximale** autorisée pour ce timeout ?

**Indices :**

- La limite se place au niveau du **job** (ou d'une step individuelle)
- Cherchez `timeout-minutes` dans la [syntaxe des workflows](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idtimeout-minutes)
- La valeur doit être un **entier positif** (pas de décimales)

**Validation :**

- [ ] Le workflow contient une limite de **10 minutes** au bon endroit
- [ ] Le job `echo "hello-world"` s'exécute toujours correctement
- [ ] Vous savez expliquer ce qui se passerait si une step faisait `sleep 700`

<details>
<summary>Indice supplémentaire (structure YAML)</summary>

La clé se place au **même niveau d'indentation** que `runs-on`, à l'intérieur de la définition du job :

```yaml
jobs:
  mon-job:
    runs-on: ubuntu-latest
    # ← ajouter la limite ici
    steps:
      - run: echo "hello-world"
```

</details>

---

## Nettoyage

Ce workflow est minimal — vous pouvez le garder comme base pour les chapitres suivants. Si vous testez des variantes, supprimez les workflows inutiles pour garder le repository lisible.

---

## Questions de réflexion finale

- Pourquoi GitHub détruit le runner après chaque job ?
- Quand préféreriez-vous un **self-hosted runner** plutôt que `ubuntu-latest` ?
- `ubuntu-latest` vs `ubuntu-24.04` : lequel choisissez-vous en CI de production, et pourquoi ?
- Pourquoi fixer un `timeout-minutes` bas (ex. 10 min) même sur un job qui dure 30 secondes ?

---

## 🏆 Validation de vos connaissances

À la fin de cet exercice, vous devriez maîtriser :

1. ✅ **Créer** un workflow déclenché au `push`
2. ✅ **Exécuter** un job sur `ubuntu-latest` avec une commande shell
3. ✅ **Expliquer** ce qu'est `ubuntu-latest` (label, specs, image jetable)
4. ✅ **Comparer** le coût public vs privé et estimer une consommation mensuelle
5. ✅ **Limiter** la durée d'un job à 10 minutes avec `timeout-minutes`
