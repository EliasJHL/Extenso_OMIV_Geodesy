# Base de données (Développeur)

## Utilisation & Mise à jour des modèles

La base de données se base sur les modèles (dans /api_service/models/) pour créer son schèma grâce à `flask-migrate` qui va modifier et suivre les modifications au fur et à mesure qu'on modifie les models, pour lancer la mise à jour de la base de données il y un script bash `update_base.sh` qui va lancer `flask-migrate`.

S'il y a un soucis avec la migration :

- Supprimer toutes les tables (stations en dernière)
- Relancer le script de migration
- Lancer le script de parsing

```

```