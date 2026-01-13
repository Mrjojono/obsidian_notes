il enregistre les informations de point de reprises:
	-  dans le fichier de contrôle  
	-  dans l'en-tete de chaque fichier de données 

c'est une structure de données  qui définit un numéro SCN   ( system change Number) dans le thread  de journalisation d'une base de données 

quand  un point de reprise es créé  , oracle database doit mettre a jour les en tête   de tous les fichiers de données  pour actualisé  les informations correspondantes 
le processus CKPT n'ecris pas les blocs disque; c'est le role du processus  DBWn 

