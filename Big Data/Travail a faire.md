
1. lister tous les emprunts 
db["emprunt"].find()
2. lister uniquement les titres de livres emprunter. 
db.emprunt.find({}, {"livre.titre": 1,id:0})
3. trouver tous les emprunts non encore rendu. 
db.emprunt.find(
  { "livre.emprunt.date_rendu": null },
  { "livre.emprunt": 1 }
)
4. lister tous le emprunts d'un utilisateur appelé  "fafa"  
db.emprunt.find({
  "emprunteur.name": "fafa"
})
5. trouver tous les emprunts des lectrices 
db.emprunt.find({
  "emprunteur.sexe": {$eq:"F"}
})
6. lister les livres ayant plus de 200  pages  
db.emprunt.find({
  "livre.nb_pages": {$gt : 200}
})
7. lister tous les livres écrit par des auteur français 
db.emprunt.find({
  "livre.auteur.pays": {$eq : "France"}
})
8. lister tous les livres éditer au Togo  
db.emprunt.find({
  "livre.editeur.pays": {$eq : "Togo"}
})
9. lister les livres dont les auteurs sont encore en activité 
db.emprunt.find({"livre.auteur.date_nais": {$eq : null}})
10. lister tous les livres publiés  avant 1950
db.emprunt.find({"livre.date_paru": {$lt : "1950-01-01"}})