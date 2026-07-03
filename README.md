# Pyronear - Sujet 2
Ce projet consiste en la création d'un code qui permet d'améliorer la détection efficace de feux de forêt.

Pyronear développe un model d'IA opensource qui détecte des feux de forêts avec des caméras placés à des points stratégiques. Le programme utilise YOLO pour la détéction de fumé. L'IA produit un score de confiance qui produit ensuite une alerte transmise à la police si ce score dépasse un seuil. Ce seuil se corrèle lui même avec la probabilité d'incendie qui est tirée d'un score le Fire Warning Indice (FWI). Ce FWI est récupéré via une API. 

Le but est d'optimiser le seuil avec les bons paramètres pour éviter de saturer les pompiers avec des faux positifs. Cela leur fait perdre du temps et retire beeaucoup de crédibilité sans pour autant ne pas détecter de vrais feux, ce qui serait dangereux. Pour cela nous voulons trouver un seuil qui dépend de la classe de fwi qui permet de réduire les faux positifs tout en gardant un maximum de vrais positifs.

Dans ce repo, on retrouve plusieurs fichiers : 
- [un notebook](main.ipynb) contenant nos codes qui nous permettent de traiter les données.
- [un dossier](mines_pyronear_data) qui contient le premier set de données que nous avons eu à disposition
- [un fichier csv](sequences_info.csv) qui contient le deuxième set de données

Ces deux sets contiennent des images de caméras détectant un feu. Pour le premier set, un parsing était nécessaire pour récupérer les données de l'image et de la caméra, pour le deuxième set, toutes les données étaient présents dans le csv. Nous avons eu besoin des deux sets de données pour avoir un maximum d'informations.

De plus, nous avons utilisé le site copernicus qui recense les fwi en fonctions de la position géographique et temporelle.

---
## Pour le notebook

Les parties 1 et 2 du notebook ne servent que pour le parsing des données du premier set de données. Ces parties ne sont pas forcement utiles selon la forme des données supplémentaires qui seront ajouter par la suite. 

La partie 3 permet de concaténer toutes les données utilisées et de les mettre sous la forme d'un DataFrame utilisable pour les statistiques et l'optimisation faite par la suite. 

La partie 4 utilise l'API pour récupérer les données FWI sur Copernicus et joint cela aux données déjà obtenues pour créer une database utilisable. 

La partie 5 permet de visualiser les résultats pour en tirer des conclusions. 

Enfin, la partie 6 permet de faire une optimisation sous contrainte sur les seuils pour chaque classe de fwi. Cette optimisation dépend donc fortement de deux choses : la première est le nombre de données que nous disposons, la deuxième est l'utilisation de la fonction coût qui est très difficile à déterminer (un essai d'une fonction polynomiale est fait dans le notebook, elle permet de trouver des résultats convenable sur le set de données que l'on a, cependant, cela est à essayer sur un autre set de données plus grand).

---
## Pour mettre un nouveau fichier CSV

Pour ajouter un nouveau set de données, il faut modifier le script de la partie 3.

Ce fichier devra avoir la forme suivante (au minimum) (cela dépend aussi du besoin de faire un parsing ou non) :



| label | lat | lon | alert_time | last_seen_at | max_conf |
|---|---|---|---|---|---|
| fp | 48.37916667 | 2.820833333 | 2026-05-08T04:02:41.910825 | 2026-05-08T05:08:49.031245 | 0.467 |

les données sous cette forme sont ensuite traitées dans le notebook pour trouver le seuil optimal.

---
Avec plus de temps on pourrait s'ataquer à un problème rencontré les jours nuageux. Ces jours sont très majoritairement classés en 'verylow' par le FWI. Or les nuages sont souvent très similaires à la fumée mais sur seulement une image.

Une étude qui pourrait être envisagée à la suite de ce notebook peut être une possibilité pour régler ce problème. En effet, on pourrait avec les données du csv, connaître la durée de détection de la fumée. Si cette durée est longue, cela peut d'agir d'un vrai feu. Sinon, cela peut être un nuage ou une fumée d'usine.

Ainsi, il serait intéressant de déterminer le lien de corrélation entre la durée de la fumée et la nature de la fumée.

