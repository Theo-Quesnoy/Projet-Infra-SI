# Projet Infra/SI

## Sommaire
[1. Prérequis](#1-prérequis)

[2. Mise en place de la solution](#2-mise-en-place-de-la-solution)

[3. Utilisation de la solution](#3-utilisation-de-la-solution)

[4. Matrice de test](#4-matrice-de-test)


## 1. Prérequis
- Avoir un hyperviseur (VirtualBox, VMWare, Hyper-V, ...)

## 2. Mise en place de la solution
- Création de 5 VM (1 par serveur) Debian 11
- Création d'un utilisateur `user1` avec les droits sudo sur chaque serveur 
    ```bash
    $ useradd -m -s /bin/bash user1
    $ su -
    $ apt update
    $ apt install -y sudo
    $ visudo
    -----------------
    user1       ALL=(ALL:ALL) ALL
    ```
- Installation de Docker sur chaque serveur
    ```bash
    $ sudo apt update
    $ sudo apt install ca-certificates curl gnupg
    $ sudo install -m 0755 -d /etc/apt/keyrings
    $ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    $ sudo chmod a+r /etc/apt/keyrings/docker.gpg
    $ echo \
     "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
     "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    $ sudo apt update
    $ sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
- Création des fichiers nécéssaires sur chaque serveur
    * Récupérer le contenu du fichier `docker-compose.yml` adéquat (selon le serveur souhaité) et l'inclure dans le fichier `docker-compose.yml` créé dans le serveur.
    * Récupérer le contenu du fichier `keepalived.conf` adéquat et l'inclure dans le fichier `keepalived.conf` créé dans les serveurs WordPress [1](./VM_Wordpress1/keepalived.conf) et [2](./VM_Wordpress2/keepalived.conf), ainsi que sur les serveurs NGINX [1](./VM_NGINX1/keepalived.conf) et [2](./VM_NGINX2/keepalived.conf).
    * Récupérer le contenu du fichier `nginx.conf` adéquat et l'inclure dans le fichier `nginx.conf` créé dans les serveurs NGINX [1](./VM_NGINX1/nginx.conf) et [2](./VM_NGINX2/nginx.conf).

## 3. Utilisation de la solution
### Commandes Docker :
- Création et démarrage des contenaires Docker :
    ```bash
    $ sudo docker compose up -d
    ```
- Arret des contenaires Docker :
    ```bash
    $ sudo docker compose stop
    ```
- Détruire les contenaires Docker :
    ```bash
    $ sudo docker compose down
    ```

Une fois tout les contenaires démarrés, il est possible d'accéder au site WordPress via la VIP des serveurs NGINX : 192.168.20.210

![](https://hackmd.io/_uploads/ryZXX_vH3.png)


### 4. Matrice de test

- Si vous éteignez une VM Wordpress ou NGINX, la page d'acceuil restera telle qu'elle est

- Si vous éteignez la VM de la base de données, la page changera et indiquera que la BDD n'est pas accessible

![](https://hackmd.io/_uploads/rJo_Xuvrn.png)
