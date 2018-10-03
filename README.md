# TDM-EDGE Repository

***tdm-edge*** repository is the main repository for the deployment of the TDM
***Edge Gateway*** software architecture.

As described in the ***Reference Design*** document and in the ***Manual***,
the micro-services that make up the Edge Gateway system are bundled in
***Docker*** containers and their interaction are managed by ***Docker
Compose*** tool.

Roles and collaboration between the micro-service, the *coreography*, is
described in the ***docker-compose.yaml*** file. This file contains all the
information necessary to docker-compose to setup the environment, network and
storage for the Edge Gateway system, to retrieve and update the containers and
start, stop and re-start the micro-services.

For more information about Docker and Docker Compose, refer to:

* [**Docker overview**](https://docs.docker.com/engine/docker-overview/)
* [**Overview of Docker Compose**](https://docs.docker.com/compose/overview/)

This repository contains:

* the reference ***docker-compose.yaml*** file for the TDM Edge Gateway as
  described in the ***Reference Design*** prototype and ***Manual***;
* the official ***TDM Project Deliverable*** for the Edge Gateway, namely the
  Edge Gateway ***Reference Design*** and ***Manual***.

## Documentation

### Italian Version:
* [**REFERENCE DESIGN PER SENSORISTICA DIFFUSA**](docs/refdesign/it/refdesign.md)
* [**MANUALE DI INSTALLAZIONE E CONFIGURAZIONE DEL PROTOTIPO DI EDGE GATEWAY**](docs/manual/it/manual_it.md)

### English Version:
* [**REFERENCE DESIGN FOR WIDESPREAD SENSORS TECHNOLOGY**](docs/refdesign/en/refdesign_en.md)
* [**INSTALLATION AND CONFIGURATION MANUAL OF THE EDGE GATEWAY PROTOTYPE**](docs/manual/it/manual_en.md)
