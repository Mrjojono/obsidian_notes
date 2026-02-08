Commande d'insertion dans mongodb 

TP 1 : 

db.createcollection("Etudiant")
pour une seulr insertion on peut taper la commande :  insertOne({})
pour inserer plusieurs elements : InsertMany([{},{},{}]) 

exemple d'insertion : 

db.etudiant.insertOne({
"matricule":100,
"nom":"Etunde",
"prenom":"Olalma",
"sexe":"M",
"date_naissance": "2001-01-13"
})

exercice  :  
categorie ( code_cat, libelle)
Produit(Libelle_prod,Qte,_stock, Prix,code_cat)
Client(Nom_cli,Adresse)
Commande(Id_cde,date_cde,Id_clt)
Detail(ID_cde,Id_prod,Qte_cde)

TAF:  
Traduire le modele relationnel  en modele Json avec Mongodb en créant la collection valide 


