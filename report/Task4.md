# Task 4

## Déliverables 
### 1)

Il faudrait séparer les installations de programmes en plusieurs commandes RUN, afin de créer plusieurs images intermédiaires, qui contiennent le système de fichier à l'état suivant la commande effectuée.
(Plus particulièrement, ces différentes images contiennent uniquement les différences entre l'image précédente et l'image actuelle, suite à l'exécution du RUN).

Dans le cas l'on effectue plusieurs commandes qui modifient, ajoutent ou supprimes les mêmes fichiers, il peut être préférable de les effectuer en une seule commande, afin de n'avoir qu'une seule image layer, avec les résultats finaux des différences commandes.

Dans le cas ou nous avons plusieurs images docker, contenant plusieurs dépendances en commun, il peut être préférable de commencer les Dockerfiles en exécutant les commandes communes aux deux images, afin de limiter le nombre de layers nécessaires.

### 2)

Il faudrait modifier les Dockerfiles des deux images, de telles façons à ce qu'ils exécutent leur commandent similaires au même endroit dans la création de leur image.
C'est-à-dire qu'il faudrait que les Dockerfile de ha et webapp fassent immédiatement un apt-get update, et installent les paquets qu'ils ayant en commun (wget, curl, vim)

Ensuite qu'ils installent S6 et Serf, et finalement qu'ils installent les autres paquets qui ne sont pas en commun, et continuent avec le contenu de leur Dockerfile original restant.

### 4)

Car on ne connaît que la dernière machine ayant rejoint le serveur sans avoir l'information des serveurs ayant déjà rejoint le serveur ! De plus, lorsqu'un serveur quitte la structure (arrêt), nous n'avons en aucun cas réagi à ce départ. Il serait donc nécessaire de faire un annuaire où l'on enregistre l'ensemble des serveurs connectés. De plus, il est nécessaire de filtrer les backends et le proxy, car le proxy n'est pas un serveur d'application et donc ne devra pas se trouver dans la configuration.
