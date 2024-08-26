## ggplot_serie_option_2d() - Graphique ggplot 2D des séries temporelles

### Description

La fonction `ggplot_serie_option_2d` génère des graphiques 2D des séries temporelles. Elle permet de visualiser les décalages en Est, Nord et Up (ENU) pour les stations GNSS. Les options de visualisation incluent l'affichage des données brutes.

### Fonctionnalités

- **Affichage des données brutes** : Visualisation des données GNSS brutes en fonction de la date.

- **Visualisation interactive** : Affichage des graphiques avec `plotly` et `ggplot2`

### Installation

Pour utiliser cette fonction, assurez-vous d'avoir ces paquets installés :

```R
install.packages(c("ggplot2", "plotly", "zoo"))
```

### Utilisation

#### 1. Paramètres

- `s` : Un data.frame contenant les séries temporelles GNSS avec les colonnes date, e (Est), n (Nord), et u (Up).
- `option` : Un entier qui contrôle l'affichage des données :
    - `1` : Affiche les données brutes.
    - `2` : Affiche la médiane glissante.
    - `4` : Affiche la régression linéaire.
- `plotly_output` : Un booléen (TRUE par défaut) pour déterminer si les graphiques doivent être générés avec plotly pour l'interactivité ou avec ggplot2 pour un affichage statique.