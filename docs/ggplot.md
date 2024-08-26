## ggplot_serie_option() - Graphique ggplot des séries temporelles

### Description

La fonction `ggplot_serie_option` génère des graphiques pour les séries temporelles, permettant de visualiser les déplacements en Est, Nord et Up (ENU) pour une station selectionné, Elle propose l'affichage des données brutes.

## Fonctionnalités

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
- `option` : Un entier pour sélectionner les options d'affichage :
    - `1` : Affiche les données brutes.
    - `2` : Affiche la médiane glissante.
    - `4` : Affiche la régression linéaire.