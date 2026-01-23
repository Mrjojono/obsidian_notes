


les besoins de la plateforme elearning peuvent etre associés  au type de données noSql  suivant   

*  les profiles utilisateurs sont stocker dans une base de données  orienté  documents 
* les cours sont stocker dans une base de données  orienté  documents 
* les commentaires en temps réel  sont stocker dans une base de données  orienté  clé valeur  ( redis par exemple )
* les statistiques  de consultation sont stocker dans une base de données  orienté colonne  


1. justification des choix  

	les profiles utilisateurs correspondent  a des données  semi structuré  dont le schéma peut évoluer( information personnelle , rôle , historique) .
	une base de données  orienté  document permet donc de stocker ces informations  de manière  flexible   sans schéma rigide  tout en regroupant les données d'un utilisateur dans un même document
	les cours sont des objets complexe composé  de méta-données  ( titre, auteur ) de  contenu pédagogique ( chapitre vidéo documents ) et éventuellement  d'évaluation. une base de données  orienté  document est donc adapté  a ce type de données  hiérarchique et évolutive  car elle permet de représenter  naturement la structure  d'un cours 
		les commentaires en tant réel  nécessite  des  operations tres rapides en lectures et en ecriture