Antialiasing (anticrénelage)
============================

L'antialiasing est un procédé permettant d'éviter l'aspect crénelé caractéristique de certains algorithmes de rendu. Il existe plusieurs types d'antialiasing :

 1) on peut d'abord obtenir une image crénelée, puis la traiter pour cacher ces défauts
 2) on peut aussi tout simplement changer d'algorithme de rendu pour que le problème disparaisse ! Ce problème survient quand un algorithme n'analyse pas équitablement le contenu des pixels. Par exemple, la base de raytracer donnée ne lance qu'un rayon au centre du pixel, et ignore tous les détails qui pourraient se trouver dans les autres régions. On peut tout simplement lancer plus de rayons dans le pixel pour obtenir une couleur plus proche de ce qu'on obtiendrait si on prenait la moyenne de tous les rayons qu'il est possible de lancer par ce pixel. Par exemple, on peut lancer 4 rayons par pixel, organisés en grille.

La première option comprend les techniques FXAA, TSAA ou SMAA, tandis que la dernière option comprend les plus simples MSAA et SMAA.

L'immense majorités des raytracers utilisent la seconde option (les pathtracers sont de toute façon obligés de lancer un grand nombre de rayons par pixel), mais vous êtes libre de choisir la première si vous avez beaucoup de temps libre.

Approche suggérée
-----------------

 - commencez par lancer plusieurs rayons par pixel, organisés en grille régulière
 - calculez la moyenne de la couleur trouvée par ces rayons

Le résultat devrait déjà être relativement satisfaisant. Lancer 4 rayons par pixel est déjà une énorme amélioration. Vous pouvez améliorer vos résultats en rajoutant un petit décalage aléatoire (dont la norme est inférieure à la moitié de la distance séparant deux rayons sur la grille).
