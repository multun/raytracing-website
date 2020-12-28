KD trees
========

Un KD trees est une structure de donnée arborescente qui crée une hiérarchie de groupes d'objets. Chaque noeud d'un KD tree correspond à une région de l'espace, et a deux enfants, qui définissent eux-mêmes des sous-régions de cet espace.

Utiliser un KD tree permet de réduire le temps de recherche d'intersections avec les objets de la scène : au lieu de tester l'intersection avec tous les objets de la scène, on cherche les zones définies par les noeuds du KD tree par lesquels passent un rayon.

Recherche d'intersection
------------------------

L'idée est très simple: si un rayon ne passe pas par la région définie par un noeud, il ne passe par aucune des deux sous-régions.

Par contre, s'il y a une intersection entre le rayon et la région de l'espace définie par un noeud, il est nécessaire de tester l'intersection avec ses deux sous régions. Pour chaque enfant, s'il y a une intersection, on continue récursivement l'exploration, et s'il n'y en a pas, il est inutile de continuer dans cette branche.

Cette technique permet de réduire la complexité de la recherche d'intersection de :math:`O(n)` à :math:`O(log(n))`.

AABB Bounding boxes
-------------------

Il est très commun que les régions que définissent les noeuds d'un KD tree soient des `pavés droits <https://fr.wikipedia.org/wiki/Pav%C3%A9_droit>`_, dont les arrêtes sont alignées sur les axes x, y et z.

Ces boites, qui englobent des formes géométriques ou groupes de formes, sont appelées boites AABB.

Cette forme géométrique a plusieurs avantages:

 - elle est facile à subdiviser en deux
 - elle nécessite peu de mémoire : au lieu de stocker les 8 points du pavé, on peut simplement stocker les coordonnées de deux points opposés
 - on peut facilement calculer la boite AABB minimale d'un polygone (un triangle par exemple) en calculant le minimum et le maximum des coordonnées en x, y et z de l'objet : le point aux coordonnées du minimum en x, y, et z forme un coin du pavé, et le point portant les coordonnées du maximum des x, y et z forme le coin opposé eu pavé
 - on peut facilement calculer la boite AABB minimale de deux boites AABB : on prend juste le minimum et la maximum des coordonnées des points, comme avec n'importe quel autre polygone

Construction de l'arbre
-----------------------

Comme avec la plupart d'algorithmes de recherche reposant sur des arbres, la performance de l'algorithme dépend surtout de la technique utilisée pour construire l'arbre.

En effet, il est tout à fait possible de construire un KD tree n'offrant pas ou peu d'amélioration de performance, par exemple en divisant l'espace toujours dans la même direction.

Voici un algorithme naïf, mais offrant déjà une amélioration de performance importante par rapport à un test de toutes les intersections :

 - on pré-calcule les AABB box de tous les objets de notre scène, afin de faciliter la suite des opérations : lors de la construction de l'arbre, seule l'AABB box d'un objet importe
 - on calcule d'abord l'AABB box de notre scène, et donc de la racine de notre KD tree, en fusionnant itérativement toutes les AABB box des objets de notre scène
 - on divise notre AABB racine en deux sous région, selon par exemple l'axe des X. On déplace nos objets du noeud racine vers les deux sous boites. Si un objet est à cheval sur les deux boites, il est ajouté dans les deux listes.
 - on répète l'opération de séparation jusqu'à la profondeur souhaitée (10, par exemple), en alternant l'axe de division des boites (la racine sera divisée selon l'axe des x, ses enfants selon l'axe des y, les enfants des enfants selon l'axe des z, et ainsi de suite en repartant de x)


Ressources utiles
-----------------

`<https://en.wikipedia.org/wiki/Minimum_bounding_box>`_

`<https://www.flomonster.fr/articles/kdtree.html>`_

`<https://fr.wikipedia.org/wiki/Arbre_kd>`_

`<https://en.wikipedia.org/wiki/K-d_tree>`_
