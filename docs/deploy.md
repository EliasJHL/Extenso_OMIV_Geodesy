## Déploiement & Configuration

### 1. 

La base de données se base sur les modèles (dans /api_service/models/) pour créer son schèma grâce à `flask-migrate` qui va modifier et suivre les modifications au fur et à mesure qu'on modifie les models, pour lancer la mise à jour de la base de données il y un script bash `update_base.sh` qui va lancer `flask-migrate`.
