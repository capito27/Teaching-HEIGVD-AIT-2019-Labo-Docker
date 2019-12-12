# Report

## Authors

- Filipe Fortunato <filipe.fortunato@heig-vd.ch>
- Pierre Kohler <pierre.kohler@heig-vd.ch>
- Jonathan Zaehringer <jonathan.zaehringer@heig-vd.ch>

## Table des matières

- [Introduction](#introduction)
- ![Task 0](./Task0.md)
- ![Task 1](./Task1.md)
- ![Task 2](./Task2.md)
- ![Task 3](./Task3.md)
- ![Task 4](./Task4.md)
- ![Task 5](./Task5.md)
- ![Task 6](./Task6.md)
- ![Difficulté](./Difficulty.md)
- [Conclusion](#conclusion)

## Introduction {introduction}

Dans ce laboratoire, nous avons détaillé la procédure pour permettre une scalability dynamique d'une infrastructure utilisant HAProxy.
Nous allons mettre en place pour ce faire le système S6 qui permet la mise en place de plusieurs processus dans un même docker pour permettre une remise en marche du processus d'application en cas de crash.
De plus, S6 nous permet de lancer un second processus qui se nomme Serf qui est un service d'autodiscovery de machine dans un réseau avec un protocole léger.
Serf permet aussi de connaître lorsqu'un noeud disparaît et permet que l'ensemble des noeuds soit mis au courant très rapidement.

Ces notifications permirent alors de constituer une liste de machine ayant un service à disposition, cette liste sera alors convertie en configuration pour HAProxy permettant alors l'utilisation de ces noeuds par le reverse proxy.

## Conclusion {conclusion}

Nous avons pu voir une manière de rendre dynamique l'utilisation de HAProxy.
Malheureusement dans notre cas, nous avions déjà travaillé avec le concurrent Traefik qui est bien plus smart sur sa manière de gestion de Docker (cf. Task5 - question4).
Ceux-ci nous a confortés sur le faite que Traefik est plus intéressant dans cette situation configuration.
Toutefois, nous avons pu tester Serf qui est une technologie intéressante pour faire de l'autodiscovery et de la gestion de perte de noeud.
Pour finir, S6 a été une découverte dans l'ensemble comme une alternative de la restriction monoprocessus de Docker, mais nous avons de la peine à adhérer à cette philosophie.
