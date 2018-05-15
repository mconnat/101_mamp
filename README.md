Vous l’avez peut être remarqué, pour la piscine PHP, 42 utilise un soft du nom de PAMP.

Ici au **101**, nous allons tenter autre chose :

### DOCKER

Pourquoi ? Parce que c'est bien. Et puis aussi parce que c'est bien! Vraiment bien! Vraiment VRAIMENT bien!

Pour cela, vous allez devoir tout simplement clôner un repo et faire 2/3 modifs toutes simples.

# Installation

Il vous faut d'abord clôner le repo suivant quelque part dans votre home :

```sh
git clone https://github.com/mconnat/101_mamp
```

Sur ce repo, vous aurez 3 fichiers:

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

Vous devrez modifier le `docker-compose.yml`. Dans ce dernier, nous trouvons ceci :

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
      - ./data/www:/var/www/html #                                à modifier
      - ./config/php/7.0/php.ini:/etc/php/7.0/apache2/php.ini
    working_dir: /var/www/html
  mysql:
    image: keopx/mysql:5.5
    ports:
      - "3306:3306"
    volumes:
      - ./data/database:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=testroot  #                           à modifier
      - MYSQL_DATABASE=testdb         #                           à modifier
      - MYSQL_USER=testuser           #                           à modifier
      - MYSQL_PASSWORD=testpass       #                           à modifier
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8080:80"
    links:
      - mysql
    environment:
      - PMA_HOST=mysql
```

Les seules lignes à modifier sont celles indiquées ci-dessus.

La première modification est importante : elle indique où votre site se situe. Nous vous conseillons de le changer pour quelque chose du style `/Users/login/piscine-php/`.

La seconde modification concerne l'accès à votre base de données. À vos risques et périls.

# Utilisation

Il faut, obviously, que vous alliez au préalable installer `Docker`, disponible dans tous les MSC du coin!

### Start du service

Il suffit d'être dans le dossier courant du `docker-compose.yml` et d'exécuter (après avoir fait vos modifs bien entendu et lancer Docker) :

```sh
docker-compose up
```

Vous pouvez même rajouter l'option `-d` pour lancer les services en tâche de fond.

Si vous avez décidé d'utiliser cette option, il vous faudra utiliser la commande `docker-compose stop` pour arrêter les services.

### Dev

Vous devrez donc bosser dans le dossier que vous avez renseigné dans le `docker-compose.yml` et pour accéder au site, il faudra rentrer comme URL `http://localhost:8008` ou `http://zXrXpX.le-101.fr:8008` ou `http://0.0.0.0:8008` ou `http://10.X.X.X:8008` ou ...

### Problèmes ?

Vous êtes grands, démerdez-vous.

# Bon projets!