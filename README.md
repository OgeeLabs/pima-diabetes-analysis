# Prédiction du diabète chez les femmes pimas : comparaison avec le modèle ADAP (1988)

Analyse du jeu de données **Pima Indians Diabetes** et comparaison de deux modèles d'apprentissage automatique (forêt aléatoire et régression logistique) avec le réseau de neurones **ADAP** de l'article original de Smith et al. (1988).

## Contexte

Les Pimas, une population amérindienne de l'Arizona, ont l'un des taux de diabète de type 2 les plus élevés au monde. Ils sont suivis depuis 1965 par le NIDDK (*National Institute of Diabetes and Digestive and Kidney Diseases*).

En 1988, Smith et al. ont utilisé ADAP, un réseau de neurones inspiré du perceptron, pour prédire la **survenue d'un diabète dans les 1 à 5 ans** suivant un examen. Entraîné sur 576 patientes et testé sur 192, ADAP atteignait une **sensibilité et une spécificité de 76 %** au point où les deux sont égales.

**Question de ce projet :** à la fin de leur article, les auteurs annonçaient comparer ADAP à une régression logistique et à un perceptron linéaire sur les mêmes données, mais les résultats de cette comparaison n'y figurent pas. Ce projet tente d'y répondre en cherchant à savoir si des modèles plus classiques font aussi bien qu'ADAP.

## Contenu du notebook

1. **Analyse de l'échantillon** : valeurs manquantes, âge, IMC, pression artérielle, avec des points de comparaison issus de la population française.
2. **Visualisation et corrélations** : matrice de corrélation, distributions des variables les plus associées au diabète, intervalles de confiance, et méthode IQR.
3. **Modèles de prédiction** : forêt aléatoire et régression logistique évaluées en validation croisée, courbes ROC, recherche du point où sensibilité = spécificité, et marges d'erreur pour comparer les résultats à ceux d'ADAP.

## Résultats principaux

**Exploration**
- La glycémie est la variable la plus associée au diabète (r = 0,49), devant l'IMC (0,31) et l'insuline (0,30).
- 62,4 % des patientes sont en situation d'obésité ; l'âge médian est de 29 ans.

**Performances globales**

| Modèle | Accuracy | AUC |
|---|---|---|
| Random Forest | 77,2 % | 0,836 |
| Logistic Regression | 76,8 % | 0,842 |
| Modèle naïf (« jamais de diabète ») | 65,1 % | – |

**Au point de croisement (sensibilité ≈ spécificité), avec intervalles de confiance à 95 %**

| Modèle | Seuil | Spécificité | Sensibilité |
|---|---|---|---|
| Random Forest | 0,38 | 75,6 % [71,8 ; 79,4] | 75,4 % [70,2 ; 80,5] |
| Logistic Regression | 0,32 | 75,4 % [71,6 ; 79,2] | 75,7 % [70,6 ; 80,9] |
| ADAP (Smith et al., 1988) | 0,448 | 76 % [68,5 ; 83,5]* | 76 % [65,8 ; 86,2]* |

\* Intervalles estimés en supposant que les 192 patientes de test d'ADAP comptaient la même proportion de cas positifs que l'ensemble des données (environ 35 %).

## Conclusion

Les deux modèles obtiennent des performances très proches, et leurs courbes ROC se superposent presque entièrement. Au point de croisement, ils atteignent environ 75 %, contre 76 % pour ADAP.

Les résultats de ce projet sont plus précis, car les calculés sont réalisés sur 768 patientes au lieu de 192. On peut le constater par des marges d'erreur réduites par rapport au modèle ADAP.

Sur ces données, une simple régression logistique semble donc aussi performante que le réseau de neurones de l'article.

## Limites

- Les trois modèles ne traitent pas les valeurs manquantes de la même façon (catégorie « inconnu » pour ADAP, gestion native pour la forêt aléatoire, imputation pour la régression logistique).
- Les marges d'erreur d'ADAP sont des estimations : l'article ne donne pas la composition exacte de son groupe de test.
- Le perceptron n'a pas été abordé dans ce projet


## Références

- Smith J.W., Everhart J.E., Dickson W.C., Knowler W.C., Johannes R.S. (1988). *Using the ADAP Learning Algorithm to Forecast the Onset of Diabetes Mellitus*. Proceedings of the Annual Symposium on Computer Application in Medical Care, 261–265. [PubMed Central](https://pmc.ncbi.nlm.nih.gov/articles/PMC2245318)
- Données : NIDDK, diffusées par le dépôt UCI Machine Learning (*Pima Indians Diabetes Database*).
