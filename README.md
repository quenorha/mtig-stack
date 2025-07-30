ENGLISH VERSION
================
(French version below)

The purpose of this repo is to deploy a MTIG stack : Mosquitto Telegraf InfluxDB Grafana

# Prerequisites
- Docker up and running
- Date/Time up-to-date (NTP prefered)
- On WAGO controllers (except PFC300 750-8302) Docker should be configured to use memory card. For this replace the /etc/docker/daemon.json by the file provided in conf folder, and restart Docker or the controller.
  Memory card should be formatted in ext4.

# Install with Portainer

## Portainer installation (not needed on Edge computer, Portainer is already installed)
- Create Portainer volume
```
docker volume create portainer_data
```

- Deploy Portainer container 
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:lts
```

- Access Portainer https://X.X.X.X:9443 and set up password for the first use.

## Download configuration file
- Download configuration files telegraf.conf and mosquitto.conf located in conf directory of the Github repo and place it in /root/conf directory.
(You can place it anywhere else and change the configuration in docker-compose accordingly.)

- To do this in command line :
```
mkdir -p /root/conf
cd /root/conf
curl -k -O -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/conf/{telegraf.conf,mosquitto.conf}
```

## Stack deployment

In Portainer : 
- Go in Stack / Add Stack
- Give a name to your stack

- If you select Web editor, copy paste the content of docker-compose.yml in editor and click on Deploy the stack

OR

- Alternatively, select Repository, and add https://github.com/quenorha/mtig-stack in the repository URL field, click on Deploy the stack.

Portainer will use docker compose to install the stack.


# Install with CLI (without Portainer)

## On Edge Computer

- On an Edge Computer, you might need to install Docker compose and Git: 
```
sudo apt-get update
sudo apt-get install docker-compose-plugin git
```

- Download the docker compose file and deploy the stack :
```
git clone https://github.com/quenorha/mtig-stack.git && docker-compose up -d
```


## On other WAGO Controllers

### Manual installation 
If you want to do it manually : 

- Download configuration file
```
mkdir -p /root/conf
curl -k -O -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/conf/{telegraf.conf,mosquitto.conf}
```

- Download Docker compose file
```
curl -k -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/docker-compose.yml
```

- Use docker container as docker-compose command
```
docker run --rm -t \
--privileged \
-v $(pwd):/compose \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /usr/bin/docker:/usr/bin/docker \
wagoautomation/docker-compose -f docker-compose.yml up
```

Optionally if you want to use docker-compose command in short form you can use : 

```
alias docker-compose='docker run --rm -t --privileged -v $(pwd):/compose -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker wagoautomation/docker-compose'
```

And then 
```
docker-compose -f docker-compose.yml up
```

### Automatic installation 

```
curl -k -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/mtig-stack-install && chmod +x mtig-stack-install && ./mtig-stack-install
```

FRENCH VERSION
================


Le but de ce repo est de déployer une pile MTIG : Mosquitto Telegraf InfluxDB Grafana


# Prérequis
- Docker activé
- Date/Heure à jour (NTP de préférence)
- Sur les contrôleurs WAGO (sauf PFC300 750-8302) Docker doit être configuré pour utiliser une carte mémoire. Pour cela remplacer le fichier /etc/docker/daemon.json par le fichier fourni dans le dossier conf, et redémarrer Docker ou le contrôleur. La carte mémoire devrait être formatée en ext4.

# Installer avec Portainer

## Installation de Portainer (pas nécessaire sur l'ordinateur Edge, Portainer est déjà installé)
- Créer un volume Portainer
```
docker volume create portainer_data
```

- Déployer le conteneur Portainer 
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker. sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:lts
```

- Accéder à Portainer https://X.X.X.X:9443 et définir le mot de passe pour la première utilisation.

## Télécharger le fichier de configuration
- Télécharger les fichiers de configuration telegraf.conf et mosquitto.conf situés dans le répertoire conf du repo Github et les placer dans le répertoire /root/conf.
(Vous pouvez le placer n'importe où ailleurs et changer la configuration dans docker-compose en conséquence).

- Pour faire cela en ligne de commande :
```
mkdir -p /root/conf
cd /root/conf
curl -k -O -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/conf/{telegraf.conf,mosquitto.conf}
```

## Déploiement de la Stack

Dans Portainer : 
- Allez dans Stack / Add Stack
- Donnez un nom à votre stack

- Si vous sélectionnez Web editor, copiez-collez le contenu de docker-compose.yml dans l'éditeur et cliquez sur Deploy the stack

OU

- Alternativement, sélectionnez Repository, et ajoutez https://github.com/quenorha/mtig-stack dans le champ repository URL, cliquez sur Deploy the stack.

Portainer utilisera docker compose pour installer la pile.


# Installer avec CLI (sans Portainer)

## Sur un Edge computer

- Sur un ordinateur Edge, vous pourriez avoir besoin d'installer Docker compose et Git : 
```
sudo apt-get update
sudo apt-get install docker-compose-plugin git
```

- Téléchargez le fichier docker compose et déployez la pile :
```
git clone https://github.com/quenorha/mtig-stack. git && docker compose up -d
```


## Sur d'autres contrôleurs WAGO

### Installation manuelle 
Si vous voulez le faire manuellement : 

- Téléchargez le fichier de configuration
``
mkdir -p /root/conf
curl -k -O -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/conf/{telegraf.conf,mosquitto.conf}
```

- Télécharger le fichier Docker compose
```
curl -k -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/docker-compose.yml
``

- Utiliser le conteneur docker comme commande docker-compose
```
docker run --rm -t \N-
--privileged \N-
-v $(pwd):/compose \N-
-v /var/run/docker.sock:/var/run/docker. sock \N-
-v /usr/bin/docker:/usr/bin/docker \N-
wagoautomation/docker-compose -f docker-compose.yml up
```

Optionnellement, si vous voulez utiliser la commande docker-compose sous forme abrégée, vous pouvez utiliser : 

```
alias docker-compose='docker run --rm -t --privileged -v $(pwd):/compose -v /var/run/docker.sock:/var/run/docker. sock -v /usr/bin/docker:/usr/bin/docker wagoautomation/docker-compose'
```

Et ensuite 
```
docker-compose -f docker-compose.yml up
```

### Installation automatique 

```
curl -k -O https://raw.githubusercontent.com/quenorha/mtig-stack/refs/heads/main/mtig-stack-install && chmod +x mtig-stack-install && ./mtig-stack-install
````
