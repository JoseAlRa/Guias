el código es de https://www.linuxserver.io

https://github.com/linuxserver/docker-unifi-network-application

esta es la configuración de funciono en portainer

---
services:
  unifi-db:
    image: docker.io/mongo:4.4.29
    container_name: unifi-db
    volumes:
      - /home/soporte/unifi_network_application/db:/data/db
      - /home/soporte/unifi_network_application/db/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro
    restart: unless-stopped
  unifi-network-application:
    image: lscr.io/linuxserver/unifi-network-application:latest
    container_name: unifi-network-application
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Costa_Rica
      - MONGO_USER=unifi
      - MONGO_PASS=lscr_io1
      - MONGO_HOST=unifi-db
      - MONGO_PORT=27017
      - MONGO_DBNAME=unifi
      - MEM_LIMIT=1024 #optional
      - MEM_STARTUP=1024 #optional
      #- MONGO_TLS= #optional
      #- MONGO_AUTHSOURCE= #optional
    volumes:
      - /home/soporte/unifi_network_application:/config
    ports:
      - 8443:8443
      - 3478:3478/udp
      - 10001:10001/udp
      - 8080:8080
      - 1900:1900/udp #optional
      - 8843:8843 #optional
      - 8880:8880 #optional
      - 6789:6789 #optional
      - 5514:5514/udp #optional
    restart: unless-stopped

el archivo init-mongo.js  su contenido es 

db.getSiblingDB("unifi").createUser({user: "unifi", pwd: "lscr_io1", roles: [{role: "dbOwner", db: "unifi"}]});
db.getSiblingDB("unifi_stat").createUser({user: "unifi", pwd: "lscr_io1", roles: [{role: "dbOwner", db: "unifi_stat"}]});