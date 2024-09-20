# Running the enrollment receiver

The enrollment receiver will de contacted directly by the enrolling users'
browser to start the authentication flow, and by the catalog to start the
actual enrollment. Therefore it must be publicly accessible, and use a valid
TLS certificate. It's advised to run the application as a docker container.

## [Run as docker docker_container](runasdockerdockercontainer)

A docker image is published [in the github registry](ghcr.io/surfnet/student-mobility-inteken-ontvanger-generiek/intekenontvanger-generiek).

Example docker-compose.yml:

```yaml
---
services:
  intekenontvanger-generiek:
    image: ghcr.io/surfnet/student-mobility-inteken-ontvanger-generiek/intekenontvanger-generiek:latest
    container_name: "intekenontvanger-generiek"
    restart: unless-stopped
    volumes:
      - ./application.yml:/application.yml
      - ./database:/database
    ports:
      - 8092:8092
```

This will start the container using the [application.yaml](./application.yaml)
placed in the same directory. By default a file-based database will be used
and places in `./database`. The database configuration can be changed in
`application.yaml`.

A reverse proxy or loadbalancer should be used to provide TLS communication
and protect the application. A traefik container could do this, if no existing services
are available; e.g.:

```yaml
---
services:
  intekenontvanger-generiek:
    image: ghcr.io/surfnet/student-mobility-inteken-ontvanger-generiek/intekenontvanger-generiek:latest
    container_name: "intekenontvanger-generiek"
    restart: unless-stopped
    volumes:
      - ./application-generiek.yml:/application.yml
      - ./database:/database
    ports:
      - 8092:8092
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.generiek.rule=Host(`enrollment.org.tld`)"
      - "traefik.http.routers.generiek.entrypoints=websecure"
      - "traefik.http.routers.generiek.tls.certresolver=myresolver"

  traefik:
    image: "traefik:v2.11"
    container_name: "traefik"
    restart: unless-stopped
    command:
      - "--log.level=DEBUG"
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.myresolver.acme.tlschallenge=true"
      - "--certificatesresolvers.myresolver.acme.email=your@email.com"
      - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
    ports:
      - "443:443"
      - "8080:8080"
    volumes:
      - "./letsencrypt:/letsencrypt"
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
```

## [JAVA process](javaprocess)

### [System Requirements](system-requirements)

- Java 8
- Maven 3

Set the JAVA_HOME property for maven (example for macOS):

```shell
export JAVA_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/
```

## [Building and running](building-and-running)

Copy the source code

```shell
git clone https://github.com/SURFnet/student-mobility-inteken-ontvanger-generiek.git
```

Copy and edit [application.yaml](./application.yaml) to the root of the project.

This project uses Spring Boot and Maven. To run locally, type:

```shell
mvn spring-boot:run
```

To build and deploy (the latter requires credentials in your maven settings):

```shell
mvn clean deploy
```
