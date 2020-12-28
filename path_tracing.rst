Pathtracing
===========

Le pathtracing est un type de raytracing à la fois un peu spécial et omniprésent:

 - la totalité des jolies images présentées comme générées avec du raytracing sont en fait générées par un pathtracer
 - les pathtracers lancent beaucoup de rayons par pixel, puis font la moyenne de la couleur trouvée pour chaque rayon pour décider de la couleur du pixel
 - de manière générale, chaque rayon primaire (provenant du plan image) lancé par un pathtracer ne génère pas d'autres rayons au fil de son évolution. Par exemple, si un rayon touche une surface est mi matte mi réfléchissante, un pathtracer ne lancera *pas* deux rayons pour en faire la moyenne, mais laissera au hasard le sort du rayon. En plus d'être plus proche de la réalité physique, cette approche permet d'éviter de lancer trop de rayons.
 - Sans astuce et optimisation, cette méthode est particulièrement lente: pour qu'un rayon ne trouve pas que du noir, il faut qu'il trouve une source de lumière. Si la source de lumière est toute petite.

Choisir ce projet implique de choisir quel comportement associer aux rayons. La plupart des pathtracers utilisent des méthodes élaborées simulant le comportement des rayons via des distributions de probabilité, échantillonnées par des algorithmes tout aussi subtils.

Je vous suggère de suivre le planning suivant :

 - ajouter un fond lumineux (faire en sorte que les rayons qui touchent l'immensité infinie du vide numérique portent une couleur un peu plus lumineuse que le noir)
 - écrire du code pour lancer plusieurs rayons partant d'un point aléatoire dans le pixel du plan image
 - d'abord simuler une réflection diffuse de idéale de Lambert (n'ayez pas peur, c'est expliqué par la suite)
 - pour des rendus plus intéressants, vous pouvez implémenter de la réflection, et tirer au sort entre réflection et lumière diffuse, avec une probabilité variant en fonction de l'angle (loi de fresnel)


Ideal Lambertian Reflection
---------------------------

C'est comme si on avait un rayon aléatoire dans la demi sphère dans le sens de la normale, mais en prenant en compte la loi du cosinus.

Quand un rayon atteint une surface, il rebondit dans une direction aléatoire dans la demi sphère dans le sens de la normale, mais de telle sorte à ce que la probabilité de chaque angle de rebond soit proportionnelle au cosinus entre l'angle de rebond et la normale à la surface.

Une méthode maligne pour obtenir un tel rayon est d'additionner à la normale de la surface un vecteur unitaire aléatoire, puis de normaliser le résultat. La direction obtenue aura bien les propriétés décrites ci-dessus.

Notes et astuces
----------------

 - attention, cette technique est malheureusement particulièrement lente sur CPU, et donc, en C. Vous êtes libres de l'implémenter sur GPU avec GLSL et shadertoy, si vous êtes particulièrement confiants : le langage de programmation est plus adapté au problème, mais différent et comporte nombre de contraintes étranges.
 - si votre rayon ne touche aucun objet (le ciel), vous pouvez lui attribuer une couleur constante, ou même fonction de l'angle du rayon. vous pourriez par exemple simuler un coucher de soleil en mélangeant du rouge, du noir et du bleu en fonction de la composante verticale du rayon.
 - n'utilisez pas de source de lumière « point », votre rayon ne la touchera jamais.

Ressources utiles
-----------------

`<https://raytracing.github.io/books/RayTracingInOneWeekend.html>`_

`<https://wwwtyro.net/2018/02/25/caffeine.html>`_

`<https://blog.demofox.org/2020/05/25/casual-shadertoy-path-tracing-1-basic-camera-diffuse-emissive/>`_

`<https://blog.demofox.org/2020/06/06/casual-shadertoy-path-tracing-2-image-improvement-and-glossy-reflections/>`_

`<https://blog.demofox.org/2020/06/14/casual-shadertoy-path-tracing-3-fresnel-rough-refraction-absorption-orbit-camera/>`_
