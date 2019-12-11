# Task 1

## Déliverables 
###1)
Voici la capture d'écran de HA proxy
![](assets/img/T1_1_haproxy.png)
###2

Nos dificultées dans cette taches sont principalement dues au fait que la plupart des commandes ne peuvent pas etre executées directement sous la forme fournie.
Plus serieusement, l'utilitée principale de l'installation de ce supperviseur dans le contexte d'un conteneur docker, est le redémarage automatique de certains daemons et services, dans le cas ou le serveur webapp meurt automatiquement, il sera redémarré automatiquement.

Selon nous, une telle solution n'est pas appropriée. En effet, il ce peut que le serveur crash de telle façon à ce qu'il corrompe le conteneur. 
Pour palier à ce probleme, il faudrait utiliser les comportements de base de  docker compose, qui permet de recréer automatiquement un container avec l'image sous-jaçante si celui-ci se crash.