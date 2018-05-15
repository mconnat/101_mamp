À partir du Day 03 les sujets vont mentionner un soft qui se nomme PAMP (accompagné d'une vidéo rigolote de Ly) pour faire tourner un serveur php, apache, mysql.
Malheureusement, ce soft ne tourne pas sur des versions supérieures à El Capitan :(

Donc, avec Bodo, nous vous avons préparé un Docker Compose qui va remplacer ce soft (avec un tuto en plus, petits veinards) !
### DOCKER

Pourquoi ? 

Parce que c'est bien. Et puis aussi parce que c'est bien ! Vraiment bien ! Vraiment VRAIMENT bien !
Docker compose va vous monter un environnement, isolé, de containers docker pour faire tourner les services requis.
C'est très utile pour un environnement de dev, peu importe l'Os sur lequel vous allez travailler !
Je vous invite à vous renseigner sur cette techno, car il y a beaucoup d'autre cas d'utilisation ;)

Pour notre utilisation, rien de sorcier, vous allez devoir tout simplement cloner ce repo et faire des modifications dans un fichier.

# Installation

Obviously, `Docker` est requis, il est disponible dans tous les MSC du coin !

Cloner le repo suivant quelque part dans votre home :

```sh
git clone https://github.com/mconnat/101_mamp
```

Voici la structure:

```
101_mamp
└───config
│   └───php
│       └───7.0
│         │ php.ini
│         │
└───data
│   └───www
│     │ index.php
│     │
docker-compose.yml
```

Le fichier, le plus important est `docker-compose.yml` :
  

```yaml
version: '2'
services:
  web:
    image: keopx/apache-php:7.0
    ports:
      - "8008:80"
    links:
      - mysql
    volumes:
      - ./data/www:/var/www/html 
      - ./config/php/7.0/php.ini:/etc/php/7.0/apache2/php.ini
    working_dir: /var/www/html
  mysql:
    image: keopx/mysql:5.5
    ports:
      - "3306:3306"
    volumes:
      - ./data/database:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=testroot  
      - MYSQL_DATABASE=testdb         
      - MYSQL_USER=testuser           
      - MYSQL_PASSWORD=testpass       
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8080:80"
    links:
      - mysql
    environment:
      - PMA_HOST=mysql
```

Il contient, entre autres, des informations comme : 
  * Les images Docker que nous allons utiliser (eg. `image: keopx/apache-php:7.0`)
  * Votre dossier de travail local qui sera monté sur le container et son emplacement sur celui-ci (`./data/www:/var/www/html`)
  * Les credentials de votre base de donnée SQL (eg. ` MYSQL_USER=testuser`)

Les points listés ci-dessus sont ceux qui nous intéressent, il y a d'autres instruction que nous vous laisserons découvrir par vous-même.

### Configuration

Voici les modifications à faire :
```yaml
  - ./data/www:/var/www/html
```

C'est le dossier dans lequel vous aurez vos `.php`, autrement dit, votre site. 
Nous vous conseillons de le changer pour un emplacement en dehors de ce repo (eg. `/Users/login/piscine-php/MyWebSite`).

```yaml
- MYSQL_ROOT_PASSWORD=testroot  
- MYSQL_DATABASE=testdb         
- MYSQL_USER=testuser           
- MYSQL_PASSWORD=testpass 
```
Ce sont des variables qui seront load au lancement du container pour sa configuration.
Leurs noms sont plutôt explicites.

### Exemple
Voici un exemple de configuration pour l'user `chmaubla`:
```yaml
version: '2'
services:
  web:
    image: keopx/apache-php:7.0
    ports:
      - "8008:80"
    links:
      - mysql
    volumes:
      - /Users/chmaubla/piscine-php/MyWebSite:/var/www/html 
      - ./config/php/7.0/php.ini:/etc/php/7.0/apache2/php.ini
    working_dir: /var/www/html
  mysql:
    image: keopx/mysql:5.5
    ports:
      - "3306:3306"
    volumes:
      - ./data/database:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=lib_standard_en_php
      - MYSQL_DATABASE=chmaubla_db    
      - MYSQL_USER=chmaubla           
      - MYSQL_PASSWORD=mypassword     
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8080:80"
    links:
      - mysql
    environment:
      - PMA_HOST=mysql
```

# Utilisation

### Start du service

Il suffit d'être dans le dossier courant du `docker-compose.yml` et d'exécuter (après avoir fait vos modifs bien entendu et lancer Docker) :

```sh
docker-compose up
```

Vous pouvez même rajouter l'option `-d` pour lancer les services en mode détaché.

Si vous avez décidé d'utiliser cette option, il vous faudra utiliser la commande `docker-compose stop` pour arrêter les services sinon un bon `Ctrl-C` des familles en mode non détaché.

### Dev

Vous devrez donc mettre vos fichiers `*.php.` dans le dossier que vous avez renseigné dans le `docker-compose.yml`.
Pour accéder au site, il faudra rentrer comme URL `http://localhost:8008` ou `http://zXrXpX.le-101.fr:8008` ou `http://0.0.0.0:8008` ou `http://10.X.X.X:8008` ou ...

Il y a aussi un container PhpMyAdmin atteignable sur `http://localhost:8080` ou `http://0.0.0.0:8080` ou ...

### Problèmes ?

Vous êtes grands, démerdez-vous.

# Bon projets!

