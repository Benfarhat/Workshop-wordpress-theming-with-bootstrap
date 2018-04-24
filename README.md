# Creation d'un theme Wordpress paramétrable

## Avant propos

### Présentation de Wordpress
Pour rappel, Wordpress est l'un des plus connus et utilisés système de gestion de contenu open source (CMS pour Content Management System / SGC pour système de gestion de contenu), les autres étants Drupal, Jomla, Typo3.

Wordpress est très simple à prendre en main et il est extensible ou nous dirons dans notre cas malléable via les thèmes qui vous permettent de mettre en place le visuel désiré et les plugins (extensions) qui vous permettent de rajouter des fonctionalités aux CMS.

Wordpress est également un CMS multisite, c'est sa force et cette fonctionalité est simple à mettre en place. Cela veut en résumé dire que vous pouvez héberger plusieurs sites sous la même installation et donc pouvoir faire la même gestion pour plusieurs sites en même temps ce qui inclus:

- Mettre à jour wordpress
- Centraliser la gestion des utilisateurs
- Gérer les noms de domaines et sous domaines au niveau d'un même point unique
- Mettre à disposition un thème
- Mettre à disposition un plugin
- Permettre l'interaction entre les sites simplement

### Un mot sur le sécurité

Le risque du multisite est qu'une faille au niveau d'un plugin ou d'une installation, impacterait la sécurité de tous les sites hébergés sous la même instance Wordpress.
Il est donc important de prendre en compte cet aspect en adoptant les bonne pratiques de sécurité et éventuellement en faisant appel à un professionnel ou un auditeur qui localisera pour vous les éventuelles failles trouvées au moment de son audit de sécurité.
Il est conseillé de:

