# Gitlab-Docker

# Installation / Configuration et Tests CICD
   - www.n-services.net
   - NEWTON SERVICES
   - http://xxxxxxxxxxxx:9080
#

# Installation / Configuration

1- Connexion:
   - ssh 

2- Installation de Docker et docker compose :
   - sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugins.

3- Création des repertoires :
   - Gitlab : compose.yaml
     * scp compose.yaml root@xxxxxxxxx:/tmp
     * mkdir -p /opt/outils/gitlab
     * mv /tmp/compose.yaml/ opt/outils/gitlab
     * cd /opt/outils/gitlab
     * Creation du réeau
     * docker network create applications-networks
     * mkdir gitlab-data
     * chmod 666 gitlab-data
     * docker compose up -d
     * connexion à Gitlab
     * docker exec -it gitlab-ce sh
     * ls -al /etc/gitlab
     * cat /etc/gitlab/initial_root_password
 
   - Gitlab-runner : compose.yaml
     * scp compose.yaml root@xxxxxxxx:/tmp
     * cd /opt/outils
     * mkdir gitlab-runner
     * cd gitlab-runner
     * cp /tmp/compose.yaml .
     * mkdir data
     * mkdir configs
       - vim configs/config.toml
       - rajouter :
       - check_internal = 0
       - shutdown_timeout = 0
     * docker compose up -d

4- Création de Runner

   * Se placer dans le dossier gitlab 
   * docker exec -it gitlab-ce cat /etc/gitlab/gitlab.rb
   * mkdir configs
   * docker exec -it gitlab-ce cat /etc/gitlab/gitlab.rb > configs/gitlab.rb
   * vim configs/gitlab.rb
   * external_url = ‘http :xxxxxxxxx :9080’
   * nginx[‘liseten_port’] = 9080
   * dans compose.yaml rajouter dans volume :  - './configs/* * gitlab.rb:/etc/gitlab/gitlab.rb'
   * ensuite docker compose stop docker compose rm -f
   * docker compose up -d

5- Déclaration de Runner

   * docker exec -it gitlab-runner
   * gitlab-runner register  
   * --url http://gitlab-ce:9080
   * --token glrt-PH-ieqo7Crbhk_kmGEak
   * --executor docker
   * --docker-image node:21
   * --docker-network-mode applications-networks

6- Rajout de volume
   * cd ../gitlab-runner
   * cat configs/config.toml
   * 1) volumes = ["/cache"]
   * vim configs/config.toml
   * 2) volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]


# Tests CICD

1- Création de fichier .gitlab-ci.yml







