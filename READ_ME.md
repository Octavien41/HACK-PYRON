# Pyronear - Sujet 2
Ce projet consiste à l'amélioration de critère dans la détectin de feus de forêts.

Pyronear développe un model d'IA opensource qui detecte des feux de forêts avec des caméras placés à des points stratégiques. Le programme utilise YOLO pour la détéction de fumé. L'IA produit un score de confiance qui produit ensuite une alerte transmise à la police si ce score dépasse un seil. Ce seuil dépend lui même des conditions et de la probabilité d'incendie qui est tirée d'un score le Fire Warning Indice (FWI). Ce FWI est récupérer via une API. 

Les faux positifs arrivent régulièrement avec le modèle YOLO. Le but estt d'optimiser le seuil avec les bons paramètres pour éviter de saturer les pompiers avec des faux positifs. Cela leur fait perdre du temps et retire beeaucoup de crédibilité.
