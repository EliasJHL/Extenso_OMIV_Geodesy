## ggplot_cumul() - Graphique ggplot des séries temporelles

### Description

La fonction `ggplot_cumul` génère le graphiques pour des series temporelles. Elle permet de visualiser les décalages en Est, Nord et Up (coordonnées projetées) pour une liste de stations donnée en argument. Elle utilise le package `plotly`.

### Fonctionnalités

- Calcul et affichage des décalages dans les direction, Est, Nord et Up pour chaque station.

- Génération de graphiques interactifs avec `ggplot2` et `plotly`.

- Prise en chage du sous-échantillonnage des données lorsque la période de temps supérieure à deux mois.

### Paquets nécessaires

Pour utiliser cette fonction, assurez-vous d'avoir ces paquets installés :

```R
install.packages(c("ggplot2", "plotly", "dplyr", "lubridate", "cowplot"))
```

### Utilisation

#### 1. Paramètres

- `sta_code_list` : Une liste de codes de stations GNSS pour lesquelles les graphiques doivent être générés.
- `stations` : Un data.frame contenant des informations sur les stations, incluant les colonnes sta_code et station_id.
- `con` : Une connexion à la base de données contenant les séries temporelles des stations.
- `dr` : Une période de temps spécifiée sous la forme d'un vecteur c(start_date, end_date). Si end_date est manquant, la fin de l'année de start_date est utilisée.
- `plotly_output` : Un booléen ou une chaîne de caractères. Si TRUE, les graphiques sont générés avec plotly. Si "24h", les graphiques sont aussi générés avec plotly et affichent des ticks dynamiques. Par défaut, c'est TRUE.
- `sampling` : Une chaîne de caractères spécifiant l'intervalle d'échantillonnage des données. Par défaut, c'est "24h".

### 2. Résultat

![ggplot_cumul](img/plot_ggplot_cumul.png)