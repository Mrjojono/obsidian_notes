
La reprise de documents cartographique existants sur le paoer  en vue de les introduire dans un SIG, pouvait rouvrir a des techniques différentes ; la digitalisation et le balayage électronique par exemple : 

modélisation du monde réel --> vers  --> le logiciel 

	Mode vecteur
Ce mode répond au soucis de représenter un objet de manier aussi exacte que possible . Pour transformer un objet réel en une donnée a référence spatiale  , on décompose le territoire en couches thématiques (relief routes) Structurées dans des bases de données numérique 

une *Couche* renuit généralement des éléments géographiques  de même  type  
les éléments géographiques (objets spatiaux) peuvent être   représentés sur une carte par des points, des lignes ou des polygones 

==les avantages du mode vecteur==  
- une meilleur  adaptation a la description des entités ponctuelles et linéaires 
- une facilité  d'extraction de détails  
- Une simplicité   dans la transformation de coordonnées  

==Les inconvénients  du mode vecteur== 
- les croisements  de couches d'information sont délicats et nécessite une topologie parfaite  
ex: arbre  , bâtiment  , maison 

	Mode raster
Le mode trame ou raster est également   appelé modèle matriciel . 
Contrairement  au mode vecteur qui ne décrit que les contours , le mode raster décrit la totalité  de la surface cartographique point par point 
il est utilisé  principalement  dans le système  de balayage 

==Les avantages du mode raster sont== 
- Meilleure adaptation  a la représentation  des détails surfaciques  
- Acquisition des données  a  partir d'un scanner a balayage  
- Meilleure adaptions a certains types de traitements numériques : filtres , classifications  

==Les inconvénients  du mode raster  sont==  
- Mauvaise adaptation a la représentation des détails  linéaires 
- Obligation de parcourir toute la surface pour extraire un détail  
- Impossibilité   de réaliser certaines opération topologiques  . la recherche  du plus court chemin dans un réseau par exemple  

exp : relevé   de précipitations , ph du sol , nuages. 