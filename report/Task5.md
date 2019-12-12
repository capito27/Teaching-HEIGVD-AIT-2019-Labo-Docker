# Task 5

## Déliverables 
### 4)

Nous proposons de complètement supprimer Serf, S6 et HAProxy, et de les remplacer intégralement par Traefik.
En effet, en utilisant une Traefik comme solution pour le reverse-proxy, le load-balancing et la scalabilitée, ainsi que Docker-Compose pour la redondance (grâce à la reconstruction automatique de conteneurs) ainsi que le moteur de scalabilitée, il est possible d'implémenter un système de reverse proxy, avec load balancing, redondance et forcement scalable sans que la webapp concernée n'ait à avoir aucune connaissance de l'existence de ce système.
Cela devient un système puissant et simple d'utilisation, afin de simplement et rapidement transformer toute webapp dockerisée penser pour être scalable en une webapp load balancé, scalable et redondant.


Voici la configuration docker-compose.yml afin d'utiliser traefik pour du load balancing :

```yaml
version: '3.7'

services:
  treafik:
    image: traefik
    restart: unless-stopped
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro

  webapp:
    build:
      context: ./webapp
      dockerfile: Dockerfile2
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.webapp.rule=Host(`localhost`)"
      - "traefik.http.services.webapp.loadbalancer.server.port=3000"
      - "traefik.http.services.webapp.loadbalancer.sticky.cookie.name=StickySessionCookie"
      - "traefik.http.services.webapp.loadbalancer.sticky.cookie.httpOnly=true"
      - "traefik.http.middlewares.latency-check.circuitbreaker.expression=LatencyAtQuantileMS(50.0) > 100 || NetworkErrorRatio > 0.30"
```

Comme on peut le remarquer, TOUTE la configuration de traefik est faite au niveau du docker-compose, le routage par défaut du loadbalancer est en round robin, et nous avons activé manuellement la stickysession (et activé le HttpOnly, par bonne mesure).
Il est également intéressant de remarquer que si la médiane du temps de réponse de quelconque noeud vient à dépasser 100 ms, ou que le taux d'erreurs réseau de celui-ci atteint 30%, il sera immédiatement supprimé de la liste des noeuds disponibles, et toutes les requêtes en attente de résolution par ce même serveur seront reroutées vers un autre serveur disponible (et les cookies de sticky session mise à jour), afin d'éviter une erreur au niveau du client autant que possible.
Une fois un noeud coupé de la liste de loadbalancing, traefik essayera de progressivement vérifier si celui-ci à pu récupérer de son problème, et est est utilisable à nouveau, auquel cas, il l'intégrera à nouveau à sa liste de noeuds.
Nous avons également pu simplifier grandement le Dockerfile de la webapp, qui désormais ne fait plus que d'installer les dépendances de la webapp et la lancer.

```bash
# The base image is one of the offical one
FROM node:latest

# TODO: [GEN] Replace with your name and email
MAINTAINER John Dove <john.doe@heig-vd.ch>

# Install the required tools to run our webapp and some utils
RUN apt-get update && apt-get -y install wget curl vim && apt-get clean && npm install -g bower

# Create app directory
WORKDIR /backend/app

# We copy the package.json to allow for dependency installation
COPY app/package*.json ./

# We then install the node dependencies
RUN npm install

# We then copy the app sources
COPY app .

# We then finally run the bower install
RUN bower install --allow-root

# Expose the web application port
EXPOSE 3000

# Set the default container command, which can still be overwritten if needed
CMD [ "node", "app" ]
```

Il est possible de tester ce nouveau système à l'aide du docker-compose2.yml présent dans le repo.
Pour ce faire, il suffit de se placer à la racine du repo, et de lancer la commande suivante, afin de démarrer l'infrastructure avec 3 backends : 

```bash
docker-compose -f docker-compose2.yml up --scale webapp=3
```

