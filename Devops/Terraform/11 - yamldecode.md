on peut creer des locals selon des fichier: 
locals {
  # 1. Récupérer tous les fichiers YAML du dossier "configs"
  yaml_files = fileset(path.module, "configs/*.yaml")

  # 2. Double boucle 'for' pour aplatir et filtrer en même temps
  flattened_items = flatten([
    for filename in local.yaml_files : [
      for item in yamldecode(file("${path.module}/${filename}")).items : {
        # On crée une clé unique combinant le fichier et l'ID pour éviter les collisions
        unique_key  = "${filename}_${item.id}"
        id          = item.id
        name        = item.name
        environments = item.environments
      }
      # CONDITION : Uniquement si la liste contient "prod"
      if contains(item.environments, "prod")
    ]
  ])

  # 3. Transformation de la liste plate en Map (clé => valeur) pour le for_each
  resources_to_create = {
    for obj in local.flattened_items : obj.unique_key => obj
  }
}

et apres on fait notre bloc ressource

ressource "TYPE" "NAME" {
    count = local.resources_to_create
    parameter = parameter[1]...
}