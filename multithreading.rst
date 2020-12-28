Multithreading
==============

Comme vous l'avez certainement remarqué, même sans lancer plusieurs rayons par pixel,
rendre une image avec beaucoup de triangles est très, très lent.

En effet, pour le moment, doubler le nombre de pixel d'une scène double le temps de rendu.
Deux approches sont possibles:

 - on pourrait réduire la complexité de l'algorithme, pour faire en sorte que la taille de
   la scène impacte moins le temps de rendu
 - on peut aussi décider d'effectuer le calcul en parallèle

Pour effectuer le calcul en parallèle, vous pouvez utiliser des threads.
Un peu comme plusieurs processus peuvent s'exécuter en parallèle, plusieurs threads peuvent s'exécuter en parallèle à l'intérieur d'un même processus. Tous les threads d'un même processus peuvent partager leur mémoire (accéder directement, avec les mêmes pointeurs, aux mêmes données). Cette propriété ne tient pas quand on parle de deux threads dans deux processus différents.

On peut utiliser des threads pour accélérer le calcul:

 - une première approche pourrait être de lancer un thread par pixel de l'image. Toutefois, cette approche risque de rendre le calcul encore plus lent. En effet, lancer un thread prend du temps, car le système d'exploitation a des préparations relativement coûteuses à effectuer.
 - une approche alternative et plus efficace, serait de lancer un thread par colonne de l'image. Cette approche, bien que vastement supérieure, pose un autre problème mineur : il est inefficace pour plusieurs threads d'écrire à des endroits proches en mémoire. Ainsi, si deux threads rendent leurs colonnes de pixels en même temps, ils vont interférer, car écrire dans les mêmes `zones de mémoire <https://en.wikipedia.org/wiki/False_sharing>`_.
 - une encore meilleure approche est d'assigner plusieurs lignes de l'image à un nombre fixe de threads, choisi en fonction des capacités de la machine hôte (avec ``sysconf("_SC_NPROCESSORS_ONLN")`` par exemple. Par exemple, on peut découper le travail sur une image de cent lignes entre 3 threads comme suit: le premier thread s'occupe des lignes 0 à 33, le second de la ligne 33 à 66, et le dernier de la ligne 66 à 100.
 - une approche plus évoluée consiste à découper d'image en blocs, les mettre dans une liste d'attente, et lancer des threads qui vont piocher dans cette liste, render le bloc, puis en piocher un nouveau jusqu'à ce que tous les blocs soient rendus. L'image est ensuite réassemblée, une fois tous les blocs terminés. Cette approche a pour avantage de présenter une meilleure `localité de cache <https://en.wikipedia.org/wiki/Locality_of_reference#Spatial_and_temporal_locality_usage>`_.

Approche suggérée
-----------------

 - lancer un nombre fixe de threads avec ``pthread_create``
 - diviser le nombre de lignes de l'image par le nombre de threads, et attribuer à chaque thread un groupe de lignes consécutives
 - attendre que tous les threads se terminent avec ``pthread_join``

Ressources utiles
-----------------

Heureusement, vos threads n'ont pas besoin d'interagir ensemble, ce qui rend superflu des mécanismes de synchronisation. Vous n'aurez probablement pas besoin d'autre chose que les pages de man des fonctions évoquées dans l'approche suggérée.
