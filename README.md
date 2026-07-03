# Pyronear - Sujet 2
Ce projet consiste à l'amélioration de critère dans la détection de feux de forêts.

Pyronear développe un model d'IA opensource qui detecte des feux de forêts avec des caméras placés à des points stratégiques. Le programme utilise YOLO pour la détéction de fumé. L'IA produit un score de confiance qui produit ensuite une alerte transmise à la police si ce score dépasse un seil. Ce seuil se corrèle lui même avec la probabilité d'incendie qui est tirée d'un score le Fire Warning Indice (FWI). Ce FWI est récupéré via une API. 

Le but est d'optimiser le seuil avec les bons paramètres pour éviter de saturer les pompiers avec des faux positifs. Cela leur fait perdre du temps et retire beeaucoup de crédibilité. Pour cela nous voulons trouver un seuil qui permet de réduire les faux positifs tout en gardant un maximum de vrais positifs.

Dans ce repo, on retrouve plusieurs fichiers : 
- [un notebook](main.ipynb) contenant nos codes qui nous permettent de traiter les données.
- [un fichier csv](sequences_with_fwi.csv) qui recense le nom la position et la date d'un relevé pour chercher les FWI associé.
- [un dossier](mines_pyronear_data) qui contient les données à traiter.

---

Le dossier contient des données de 2024 et 2025. Cela représente peu de données car les les scores FWI des années précédentes n'étaient pas saccessibles. Pour abonder ce projet en données, il faudrait ajuter des données relativues aux feux de forêts des années précédentes. Pour cela, il faudrait trouver un moyen de récupérer les données FWI des années précédentes. Une fois c'est FWI récupéré, il faudrait les ajouter au fichier csv pour pouvoir les traiter dans le notebook.

Ce fichier devra avoir la forme suivante :


|sequence | label| azimut| date |latitude | longitude| score max| score moyen | nombre de frames| fwi| fwi_class|
|---------|-----|-------|------|---------|----------|----------|-------------|-----------------|----|----------|
|pyronear-force-06_cabanelle_125_2024-02-16T12-11-03|wildfire,pyronear-force-06_cabanelle|125|2024-02-16|43.7866472009|7.4196082789|0.875|0.8135462857142858|7.0|0.0|very_low
