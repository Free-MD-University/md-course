# Exercice : Docker + PostgreSQL (sans volume au debut)

Objectif : lancer un conteneur PostgreSQL, comprendre l'acces reseau avec `-p`, se connecter depuis le PC, importer un dump SQL local, puis observer ce qui se passe apres suppression/recreation du conteneur.

## 1) Lancer PostgreSQL avec un seul `docker run`

Contraintes :

- utiliser un seul `docker run`
- ne pas utiliser `-v`
- ne pas utiliser `-p` dans cette premiere tentative
- nommer le conteneur avec `--name postgres`

Questions :

- Quelles variables d'environnement sont necessaires pour initialiser PostgreSQL ?
- Quelle image choisis-tu (tag inclus) et pourquoi ?

## 2) Entrer dans le conteneur et tester PostgreSQL

Creer un `docker exec` interactif pour entrer dans le conteneur PostgreSQL.

Tu peux partir de cette idee (a corriger si besoin) :

```bash
docker exec -it posgres /bin/bash
```

Puis, une fois dans le conteneur :

- trouver le bon shell si besoin
- lancer le client `psql`
- executer quelques commandes SQL simples (creation, listing, select de verification)

## 3) Connexion depuis ton PC : comprendre `-p`

Essayer de te connecter a cette instance PostgreSQL depuis ton PC (DataGrip ou autre SQL manager).

Questions :

- Pourquoi la connexion echoue dans la premiere version du conteneur ?
- A quoi sert exactement l'option `-p` de Docker ?
- Quelle est la difference entre le port expose dans le conteneur et le port publie sur l'hote ?

## 4) Recreer l'instance avec `-p`

Supprimer puis recreer le conteneur pour publier le port PostgreSQL avec `-p`.

Ensuite :

- se connecter depuis DataGrip (ou autre outil SQL)
- verifier que la connexion distante fonctionne

## 5) Creer la base `FRENCH` et importer un dump local

Dans PostgreSQL :

- creer une base nommee `FRENCH`

Importer ensuite le dump SQL **depuis ta machine locale uniquement** :

- source SQL : [french-towns-communes-francaises.sql](https://github.com/morenoh149/postgresDBSamples/blob/master/french-towns-communes-francaises/french-towns-communes-francaises.sql)
- verifier que les tables ont bien ete creees
- verifier qu'il y a des donnees (au moins une requete de comptage)

## 6) Supprimer le conteneur puis le recreer

Supprimer le conteneur avec une commande de suppression forcee, puis recreer un nouveau conteneur PostgreSQL.
```
docker -rf 
```

Question finale :

- Retrouves-tu la base `FRENCH` apres recreation ?
- Si non, ou sont passees les donnees ?
- Explique le lien avec l'absence de `-v` (volume persistant).

## Validation attendue

A la fin, tu dois etre capable d'expliquer :

- la difference entre conteneur et donnees persistantes
- l'utilite de `docker exec`
- l'utilite de `-p` pour les connexions depuis l'exterieur
- pourquoi les donnees disparaissent apres suppression du conteneur sans volume
