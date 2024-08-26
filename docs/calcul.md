## Module de calcul Python

Répertoire GIT : `git clone https://gitlab.com/. . .`

Le module python, stocké sur le côté serveur a pour but de correctement traiter les données, il permet notamment de télécharger, convertir, calculer et stocker les données reçues.

## Installation

Pour fonctionner, ce programme nécessite :

```TXT
# Fichier requirements.txt
pandas==1.3.3
PyYAML==5.4.1
Unidecode==1.3.1
paramiko
pymap3d
psycopg2
requests
pyproj
pyOpenSSL
beautifulsoup4
Flask
```

On peut installer les paquets via : 

```bash
$ pip3 install -r requirements.txt 
```