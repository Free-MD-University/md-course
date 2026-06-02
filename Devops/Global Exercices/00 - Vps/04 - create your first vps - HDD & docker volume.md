# Exercice : Volumes Docker et Disques Durs Hetzner

Objectif : Comprendre le fonctionnement des volumes Docker, utiliser les disques durs Hetzner pour la persistance des données, et apprendre les concepts de montage et `/etc/fstab`.

## Partie 1 : Comprendre les volumes Docker

### Qu'est-ce qu'un volume Docker ?

Un volume Docker est un mécanisme de persistance des données qui permet de :
- Stocker des données en dehors du système de fichiers du conteneur
- Partager des données entre conteneurs
- Sauvegarder et restaurer des données
- Améliorer les performances I/O par rapport au stockage en couches

### Types de volumes :

1. **Named volumes** : Gérés par Docker (`docker volume create`)
2. **Bind mounts** : Pointent vers un répertoire de l'hôte (`-v /host/path:/container/path`)
3. **Tmpfs mounts** : Stockage temporaire en mémoire

### Questions à explorer :
- Où Docker stocke-t-il les named volumes sur le système ?
- Quelle est la différence entre un bind mount et un named volume ?
- Que se passe-t-il si plusieurs conteneurs utilisent le même volume ?

## Partie 2 : Créer le Docker de l'exercice précédent avec un volume

### Objectif
Reprendre le conteneur PostgreSQL de l'exercice précédent mais cette fois avec un volume pour persister les données.

### Commandes à utiliser :

`

### Tâches :
1. Créer le conteneur PostgreSQL avec un volume mounted
2. Importer le dump SQL de l'exercice précédent
3. Vérifier que les données sont présentes
4. Noter l'emplacement du volume sur le système hôte

## Partie 3 : Test de persistance - Supprimer et recréer le Docker


### Questions de validation :
- Les données sont-elles toujours présentes après suppression du conteneur ?
- Que se passe-t-il si vous supprimez aussi le volume ?
- Où sont stockées physiquement les données du volume ?

## Partie 4 : Introduction aux disques durs Hetzner

### Pourquoi utiliser un disque dur externe ?

Le disque local de la machine Hetzner :
- Est lié à l'instance VPS
- Sera perdu si l'instance est supprimée
- Ne peut pas être déplacé vers une autre instance

Un volume Hetzner :
- Peut survivre à la suppression d'une instance
- Peut être attaché à différentes instances
- Offre une sauvegarde et une flexibilité supplémentaires

### Étapes dans l'interface Hetzner :

1. **Créer un volume :**
   - Aller dans l'interface Hetzner Cloud
   - Section "Volumes"
   - Créer un nouveau volume (10 GB suffisent)
   - Choisir la même région que votre VPS

2. **Attacher le volume :**
   - Sélectionner votre VPS
   - Attacher le volume créé
   - Noter les commandes de montage fournies par Hetzner

## Partie 5 : Montage et configuration du disque dur

### Commandes typiques fournies par Hetzner :

```bash
# 1. Identifier le nouveau disque
lsblk

# 2. Formater le disque (ATTENTION : destructeur !)
mkfs.ext4 /dev/sdb

# 3. Créer un point de montage
mkdir /mnt/hc-volume

# 4. Monter temporairement
mount /dev/sdb /mnt/hc-volume

# 5. Vérifier le montage
df -h
```

### Explication de `/etc/fstab`

Le fichier `/etc/fstab` (file systems table) :
- Définit les systèmes de fichiers à monter au démarrage
- Format : `device mountpoint filesystem options dump pass`
- Exemple d'entrée pour notre volume :

```bash
# Éditer le fichier
nano /etc/fstab

# Ajouter une ligne comme :
/dev/sdb /mnt/hc-volume ext4 defaults 0 2
```

**Explication des colonnes :**
- `/dev/sdb` : périphérique à monter
- `/mnt/hc-volume` : point de montage
- `ext4` : type de système de fichiers
- `defaults` : options de montage (rw, suid, dev, exec, auto, nouser, async)
- `0` : dump (0 = pas de sauvegarde automatique)
- `2` : fsck order (2 = vérification après le système racine)

### Explication de la commande `mount`

La commande `mount` :
- Attache un système de fichiers à l'arbre de répertoires
- Syntaxe : `mount [options] device mountpoint`
- Options courantes :
  - `-t type` : spécifier le type de système de fichiers
  - `-o options` : options de montage (ro, rw, noexec, etc.)
  - `-a` : monter tous les systèmes dans `/etc/fstab`

```bash
# Exemples d'utilisation
mount /dev/sdb /mnt/hc-volume              # Montage simple
mount -t ext4 /dev/sdb /mnt/hc-volume     # Avec type explicite
mount -o ro /dev/sdb /mnt/hc-volume       # En lecture seule
umount /mnt/hc-volume                      # Démonter
```

## Partie 6 : Migration des données Docker

### Procédure de migration :

copier le chemin de votre instance postgres vers ce DD . recreer le docker mais binder le mount avec le chemin copier.

## Partie 7 : Test de résilience - Supprimer et recréer le VPS

supprimer le VPS et recreer le VPS de telle maniere a laisse rle volume . rebinder le volume et recreer un docker pour voir que les data sont presentes 

## Questions de réflexion et validation

### Sur les volumes Docker :
1. Quelle est la différence fondamentale entre un conteneur et ses données ?
2. Pourquoi utiliser un bind mount plutôt qu'un named volume ?
3. Que se passe-t-il si deux conteneurs montent le même volume en écriture ?

### Sur le montage de disques :
1. Que se passe-t-il si vous redémarrez le serveur sans `/etc/fstab` configuré ?
2. Quelle est la différence entre `mount` et `mount -a` ?
3. Comment vérifier qu'un disque est correctement monté ?

### Sur la persistance des données :
1. Quels sont les avantages d'utiliser un volume Hetzner vs le stockage local ?
2. Comment s'assurer que les données sont sauvegardées régulièrement ?
3. Que feriez-vous si le volume Hetzner devient inaccessible ?
