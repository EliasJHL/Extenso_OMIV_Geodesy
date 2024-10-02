## Déploiement & Configuration

### 1. 

La base de données se base sur les modèles (dans /api_service/models/) pour créer son schèma grâce à `flask-migrate` qui va modifier et suivre les modifications au fur et à mesure qu'on modifie les models, pour lancer la mise à jour de la base de données il y un script bash `update_base.sh` qui va lancer `flask-migrate`.

## Module de visualisation Shiny

Répertoire GIT : `git clone https://gitlab.com/. . .`

Le module shiny utilisé est `shinydashboard` qui permet d'avoir l'UI d'un dashboard en utilisant le package `shiny`

L'application actuelle se trouve dans le dossier tacheo/ qui est accessible dans le lien de test développeur `http://omiv-geodesy.u-strasbg.fr/dev/tacheo/`

### Utilisation

On lance, redémarre ou stoppe le serveur Shiny via la commande

```bash
$ sudo service shiny-server [start|restart|stop]
```

Les logs des application sont accessibles dans `/var/log/shiny-server/. . .`

Il y a un fichier par application et par session. Cela signifie qu'à chaque fois qu'un nouvel utilisateur accède à l'application, un nouveau fichier est créé. On cherche donc le fichier qui comporte la date et l'heure correspondante. Exemple : 

```bash
$ sudo tail /var/log/shiny-server/omiv-geodesy-dev/tacheo/tacheo-shiny-20240730-125544-41713.log -n 100
```

En cas de bug dans l'application, on cherche dans les logs un éventuelle message d'erreur généré par Shiny (librarie non trouvée, fichier non accessible, . . .).

### Éléments du code

Les fichier nécessaires à l'application Shiny sont :

- **global.R** : Contient les variables globales (notamment les stations par défaut).

- **app_ui.R** : Base de l'UI shiny.

- **mod_dashboard_body.R** : Module contenant l'organisation des éléments UI.

- **mod_dashboard_sidebar.R** : Module contenant la sidebar.

- **mod_dashboard_[tab].R** : Un module par tab contenant le nécessaire par page.

Ensuite, on a extérnalisé un certain nombre de fonction pour la clarté du code :

- **fct_ggplot.R** : Fonctions qui permettent l'affichage des graphiques ggplot.

- **fct_plot_heatmap.R** : Fonctions qui permettent l'affichage des heatmap.

- **fct_functions.R** : Contient les fonctions utilisés dans le calcul des séries temporelles, manipulation de la liste des stations. 

- **fct_api.R** : Fonctions qui permettent l'intéraction avec l'API et l'application.

### 1. Home

#### 1.1 Raccourcis

La page "Home" contient deux boutons qui redirigent sur les services considérés importants

![home_btn](img/home_btn.png)

#### 1.2 Carte des stations

Ainsi qu'un carte montrant l'ensemble des stations disponibles sans aucun filtre

![home_map](img/home_map.png)

### 2. Plot sites

#### 2.1 La liste des sites

Sur cette page l'utilisateur peut choisir via un bouton par site disponible avec l'image de la station (si disponible)

![plot_bouton](img/plot_bouton.png)

Une fois la station séléctionné, on arrive sur les détails des stations qui appartienent au site avec si disponible la carte définie dans le fichier de config `tiles_config.json`

```JSON
{
  "default": {
    "url_tiles": "https://data.geopf.fr/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=ORTHOIMAGERY.ORTHOPHOTOS&STYLE=normal&FORMAT=image/jpeg&TILEMATRIXSET=PM&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}",
    "config": {
      "max_zoom": 22,
      "infos": "Orthophoto IGN"
    }
  },
  "Viella": {
    "coords": {
      "x": 42.8766878,
      "y": 0.0137833
    },
    "url_tiles": "http://omiv-geodesy.u-strasbg.fr/geoserver/onf_rtm/gwc/service/wmts?layer=onf_rtm:ortho_lidar_2019&style=&tilematrixset=WebMercatorQuad&Service=WMTS&Request=GetTile&Version=1.0.0&Format=image/jpeg&TileMatrix={z}&TileCol={x}&TileRow={y}",
    "config": {
      "max_zoom": 22,
      "infos": "Orthophoto 2019 - Viella"
    }
  }
}
```

Dans le cas du site Viella, on a des tuiles personnalisés importés depuis un geoserver.

![plot_map](img/plot_map.png)

#### 2.2 Le tree view

On a également le Tree View qui permet de séléctionner les stations à afficher dans la carte et dans les graphiques ggplot :

![plot_tree_view](img/plot_tree_view.png)

**Configuration des stations selectionnées au lancement de l'application :**

La liste des stations par défaut se situe dans le fichier `global.R` :

```R
default_viella <- c('VIL-I10', 'VIL-05', 'VIL-02', 'CAH-I8', 'EBO-03', 'BAV-PL34', 'BAV-04',
                    'BAV-I4', 'HER-02', 'HER-04', 'VIL-11', 'VIL-I2', 'MID-I1', 'MID-PZ11', 'MID-02')
```
#### 2.3 Les graphiques ggplot

La plage des données à afficher sur le ggplot est récupéré sur le fichier `mod_dashboard_sidebar.R` et attribué dans une valeur réactive `data_range()`

Concernant les stations selectionnées, la liste est également stocké dans une liste réactive `station_i()`

![plot_ggplot_cumul](img/plot_ggplot_cumul.png)


### 3. Status

#### 3.1 Choix de la station

Il y a un *selectinput* qui permet de selectionner une station parims les groupes de stations du site choisi par l'utilisateur dans la value réactive `selectedSite()`

![status_select](img/status_select.png)

#### 3.2 Les heatmap

En fonction des stations disponibles une box contenant une heatmap par station.

![status_heatmap](img/status_heatmap.png)

### 4. Download

#### 4.1 Connexion API

Méthode HTTP : **GET**

Quand il est appelé il récupére les arguments données dans le lien pour faire en sorte de créer une requête à la base de données et renvoyer un fichier .zip contenant un fichier .csv par station.

Lien de l'API : 
```txt
http://omiv-geodesy.u-strasbg.fr/rest/api/download/2/[site]/[code epsg]?from=[date début]&to=[date fin]&sta_list=[ID des stations]
```

**Le code EPSG par défaut est le 2154, il est stocké dans la base de données**

Exemple d'utilisation :

```txt
# Récupération des données de la station 10 et 148 dans la plage de date du 24 Juin 2024 au 24 Juillet 2024

http://omiv-geodesy.u-strasbg.fr/rest/api/download/2/Viella/2154?from=2024-06-24&to=2024-07-24&sta_list=10,148
```

Données renvoyés par l'API :

```csv
date_utc,x(ecef),y(ecef),z(ecef),lat,lon,h,Easting(L93),Northig(L93)
2024-06-24 00:42:39,4682187.1300,1749.7616,4318021.0123,42.874777100,0.021411778,1034.1317,456438.5620,6201853.4667
2024-06-24 01:42:34,4682187.1308,1749.7604,4318021.0114,42.874777090,0.021411763,1034.1317,456438.5607,6201853.4656
2024-06-24 02:42:24,4682187.1309,1749.7614,4318021.0118,42.874777092,0.021411776,1034.1320,456438.5618,6201853.4658
```

---

![download](img/download.png)
