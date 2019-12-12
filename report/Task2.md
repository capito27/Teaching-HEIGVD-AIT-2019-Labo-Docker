# Task 2

## Déliverables 
### 2)

La réponse est fournie au niveau de la task 0, avec la M1 principalement.

La voici à nouveau : 

> Il n'est pas enviagable d'utiliser la solution actuelle dans un environnement de production.

> Ceci est dû au fait qu'il n'y a aucun mécanisme de scalabilitée, il n'ya pas de système de restauration de noeuds dans le cas ou un noeud tombe ainsi que le fait que la configuration de HAProxy n'est pas mise à jour automatiquement selon les changements des noeuds.

> En effet, avec les 2 conteneurs actuels, le système serait immédiatement surchargé dans un environnement de production.

> Ainsi que si l'un des noeuds crash, il faut une interaction manuelle pour

### 3)

Serf utilise un protocole léger appeler Gossip. 
Son principe est qu'il y aura une création d'un graphe de nodes qui communiqueront avec leur voisin. 
Dès lors, l'ensemble des nodes vont régulièrement communiquer avec leur voisin pour voir s'ils sont en vie. 
Dès que cela échoue, un message est envoyé à ces voisins qui propageront à l'ensemble du réseau ce qui permet d'avoir une propagation de l'information de manière très rapide. 
Serf permet par l'élaboration de ce réseau de pouvoir transmettre des messages et des queries, c'est ce mécanisme qui est utilisé pour donner la connaissance des noeuds vivants à HAProxy de manière dynamique. 
Une autre alternative très mauvaise serait de travailler avec NMAP.
