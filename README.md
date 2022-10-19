# test

```yaml
version: "3.9"

networks:
  jenkinsinternal:
    external: false
volumes:
  jmaster_home:

services:
  jmaster:
    image: "{{ docker_image }}"
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size:  "200m"
        max-file: "3"
    networks:
      jenkinsinternal:
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jmaster_home:/var/jenkins_home
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro

#  jslave:
#    build:
#      context: slave/
#      args:
#        - http_proxy
#        - https_proxy
#        - no_proxy
#        - JAVA_PROXY
#    restart: unless-stopped
#    logging:
#      driver: "json-file"
#      options:
#        max-size:  "200m"
#        max-file: "3"
#    volumes:
#      - /var/run/docker.sock:/var/run/docker.sock
#      - /var/lib/docker:/var/lib/docker
#      - $HOME/jenkins-backup/slave/gradle:/root/.gradle
#      - $HOME/jenkins-backup/workspace:/root/workspace
#      - /etc/localtime:/etc/localtime

#  registry:
#    image: registry:2.8.1
#    restart: unless-stopped
#    ports:
#      - 5000:5000
#    volumes:
#      - $HOME/jenkins-backup/registry:/var/lib/registry
#      - /etc/localtime:/etc/localtime

#  registry-web:
#    image: konradkleine/docker-registry-frontend:v2
#    restart: unless-stopped
#    ports:
#      - 8081:80
#    environment:
#      - ENV_DOCKER_REGISTRY_HOST=registry
#      - ENV_DOCKER_REGISTRY_PORT=5000
#      - ENV_MODE_BROWSE_ONLY=true
#    volumes:
#      - /etc/localtime:/etc/localtime

```
