# Task 0

## Questions :

### M1)

Il n'est pas enviagable d'utiliser la solution actuelle dans un environnement de production.

Ceci est dû au fait qu'il n'y a aucun mécanisme de scalabilitée, il n'y a pas de système de restauration de noeuds dans le cas ou un noeud tombe ainsi que le fait que la configuration de HAProxy n'est pas mise à jour automatiquement selon les changements des noeuds.

En effet, avec les 2 conteneurs actuels, le système serait immédiatement surchargé dans un environnement de production.

Ainsi que si l'un des noeuds crash, il faut une interaction manuelle pour le restaurer.

### M2)

Il faut ajouter les lignes suivantes dans le fichier `.env` :

```
WEBAPP_3_NAME=s3
WEBAPP_3_IP=192.168.42.33
```

Ajouter également les lignes suivantes au fichier `docker-compose.yml` file, avant le service `haproxy` :
```
  webapp3:
       container_name: ${WEBAPP_3_NAME}
       build:
         context: ./webapp
         dockerfile: Dockerfile
       networks:
         heig:
           ipv4_address: ${WEBAPP_3_IP}
       ports:
         - "4002:3000"
       environment:
            - TAG=${WEBAPP_3_NAME}
            - SERVER_IP=${WEBAPP_3_IP}
```

Ajouter également les lignes suivantes au fichier `docker-compose.yml` file, dans les lignes d'environnement du service `haproxy` :
```
            - WEBAPP_3_IP=${WEBAPP_3_IP}
```

Et finalement, ajouter à la fin de la section "backend nodes" de la configuration de haproxy, dans le fichier `ha/config/haproxy.cfg` :
```
    server s3 ${WEBAPP_3_IP}:3000 check
```

### M3)

Il serait possible d'utiliser un script hôte afin de monter ou démonter de nouveaux noeuds, et utiliser un système de communication avec HAProxy afin de mettre à jour la configuration en cours, pour qu'elle prenne en compte les noeuds qui ont été ajoutés ou supprimés.

### M4)

Il est probablement possible d'utiliser l'API Runtime de HAProxy afin de le manager de façon asynchrone pour ajouter et supprimer des noeuds de la configuration du proxy.
Ou du moins on serait censé pouvoir le faire si la version de HAProxy utilisée actuellement était au mois 1.8 (c'est la 1.5).

Utiliser Traefik au lieu de HAProxy pour résoudre ce problème de manière bien plus élégante...

### M5)

Notre solution actuelle est incapable d'exécuter plusieurs processus, car elle se repose sur les comportements par défaut de docker.

En effet, en ce moment, le Dockerfile démarre le conteneur webapp de façon bloquante, il faudrait utiliser un mécanisme ou solution pour démarrer plusieurs processus à la création du conteneur.

### M6
C'est faux. Le script ne fait rien au final, car il n'y a pas de balise `<s1>` dans le fichier haproxy.cfg. Donc, en conséquence, rien ne se passe si on ajoute plus de noeuds. Ce n'est pas dynamique, car le script ne fait rien.

Une meilleure solution serait de remplacer HAProxy par Traefik...

## Deliverables :

### 1)

Voici la capture d'écran de HA proxy
![](assets/img/T0_1_haproxy.png)


### 2)

Voici l'URL de mon URL de repo : 
https://github.com/capito27/Teaching-HEIGVD-AIT-2019-Labo-Docker