- Modifier régulièrement vos SALTs de sécurité au niveau du fichier wp-config.php via éventuellement le site: [https://api.wordpress.org/secret-key/1.1/salt/](https://api.wordpress.org/secret-key/1.1/salt/), vous pouvez les modifier à tout moment, il sont utiliser pour vos cookies et ainsi, forcera les utilisateurs ayant un cookie invalide de se reconnecter (le cookie contiennent les username et mot de passe qui sont hashés via hash_hmac en utilisant les salts que vous aurez donné).
- Supprimer les utilisateurs inactifs
- Forcer l'utilisation de https, il faudra éventuellement avoir un certificat (Let's encrypt offre gratuitement des certificats à cette adresse: [https://letsencrypt.org/](https://letsencrypt.org/))
    - Ajouter dans wp-config.php: `define('FORCE_SSL_ADMIN', true);`
- Sécuriser les accès aux fichiers wp-login.php, xmlrpc.php, wp-admin/admin-ajax.php
- vérifier bien les droits d'accès aux différents élements de wordpress et les restreindre au maximum 
- Restreindre les droits d'accès de l'utilisateur de la base de donnée mysql
    - Voir configuration au niveau de wp-config.php, votre utilisateur ne doit avoir que les privilèges:
        - `select`
        - `insert`
        - `update`
        - `delete`
- Eviter lors de l'installation en production de wordpress, d'utiliser le prefix des tables par défaut, cela permet d'éviter à une personne malintentionné, d'avoir le nom de vos tables
    - Au niveau du fichier wp-config.php modifier la clé: `$table_prefix  = 'wp_';`
- Désactivez l'éditeur de code dans l'administration pour qu'il ne soit plus disponible (et donc modifiable)
    - Mettre dans wp-config.php: `define( 'DISALLOW_FILE_EDIT', true );`
- Mettre automatiquement à jour le core de wordpress, les thèmes les traductions et les plugins
    - Au niveau de functions.php ajouter:
        - `add_filter( 'auto_update_plugin', '__return_true' );`
        - `add_filter( 'auto_update_theme', '__return_true' );`
        - `add_filter( 'auto_update_translation', '__return_false' );`
- Supprimer les extensions et thèmes non utilisés
- Ne pas utiliser de modules et thèmes de source non fiable (exiger un ensemble d'audit si vous avez des doutes)
- Désactiver les messages de debug 
    - Dans wp-config.php vérifier que vous avez: `define('WP_DEBUG', false);`
- Choisissez de bons mots de passe à modifier les régulièrement, ne pas permettre de "social engineering" en utilisant des mot de passe qui est en relation avec la personne (date de naissance, non de famille, numéro de portable) et ne pas utiliser de mots de passe appartenant à au dictionnaire des mots de passe les plus courant (password, 123456, secret, etc..), ne pas noter vos mots de passe sur un fichier dans votre PC ou sur un sticker.
    - Il est préférable de choisir un mot de passe via une méthode mnémothecnique 
        - faire des remplacements de lettres (a par @ puis 4 en alternance, s par $, i par 1, e par 3 et € par alternance) puis une alternance minuscules majuscules et inversement ...
            - Ainsi administration serait `@dm1NsTr4T10n`
        - utiliser les premières lettres ou dernières lettres d'une phrase, ou les deux premières de chaque et utiliser la technique précédente
            - Ainsi "Ma ville de naissance est Paris" permettrait d'avoir: `M@v1D3n43$P@`
    - Vous pouvez ajouter si vous en avez la possibilité, ajouter une ou plusieurs lettre d'un autre encodage, par exemple `Elyesبن` 
    - Attention aux sites web qui vous permettent de tester vos mots passes, la majorité enregistrent à votre insu vos suggestions dans un gigantesque dictionnaire de correspondances plus grand, Pour un exemple de dictionnaire dans un autre domaine de sécurité (celui des mots de passes hashés en SHA256) il existe par exemple des dictionnaires qui ont plus de 3.7 milliards de correspondances! Soyez donc prudent.

Pour plus d'infos voir: [https://codex.wordpress.org/Hardening_WordPress](https://codex.wordpress.org/Hardening_WordPress)

### Notice
 
Lorsqu'on fait un thème avancé, il est possible également de rajouter de nombreuses fonctionalités, il est toujours recommandés de garder à l'esprit la finalité d'avoir un thème facile à utiliser par une autre personne que vous, si par exemple vous modifier le menu de base de wordpress, il ne faut pas brusquer l'utilisateur finale en lui mettant en place un cockpit d'avion qui pour l'utilisateur ee sera pas aussi cohérent et simple que vous en tant que développeur ne le voyez.

## Préparation du workshop

### Serveur web

Avant de commencer vous devez avoir un serveur web sur votre machine.

- Wampserver pour windows: [http://www.wampserver.com/](http://www.wampserver.com/)
- Lamp pour Ubuntu / Debian: [https://doc.ubuntu-fr.org/lamp](https://doc.ubuntu-fr.org/lamp)
- Mamp pour Mac: [https://www.mamp.info/en/](https://www.mamp.info/en/)

### Alternative

Il est possible d'utiliser pour vos développements la version gratuite de DesktopServer qui offre un couple serveur web / wordpress simple à utiliser.

DesktopServer est disponible ici: [https://serverpress.com/](https://serverpress.com/)

### Adresse IP du serveur web

Notez que vous pouvez installer votre serveur en local ou sur une autre machine qui peut être distante ou virtuel via par exemple [VMWare workstation player](https://www.vmware.com/products/workstation-player.html) ou [VirtualBox](https://www.virtualbox.org/) qui vous permettent tout deux d'installer un autre système d'exploitation pour éventuellement y faire vos test.

Quelque-soit votre mode d'installation, vous devriez noter l'adresse IP (via la commande ipconfig sur windows ou ifconfig sur linux, Mac ou un sous système linux sous Window / WSL) de la machine sur laquelle est installé le serveur web. C'est cette adresse IP qui sera utilisée au niveau de votre navigateur pour accéder à votre site-web.

### Troubleshooting

Si lors de l'accès à votre serveur web via votre navigateur vous n'arrivez pas à obtenir un accès alors:

- Essayer de voir si le serveur répond
    - Si le serveur est sur votre machine local via un simple `ping 127.0.0.1` (127.0.0.1 est une adresse IP spéciale qui veut dire "moi même")
    - Si le serveur est distant via un `ping` sur l'adresse du serveur distant
        - Noter que si vous êtes en local, votre adresse et celle du serveur doivent être sur la même adresse réseau, pour faire simple, une adresse est composée de 4 chiffres (allant de 0 à 255), le masque réseau aussi, si avec un peu de chance votre masque réseau est composé seulement de 0 et 255 alors la ou il y a 255, il doit au niveau des deux adresses IP y avoir les même numéros (si par exemple le masque est de 255.255.0.0 alors les deux numéros de gauche doit être identique, s'il est de 255.255.255.0 c'est les 3 premier)
        - Si votre machine est le serveur ne sont pas sur le même réseau alors il doit y avoir un équipement dit "routeur" qui fera le lien entre les deux réseaux logiques
- Essayer de voir si votre serveur web fonctionne via:
    - Un indicateur visuel dans la barre des taches
    - Une commande spécifique du genre `service apache2 status` sur Linux
    - Une commande spécifique du genre `netstat -a` permettant de constater si les ports 80 e(pour le serveur web) et 3306 (pour mysql) sont en écoute (LISTENING)


### Données de travail

Vous aurez besoin d'avoir sur votre installation des données de travail (fake ou dummy data) permettant de [tester votre thème](https://codex.wordpress.org/Theme_Development#Theme_Testing_Process), il existe un fichier dit Thème Unit Test Data qui est un [fichier XML](https://raw.githubusercontent.com/WPTRT/theme-unit-test/master/themeunittestdata.wordpress.xml) dit WordPress eXtended RSS ayant un format particulier et compréhensible par wordpress (WXR), à importer via tools > import > WordPress Si vous ne voyez pas de lien vers "Run Importer" cliquer sur "Install Now" près de Wordpress pour installer le module d'import.
En cliquant sur Run importer vous avez juste a sélectionner le fichier xml téléchargé et cliquer sur le bouton "import file and import". Puisque le fichier comportera des articles, il vous sera demander si vous voulez automatiquement créer les noms des auteurs trouvés dans le fichier XML ou d'utiliser un des utiliateurs existant dans votre installation. Puis vous pouvez également importer les attachements (lien vers un fichier externe) et au final valider en cliquant sur "submit"



