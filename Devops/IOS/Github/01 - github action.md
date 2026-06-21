# Exercice : GitHub Actions — Runner macOS et setup-ruby

Objectif : créer un workflow sur runner macOS, comprendre l'action **checkout**, vérifier la présence de Ruby, installer Ruby via une **GitHub Action** tierce, afficher la version, changer de version avec `with`, et analyser les risques du tag `@latest`.

Prérequis : exercice [00 - create your first workflow](./00%20-%20create%20your%20first%20workflow.md)

Documentation :

- [GitHub Actions — Using actions in your workflow](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/using-actions-in-your-workflow)
- [actions/checkout](https://github.com/actions/checkout)
- [ruby/setup-ruby](https://github.com/ruby/setup-ruby)

---

## Partie 1 : Qu'est-ce qu'une GitHub Action ?

Jusqu'ici, vos steps utilisaient `run:` pour exécuter des commandes shell. Cet exercice introduit une autre kind de step : la **GitHub Action**.

### 🎯 Tâches

1. Lire la documentation GitHub Actions sur l'utilisation d'actions dans un workflow
2. Expliquer avec vos mots ce qu'est une GitHub Action
3. Identifier les différences entre une step `uses:` et une step `run:`
4. Décrire le rôle de `action.yml`, `@ref`, et `with:`

**Questions (répondre par écrit avant de continuer) :**

- Quelle est la différence entre une **action**, un **workflow** et un **job** ?
- Comment référence-t-on une action dans un fichier YAML ?
- Que signifie `@v1` dans `uses: ruby/setup-ruby@v1` ?

---

## Partie 2 : Workflow sur runner macOS

Créez un nouveau workflow dans votre projet qui s'exécute sur **macOS**.

### Contraintes

- Trigger : **`push`** (ou `workflow_dispatch` pour tester manuellement)
- Runner : **`macos-latest`** (ou une version pinée de votre choix)
- Timeout job : **10 minutes**

### 🎯 Tâches

1. Créer un fichier workflow au bon emplacement
2. Définir un job sur runner macOS
3. Commit, push, vérifier l'exécution dans l'onglet Actions

**Questions :**

- Quelle version macOS correspond à `macos-latest` aujourd'hui ?
- Quelles sont les specs du runner macOS (CPU, RAM) ?
- Pourquoi ce job coûte-t-il plus cher qu'un job `ubuntu-latest` sur un repo privé ?

---

## Partie 3 : L'action `actions/checkout`

Avant de parler de Ruby, la plupart des workflows commencent par récupérer le **code source du repository** sur le runner. L'action officielle pour cela est **[actions/checkout](https://github.com/actions/checkout)**.

### Ce que fait cette action

Quand GitHub Actions démarre un job, le runner est une machine **vierge** (ou plutôt : une image pré-configurée, mais **sans votre code**). Le workflow YAML lui-même est connu de GitHub, pas les fichiers de votre repo.

L'action `checkout` :

- **Clone** (ou fetch) le repository GitHub dans le répertoire de travail du job (`$GITHUB_WORKSPACE`)
- Positionne la branche, le tag ou le commit correspondant à l'événement qui a déclenché le workflow (push, PR, etc.)
- Rend vos fichiers disponibles pour les steps suivantes (`run:`, autres actions…)

Sans cette step, vos commandes shell tournent dans un dossier qui **ne contient pas** votre projet — seulement ce que l'image du runner fournit par défaut.

> C'est l'une des actions les plus utilisées sur GitHub Actions. Repérez-la dans les workflows open source que vous consultez.

### 🎯 Tâche : comparer avec et sans checkout

Dans votre workflow macOS, menez **deux expériences** et notez les résultats.

**Expérience A — sans checkout**

1. Ajouter une step qui **liste tous les fichiers** du répertoire de travail (commande de votre choix)
2. Lancer le workflow **sans** step `actions/checkout`
3. Copier la sortie des logs

**Expérience B — avec checkout**

1. Ajouter `actions/checkout` **avant** la step de listing (ref de l'action à piner consciemment)
2. Relancer la même commande de listing
3. Copier la sortie des logs

**Livrable :**

- Tableau ou note comparatif : **Expérience A** vs **Expérience B**
- Quels fichiers / dossiers apparaissent dans chaque cas ?
- Voyez-vous votre `README`, vos workflows `.github/workflows/`, le reste du repo ?

**Questions :**

- Pourquoi le runner ne contient-il pas automatiquement votre code ?
- À quoi sert la variable d'environnement `GITHUB_WORKSPACE` ?
- Pourquoi `checkout` est-il presque toujours la **première** step d'un job ?
- Quelle ref utilisez-vous pour `actions/checkout` et pourquoi ?
- Que se passe-t-il sur une pull request — quel commit est checkouté ? (consultez la doc)

Documentation : [actions/checkout](https://github.com/actions/checkout)

---

## Partie 4 : Vérifier si Ruby existe

Le workflow doit **tester** si Ruby est disponible sur le runner **avant** toute installation.

> À partir d'ici, votre workflow doit inclure `actions/checkout` — vous travaillez sur **votre** projet.

### 🎯 Tâches

1. Ajouter une step qui vérifie si Ruby est installé
2. Utiliser `ruby -v` pour voir si la commande retourne un résultat

**Indices :**

- Pensez aux commandes shell pour tester la présence d'un binaire
- GitHub Actions permet aussi des conditions au niveau d'une step

**Questions :**

- Quelle commande utilisez-vous pour vérifier si `ruby` est dans le `PATH` ?
- Ruby est-il déjà présent sur l'image macOS GitHub ? Comment le vérifier ?
- Le Ruby pré-installé sur le runner macOS vous convient-il pour un projet réel ? Justifiez.

---

## Partie 5 : Installer Ruby avec `ruby/setup-ruby`

Si Ruby est absent — ou si vous souhaitez imposer une version — utilisez l'action :

**[ruby/setup-ruby](https://github.com/ruby/setup-ruby)**

### 🎯 Tâches

1. Lire le README de l'action
2. Ajouter une step qui utilise `ruby/setup-ruby` (version de l'action à choisir consciemment)
3. Conditionner ou non l'installation selon votre logique de la partie 4 — justifiez votre choix
4. Ajouter une step finale : `ruby -v`
5. Vérifier le résultat dans les logs Actions

**Questions :**

- Que fait cette action concrètement par rapport à une installation manuelle (`brew`, etc.) ?
- Quelle référence `@...` choisissez-vous pour l'action et pourquoi ?
- Sur quelles plateformes l'action est-elle supportée ?

---

## Partie 6 : Changer la version Ruby avec `with`

L'action `setup-ruby` accepte des paramètres via le bloc `with:`.

### 🎯 Tâches

1. Faire tourner le workflow **sans** spécifier de version → noter la version obtenue via `ruby -v`
2. Modifier le workflow pour imposer **une autre version** que celle obtenue par défaut
3. Relancer et vérifier le changement dans les logs
4. (Optionnel) Tester la lecture automatique depuis un fichier de version à la racine du projet

**Questions (réponses à trouver dans la doc setup-ruby) :**

- Quel input YAML permet de fixer la version ?
- Quelles syntaxes de version sont acceptées ?
- Que se passe-t-il si vous ne passez aucune version et qu'aucun fichier de version n'existe ?
- Pourquoi certaines versions doivent-elles être quotées en YAML ?
- Comment tester plusieurs versions en parallèle ?

Documentation : [Supported Version Syntax](https://github.com/ruby/setup-ruby#supported-version-syntax)

---

## Partie 7 : Sécurité — le tag `@latest` et les tags mobiles

Observez des workflows existants (les vôtres, des repos open source, la doc GitHub).

### 🎯 Tâches

1. Repérer des usages de `@latest`, `@main`, `@master` ou d'autres refs non pinées
2. Auditer **votre** workflow : corrigez les refs d'actions si nécessaire
3. Rédiger une note (5–10 lignes) sur les risques des tags mobiles pour les GitHub Actions
4. Proposer **plusieurs** niveaux de durcissement (du plus souple au plus strict)

**Questions :**

- Quel est le problème d'utiliser `@latest` ou `@master` sur une action tierce ?
- En quoi est-ce différent — et parfois plus grave — qu'utiliser `macos-latest` comme runner ?
- Quels accès une action compromise peut-elle obtenir dans un workflow typique ?
- Comment piner une action sans bloquer toutes les mises à jour de sécurité ?
- Quels outils ou pratiques GitHub aident à maintenir les actions à jour de façon contrôlée ?

**Indices :**

- Comparez `@v1`, une release précise (`@v1.x.y`), et un SHA de commit
- Cherchez `permissions:` dans la syntaxe des workflows
- Lisez la doc GitHub sur la sécurité des actions et la supply chain

---

## Livrables

1. Note comparatif checkout **avec / sans** (partie 3)
2. Fichier workflow fonctionnel (checkout + macOS + vérification Ruby + setup-ruby + `ruby -v`)
3. Captures ou notes des logs Actions (version par défaut vs version imposée)
4. Note de sécurité sur le pin des actions (partie 7)
5. Réponses écrites aux questions marquées « répondre par écrit »

---

## Nettoyage

- Garder un workflow macOS/ruby propre et fonctionnel
- Supprimer les workflows de test temporaires
- Vérifier qu'aucune ref `@latest` / `@master` non justifiée ne reste dans `.github/workflows/`

---

## 🏆 Auto-évaluation

À la fin, vous devez être capable de :

1. Expliquer ce qu'est une GitHub Action et comment l'invoquer
2. Expliquer le rôle de `actions/checkout` et ce qu'il se passe sans elle
3. Lancer un job sur `macos-latest`
4. Vérifier puis installer Ruby via `ruby/setup-ruby`
5. Changer de version Ruby via `with:`
6. Justifier le choix de la ref (`@v1`, release, SHA) d'une action tierce
7. Argumenter les risques des tags mobiles sur les actions
