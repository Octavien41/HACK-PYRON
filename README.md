# Pyronear - Sujet 2
Ce projet consiste à l'amélioration de critère dans la détection de feux de forêts.

Pyronear développe un model d'IA opensource qui detecte des feux de forêts avec des caméras placés à des points stratégiques. Le programme utilise YOLO pour la détéction de fumé. L'IA produit un score de confiance qui produit ensuite une alerte transmise à la police si ce score dépasse un seil. Ce seuil se corrèle lui même avec la probabilité d'incendie qui est tirée d'un score le Fire Warning Indice (FWI). Ce FWI est récupéré via une API. 

Le but est d'optimiser le seuil avec les bons paramètres pour éviter de saturer les pompiers avec des faux positifs. Cela leur fait perdre du temps et retire beeaucoup de crédibilité. Pour cela nous voulons trouver un seuil qui permet de réduire les faux positifs tout en gardant un maximum de vrais positifs.

Dans ce repo, on retrouve plusieurs fichiers : 
- [un notebook](main.ipynb) contenant nos codes qui nous permettent de traiter les données.
- [un fichier csv](sequences_info.csv) qui recense le nom la position et la date d'un relevé pour chercher les FWI associé.
- [un dossier](mines_pyronear_data) qui contient les données à traiter.


---
## Pour le notebook

Les parties 1 et 2 du notebook ne servent que pour les données brutes obtenues en parsant les fichiers comme pour le dossier mines_pyronear_data. Ces parties ne sont pas forcement utiles selon la forme des données. La partie 3 est la partie qui nous intéresse. Elle permet de traiter les données du fichier csv et de trouver le seuil optimal. La partie utilise l'API pour récupérer les données FWI et joint cela aux données déjà obtenues pour créer une database utilisable. La partie 5 permet de visualiser les résultats obtenus.

---
## Pour le fichier CSV

Le dossier contient des données de 2024 et 2025. Cela représente peu de données car les les scores FWI des années précédentes n'étaient pas saccessibles. Pour abonder ce projet en données, il faudrait ajuter des données relativues aux feux de forêts des années précédentes. Pour cela, il faudrait trouver un moyen de récupérer les données FWI des années précédentes. Une fois c'est FWI récupéré, il faudrait les ajouter au fichier csv pour pouvoir les traiter dans le notebook.

Ce fichier devra avoir la forme suivante :



| sequence_id | label | sublabel | camera_id | camera_name | organization_id | lat | lon | elevation | camera_azimuth | cone_angle | angle_of_view | alert_time | last_seen_at | detections_count | max_conf | is_wildfire | is_validated |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 40766 | fp | antenna | 67 | moret-sur-loing-02 | 7 | 48.37916667 | 2.820833333 | 92.0 | 359.0 | 2.5 | 54.2 | 2026-05-08T04:02:41.910825 | 2026-05-08T05:08:49.031245 | 24 | 0.467 | | True |

les données sous cette forme sont ensuite traitées dans le notebook pour trouver le seuil optimal.

---
Avec plus de temps on pourrait s'aataquer à un problème rencontré les jours nuageux. Ces jours sont très majoritairement classés en 'verylow' par le FWI. Or les nuages sont souvent très similaire à e la fumée mais sur seulement une image. Pour regler cela il faudrait isoler cette image dans la séquence. On pourrait voir plusieurs techniques :

- on pourrait étudier l'ensemble des 'very low' pour déterminer quand il s'agot de faux positif un seil qui écarte l'image du calcul du score quand celle ci est trop à un score trop éloigné du reste des images. De manière purement statistique, on pourrait comparer cela à un écart type usuel si l'écart est suffisament grand. En pratique, cela pourrait fonctionner car l'image grandit suffisemment le score pour faire passer le seuil à l'ensemble de la séquance alors que le score est faible pour les autres images et le seui élevé pour à cause du FWI.

- on pourrait aussi laisser se processus s'automatiser avec un modèle d'IA suppervisé en lui donnant ce genre de séquences et les séquences very low restantes en annotant chacune des séquences pour ce qu'elle est. De cette manière l'IA s'entrainera à isoler les images modifiant le score de la séquence et pourra les écarter du calcul du score. Cela permettrait d'avoir un score plus juste pour les séquences ayant des nuages bas.

