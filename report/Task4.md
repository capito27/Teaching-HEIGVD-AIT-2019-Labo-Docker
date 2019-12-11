# Task 4

## Déliverables 
###1)

Il faudrait séparer les installations de programmes en plusieurs commandes RUN, afin de créer plusieurs images intermédières, qui contiennent le systeme de fichier à l'état suivant la commande effectuée.
(Plus particulièrement, ces differantes images contiennent uniquement les differences entre l'image précédante et l'image actuelle, suite à l'execution du RUN).

Dans le cas l'on effectue plusieurs commandes qui modifient, ajoutent ou supprimes les memes fichiers, il peut etre préfèrable de les effectuer en une seule commande, afin de n'avoir qu'une seule image layer, avec les resultats finaux des differentes commandes.

Dans le cas ou nous avons plusieurs images docker, contenant plusieurs dépendances en commun, il peut etre préfèrable de commencer les Dockerfiles en executant les commandes communes aux deux images, afin de limiter le nombre de layers necessaires.

###2)

Il faudrait modifier les Dockerfiles des deux images, de telle façon à ce qu'ils executent leurs commandes similaires au même endroit dans la création de leur image.
C'est à dire qu'il faudrait que les Dockerfile de ha et webapp fassent immediatement un apt-get update, et installent les packets qu'ils ayent en commun (wget, curl, vim)

Ensuite qu'ils installent S6 et Serf, et finaement qu'ils installent les autres packets qui ne sont pas en commun, et continuent avec le contenu de leur Dockerfile original restant.

###4)

Car on ne connait que la dernière machine ayant join le serveur sans avoir l'information des serveurs ayant déjà rejoint le serveur ! De plus, lorsqu'un serveur quitte la structure (arrêt), nous n'avons en aucun cas réagis à ce départ. Il serait donc nécessaire de faire un annuaire où l'on enregistre l'ensemble des serveurs connectés. De plus, il est nécessaire de filtrer les backends et le proxy car le proxy n'est pas un serveur d'application et donc ne devra pas se trouver dans la configuration.
