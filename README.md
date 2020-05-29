# TP Devops


## TP 1

### - Database -

```
----------------------------------------------------
FROM postgres:11.6-alpine

COPY 01-CreateScheme.sql /docker-entrypoint-initdb.d
COPY 02-InsertData.sql /docker-entrypoint-initdb.d

ENV POSTGRES_DB=db \
	POSTGRES_USER=usr \
	POSTGRES_PASSWORD=pwd
-----------------------------------------------------
```

#### Construction de l'image
    
    docker build -t christos78p/db_postgres11.6-alpine .

#### Lancement de l'image
    
    docker run -P -v /home/vm/data:/var/lib/postgresql/data --name db_postgres11.6-alpine christos78p/db_postgres11.6-alpine



### - Multistage build -

### *Ask yourself*: Are you able to contact your SpringBook application through a web browser?
 Non je n'ai pas réussi àbuild l'image, il y'avait quelques erreurs et vu que je n'ai jamais coder en java je suis passé à la suite pour éviter de perdre trop de temmps


### - Backend API -

#### Faire en sorte que la db et l'api soit sur le meme network

    docker network ls

    docker network create toto

    docker run -p 8080:8080 --name simple_api_container --network=toto christos78p/simple_api

 ### *Ask yourself*: explore your API other endpoints, have a look at the controllers in the source code, explain each endpoint found in each controller
 Chaque endpoints permet d'acceder une page différentes et de voir des données différentes


### - Http server -

    docker build -t christos78p/apache_http .

    docker run -dit --name http_container -p 8181:8181 christos78p/apache_http

    docker exec -t -i b95c1edf767d bash

    // installation de nano et vi dans le container pour modifier le fichier de conf
    apt-get update
    apt-get install vim nano

```
------------------------------------------------
ServerName localhost

<VirtualHost *:80>
   ProxyPreserveHost On
   ProxyPass / http://localhost:8080/
   ProxyPassReverse / http://localhost:8080/
</VirtualHost>
------------------------------------------------
```


### - Link application -

### *Ask yourself*: what are the useful docker-compose sub-commands?

    docker-compose up
    docker-compose ps
    docker-compose down
    docker rm -f $(docker ps -aq)


## TP2 

### - Setup Travis CI -

#### Ok, what is it supposed to do ?

Cette commande vérifira l'ensemble des librairies utilies et les les telechargera . Elle exécutera également les tests unitaires et tests d'intégration

### What are Unit tests ? Integration test ?
 
Les tests unitaires permettent de vérifier le bon fonctionnement d'une partion de code ou d'un module en particulier 
Les tests d’intégrations permettent de tester l'ensemble des modules une fois rassemblés entres eux

### What is a db changelog job ?
“Still, how can I run a command that directly launches a docker database in which the db-changelog creates the tables and how do I link it to my application?”

On y verra les modifications liés à la base de données

### What are testcontainers?

C'est une librairie Java qui permet d'excuter plusieurs types de tests

### Why do we need this branch ?
Il faut jamais push directement sur la branch master (normalement) 
Il faut une branche develop qui sera la branche principale où le code source reprentera les derniers changements livrés pour la prochaine version

### Secured variables, why ?