Textures procédurales et normal mapping
=======================================

.. caution::

   Obtenir de bons résultats avec de la génération de texture procédurale peut être difficile sans de bonnes bases de math et une bonne dose d'imagination. Générer procéduralement des normales est tout aussi difficile.

Textures procédurales
---------------------

Au lieu d'assigner une couleur par objet, il est possible de calculer la couleur d'un point en fonction de sa position dans l'espace, et même de la normale à la surface !

.. code-block:: c

   struct vec3 procedural_texture(struct vec3 position, struct vec3 normal)

Une texture procédurale est donc calculée à la demande, par une fonction.

Il existe une immense variété de méthodes de génération de textures, qui peuvent être composées pour aboutir au résultat escompté. Certaines fonctionnent en 2d, et il faut alors les appliquer sur une surface 3d, alors que d'autres sont capables de travailler directement en 3d.

La plus connue et populaire est certainement le bruit de perlin, mais d'autres alternatives existent. Je vous conseille fortement le site d'Inigo Quilez, qui parle entre autres merveilles de génération procédurale de textures: https://iquilezles.org/www/index.htm (cherchez procedural noises).

Cet article en particulier devrait vous intéresser : `<https://iquilezles.org/www/setarticles/warp/warp.htm>`_


Normales procédurales
---------------------

En plus de générer une couleur procéduralement pour chaque point, il est également possible de générer une normale.

Deux écoles d'opposent :

 - la normale peut être calculée analytiquement, en même temps que votre texture procédurale
 - elle peut également être approximée numériquement, en évaluant plusieurs fois tout ou partie de votre fonction de texture

Il y a une distinction à faire entre la dérivée d'une fonction de bruit, et la dérivée de la surface sur laquelle on applique cette texture de bruit.

En pratique, les deux peuvent être complètement indépendant, et il peut être préférable d'appliquer une perturbation de normale indépendante de la texture.


Normal mapping
--------------

Attention, le normal mapping est un peu plus lourd en math que la simple application de texture, car il faut effectuer une rotation dans l'espace du vecteur normal de la texture pour l'appliquer sur la face, qui a sa propre normale.

Je ne saurais faire concurence à cet excellent tutoriel, si vous souhaitez vous aventurer dans ce domaine : `<https://learnopengl.com/Advanced-Lighting/Normal-Mapping>`_

UV Mapping
----------

Au lieu de générer une texture dynamiquement avec du code, en fonction du point et de la normale d'intersection, il est également possible d'appliquer une texture 2D sur un objet 3D.

L'UV mapping est une technique omniprésente dans l'industrie, qui permet de faire correspondre à un point sur une surface un point dans une texture.

Cette technique est relativement difficile à mettre en œuvre pour un projet court, à cause de la logistique relativement compliquée du chargement d'image et de modèles, mais mérite tout de même d'être mentionnée.
