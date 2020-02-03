# TDM-EDGE Repository

***tdm-edge*** repository is the main repository for the deployment of the ***TDM Edge Gateway*** software architecture.

In the *TDM Project Architecture for Widespread Sensors Technology*, the *Edge Gateway* is the component dedicated to *collect* data and measures from sensors and metering stations deployed on public and private buildings, and to *forward* them to the ***TDM Cloud*** for storage, processing and visualization purposes. It answers to the demands of: 

* *interfacing* etherogeneous sensors with a common data representation format;
* *dealing* with the central system according with the availability and capability of the network; 
* and to allowing for *visualization and processing* of local data.

Its software architecture is designed to run on low-cost *Single Board Computers* (SBCs) like ***Raspberry PI*** and ***BeagleBone*** and as described in the ***Reference Design*** document the micro-services that make up the Edge Gateway system are bundled in ***Docker*** containers and their interactions are managed by ***Docker Compose*** tool. Roles and collaboration between the micro-services, the *coreography*, is described in the ***docker-compose.yaml*** file.

For more information about Docker and Docker Compose, refer to:

* [**Docker overview**](https://docs.docker.com/engine/docker-overview/)
* [**Overview of Docker Compose**](https://docs.docker.com/compose/overview/)

The Edge Gateway is based on Open and standard protocols and is designed mainly to be used in a ***FIWARE-based*** infrastructure, while message formats are compatible with ***FIWARE*** and ***OASC*** standards and best practices. In particular, the ***FIWARE Data Models*** have been used for communications between the Edge Gateway and the TDM Cloud infrastructure.

For more information about FIWARE and OASC, refer to:

* [**FIWARE Framework**](https://www.fiware.org/)
* [**FIWARE Data Models**](https://www.fiware.org/developers/data-models/)
* [**OASC, Open & Agile Smart Cities Initiative**](http://oascities.org/)

## Repository Contents

This repository contains:

* the reference ***docker-compose.yaml*** file for the TDM Edge Gateway as
  described in the ***Reference Design*** prototype and ***Manual***; this file contains all the information necessary to docker-compose to setup the environment, network and storage for the Edge Gateway system, to retrieve and update the containers and start, stop and re-start the micro-services;

* the official ***TDM Project Deliverable*** for the Edge Gateway, namely the
  Edge Gateway ***Reference Design*** and ***Manual***.

## Documentation

### Italian Version:
* [**REFERENCE DESIGN PER SENSORISTICA DIFFUSA**](docs/refdesign/it/refdesign.md)
* [**MANUALE DI INSTALLAZIONE E CONFIGURAZIONE DEL PROTOTIPO DI EDGE GATEWAY**](docs/manual/it/manual_it.md)

### English Version:
* [**REFERENCE DESIGN FOR WIDESPREAD SENSORS TECHNOLOGY**](docs/refdesign/en/refdesign_en.md)
* [**INSTALLATION AND CONFIGURATION MANUAL OF THE EDGE GATEWAY PROTOTYPE**](docs/manual/en/manual_en.md)
