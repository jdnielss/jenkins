~~~~
version: '3.8'

services:

  jenkins:
    image: jdniels/jenkins
    container_name: jenkins
    environment:
      - DOCKER_HOST=tcp://dockersock:2375
    depends_on:
      - dockersock
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
    networks:
      - jenkins_net
    restart: unless-stopped

  dockersock:
    image: jdniels/dockersocat
    container_name: dockersocat
    userns_mode: "host"
    privileged: true
    expose:
      - "2375"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - jenkins_net
    restart: unless-stopped

volumes:
  jenkins_home:
        driver: local

networks:
  jenkins_net:
    name: jenkins_net
    driver: bridge
~~~~
