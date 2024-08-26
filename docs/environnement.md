# Configuration de l'environnement

## Fichier `.env`

```
FLASK_PORT=[Port Flask] (4444) # Pas encore utilisé
FLASK_HOST=[Host Flask] (0.0.0.0) # Pas encore utilisé

POSTGRES_DB=[Base de données] (omiv-extenso)
POSTGRES_USER=[Utilisateur de la BDD] (omiv-extenso)
POSTGRES_PASSWORD=[Mot de passe de la BDD] (omiv-extenso)

SFTP_SERVER=[Host IP] (185.155.93.29)
SFTP_USER=[Utilisateur SFTP] (debian)
SFTP_IDENTITY_FILE_PATH=[Path du fichier SSH]

EXTENSO_MIN_DATE=[Date de début souhaité] (2022-06-15)
SOLEIL_MIN_DATE=[Date de début souhaité] (2022-06-15)
SOH_MIN_DATE=[Date de début souhaité] (2022-06-15)

EXTENSO_DATA_PATH=[Path sur le serveur distant où se trouvent les données extenso] (/home/debian/data/extenso)
ERROR_LOG_PATH=[Path du dossier log où seront stockés les rapports générés par le script de parsing] (./parsing_scripts/logs)
```

Le fichier environnement est utilisé par l'API et les scripts de parsing