


les besoins de la plateforme elearning peuvent etre associés  au type de données noSql  suivant   

*  les profiles utilisateurs sont stocker dans une base de données  orienté  documents 
* les cours sont stocker dans une base de données  orienté  documents 
* les commentaires en temps réel  sont stocker dans une base de données  orienté  clé valeur  ( redis par exemple )
* les statistiques  de consultation sont stocker dans une base de données  orienté colonne  


1. justification des choix  

	les profiles utilisateurs correspondent  a des données  semi structuré  dont le schéma peut évoluer( information personnelle , rôle , historique) .
	une base de données  orienté  document permet donc de stocker ces informations  de manière  flexible   sans schéma rigide  tout en regroupant les données d'un utilisateur dans un même document
	les cours sont des objets complexe composé  de méta-données  ( titre, auteur ) de  contenu pédagogique ( chapitre vidéo documents ) et éventuellement  d'évaluation. une base de données  orienté  document est donc adapté  a ce type de données  hiérarchique et évolutive  car elle permet de représenter  naturelement la structure  d'un cours 
		les commentaires en tant réel  nécessite  des  operations très rapides en lectures et en ecriture   avec une forte capacité  a géré  les pic de charge . une base de données cle valeurs est alors particulièrement adapté a ce besoin , car elle est offre une tres faible latence  et une grande simplicité  d'accès  au données  
	les statistiques  de consulations génère  de grand volume de données   ( nombre de vues  duré de consultation, horodatage)
	une base de données  orienté  colonne est optimiser pour le stockage et l'analyse des données massive  ainsi que pour les requêtes  analytique et les agregation ce qui la rend particulièrement  adapter  a ce cas d'usage  
conclusion :  

le choix d'une architecture  nosql adapter a chaque besoin permet d'optimiser les perfomances  , l la scalabilité  et la flexibilité  de la plate-forme qui les uses 

une approche multimodel est donc pertinente  pour repondre efficacement   au différent  type de données  et d'usage  