# Front-End | RShiny

## Installation de la bonne version R & Shiny

La version de **R** utilisé ici est la version `R Package Version` et la version de Shiny est la `Shiny Package Version`

Pour installer la dernière version de R

```bash
$ deb http://cloud.r-project.org/bin/linux/debian buster-cran40/
```

Pour installer la dernière version de Shiny, on compile depuis les sources :

```bash 
https://github.com/rstudio/shiny-server/wiki/Building-Shiny-Server-from-Source
```

## Installation des packages R

Pour une application golem, il est important de s'assurer que tous les packages sont installés et dans la bonne version

###  1. Préparation des outils nécessaires

Tous les packages nécessaires se trouvent dans le fichier `DESCRIPTION`, pour correctement installer les bonnes version vous pouvez utiliser le package `devtools`. Vous pouvez l'installer via cette commande : 

```R
install.packages("devtools")
```

Si vous venez à ajouter des packages ou autres configurations il sera important que tout soit spécifié dans le fichier `DESCRIPTION` qui est généré par *golem*

### 2. Installation des packages

Pour installer les dépendances listés vous pouvez utiliser `devtools::install_deps()` il installera tous les packages à la bonne version.

```R
devtools::install_deps()
```

### 3. Vérification de l'intégrité de l'application

Vous pouvez utiliser `devtools::check()` pour vérifier que tout est correctement installé et configuré

```R
devtools::check()
```

-----

# Back-End | Docker

Le docker contient les deux services, `backend_extenso-api_extenso` qui contient toute la partie API et `backend_extenso-db_extenso-1` qui contient la base de données et la gestion des connexions.

- Pré-requis : Configuration de l'environnement (voir [configuration env](environnement.md))

## Composition & Structure

    | 
    |-- backend_extenso/
    |   |-- Dockerfile
    |   |-- docker-compose.yml
    |   |-- .env
    |   |-- schema_extenso.sql
    |   `-- API/
    |         |-- app.py
    |         |-- db_extenso_omiv.py
    |         `-- parsing_scripts/
    |             |-- start.sh
    |             `-- [Scripts de parsing]
    |-- .gitlab-ci.yml
    |-- services_scripts/
    |   |-- kill.c
    |   |-- run.c
    |   `-- Makefile
    |-- run_services
    `-- kill_services

- `backend_extenso/` -> Contient tout le back-end de l'application
- `.gitlab-ci.yml` -> GitLab CI pour l'auto-déploiement
- `services_scripts/` -> Scripts pour le lancement du docker
- `run_services` -> Lancement de tous les services
- `kill_services` -> Arrêt des services (options disponibles)

## Configuration du docker

### Configuration du `docker-compose.yml`

Configuration de secret `ssh_key` pour la connexion sftp :

```
secrets:
  ssh_key:
    file: /home/user/.ssh/id_rsa <- Ajouter le chemin de la clé SSH
```

Configuration des ports :

- Service `db_extenso` : 

```
ports:
      - [Port souhaité (par défaut 5432)]:5432   
```

- Service `api_extenso`:

```
 ports:
      - [Port souhaité (par défaut 4444)]:4444
```
