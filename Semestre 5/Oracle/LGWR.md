commit une transaction sur une base de données    ( valider une commande)

lors d'un commit  lgwr place un enregistrement dans le tampons de journalisations ( redo log buffer)  et il écris immédiatement  sur le disque dans le fichier de transaction  ( un fichier redo.log )

chaque fois que une transaction est valider, la validation est enregistrer sur  un numero qui est générer  ==**SCN** : System change number==  qui est enregistrer dans les fichiers redo 
en cas de crache oracle utilise ces numéros  pour faire la récupération; il doit remonter a un état cohérent  avant de pouvoir récupérer. 

dans le périodes de forte activité , le processus  LGWR  peut écrire dans le fichier de journalisation via des validations groupées. 

