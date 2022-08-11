# Apache web server

Dans cette doc nous installons Apache2 qui est un web server et nous allons y ajouter PHP de manière a pouvoir héberger une page web avec le langage server correspondant. De plus nous Nous le faisons sur une VM Debian 11, qui est une fraîche installation.

## installer apache2

1. Dans un premier temps on met a jour le dépôt des logiciels : 
```
sudo apt update & sudo apt upgrade -y
```

2. Ensuite on installe apache 2 : 
```
sudo apt-get install apache2
```

3. afin de vérifier que l’installation s’est bien effectuée on utilise la commande : 
```
sudo systemctl status apache2
```
si le service est bien actif, on peut ouvrir un onglet en localhost , une page par défaut de apache2 devrait alors être affiché.   
Dans le cas ou cela ne produit pas de résultat vérifier si vous avait un nom de système : 
```
hostname
```
et rentrer L’URL: http://resultat_du_hostname .vous devriez alors avoir un résultat.

# configurer les permissions

Dans la configuration par défaut le dossier ou se trouve les fichiers connus du server ont pour path:
```
cd /var/www/
```

En revanche seul l’utilisateur root possède les droits sur ce dossier. Or on ne souhaite pas taper un mot de passe a chaque modification, ni rencontrer de problème de droit lors de certaines actions effectué par le système. On va donc rajouter l’utilisateur actuelle dans (ou un créé spécialement pour la gestion du web server) le groupe “www-data” en charge de la plupart des processus web.  
```
sudo adduser $USER www-data
```
Cette commande rajoute l’utilisateur actuellement connecté au groupe www-data. Attention donc de ne pas être connecté a l’utilisateur root. De plus on va rajouter l’utilisateur et le groupe www-data en tant que propriétaire du dossier :  
```
sudo chown -R $USER:www-data /var/www/*
```
Enfin on va affecter les permissions 755 afin que les propriétaire du dossier puisse tout faire avec les éléments qui y sont présent. Les autres utilisateurs du groupe www-data ne pourront alors qu’exécuter et lire les fichiers. Enfin ceux ne faisant pas parti du groupe ne pourra alors qu’exécuter les fichiers.  
```
sudo chmod -R 755 /var/www
```
Bien entendu, ces permissions seront à adapter suivant la nature et la criticité du projet sur lequel vous travaillez.


# Ajouter PHP

Dans un premier temps pour ajouter PHP il faut installer les packages nécessaires. pour cela on utilise la commande :   
```
sudo apt install php libapache2-mod-php
```
On souhaite alors vérifier que l’installation s’est bien effectué :  
```
php --version
```
Enfin on va s’assurer que apache fasse bien la liaison avec PHP. Pour cela on va créer un fichier info.php dans le dossier attribué. On utilise la commande suivante pour créer et éditer le fichier :  
```
sudo nano /var/www/html/info.php
```
Une fois que vous êtes sur l’interface de modification du fichier écrivez les informations suivante :  
```
<?php 
	php.info()
?>
```
Pour sauvegarder utiliser le raccourcis Ctrl+X puis appuyer sur Y afin de confirmer les modification faite au fichier et enfin entrer pour confirmer le nom du fichier déjà a info.php  
Pour vérifier que cela fonctionne aller sur la page par défaut de votre serveur web. puis rajouter a la fin de l’URL  /info.php. On a donc quelque chose de semblable pour l’URL :  
```
http://localhost/info.php
```
Le rendu sur la page web est une page sous forme de tableau contenant un grand nombre d'information sur la version de php installé et son environnement.  
Une fois que tout fonctionne on peux utiliser la commande suivante pour supprimer le fichier info.php:  
```
sudo rm /var/www/html/info.php
```

Voila votre server Apache2 et PHP sont prêt vous pouvez donc commencer a y déployer votre page web, celle qui sera lancé lorsque seul l’IP ou nom de domaine sera tapé sera le fichier nommé index que l’extension du fichier soit .php ou .html
