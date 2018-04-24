# Creation d'un theme Wordpress paramétrable

## Avant propos

Pour rappel, Wordpress est l'un des plus connus et utilisés système de gestion de contenu open source (CMS pour Content Management System / SGC pour système de gestion de contenu), les autres étants Drupal, Jomla, Typo3.

Wordpress est très simple à prendre en main et il est extensible ou nous dirons dans notre cas malléable via les thèmes qui vous permettent de mettre en place le visuel désiré et les plugins qui vous permettent de rajouter des fonctionalités aux CMS.

Lorsqu'on fait un thème avancé, il est possible également de rajouter de nombreuses fonctionalités, il est toujours recommandés de garder à l'esprit la finalité d'avoir un thème facile à utiliser par une autre personne que vous, si par exemple vous modifier le menu de base de wordpress, il ne faut pas brusquer l'utilisateur finale en lui mettant en place un cockpit d'avion qui pour l'utilisateur ee sera pas aussi cohérent et simple que vous en tant que développeur ne le voyez.

## Préparation du workshop

### Serveur web

Avant de commencer vous devez avoir un serveur web sur votre machine.

- Wampserver pour windows: [http://www.wampserver.com/](http://www.wampserver.com/)
- Lamp pour Ubuntu / Debian: [https://doc.ubuntu-fr.org/lamp](https://doc.ubuntu-fr.org/lamp)
- Mamp pour Mac: [https://www.mamp.info/en/](https://www.mamp.info/en/)

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
- 





Il est possible d'utiliser pour vos développement la version gratuite de DesktopServer qui offre un couple serveur