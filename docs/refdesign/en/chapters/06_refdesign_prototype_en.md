## 6. Implemented prototype

The prototype of the whole Edge Gateway system is composed by three elements:

* The Edge Gateway
* The environmental weather station
* The Energy measure station

The main components of the Edge Gateway are:

* Rasperry PI 3-B board
* An I2C Temperature/humidity sensor directly connected to the RPI 3
* A 16GB microSD class 10 card used to store the OS.
* A customized version of the Arch Linux OS for RPI. The customization add the following features:
	* *Access Point* mode for the Wifi needed for the first system configuration and the connection of external stations;
	* Management tools for Docker containers and the pre-installed Git repositories;
	* The script for the automatic start of Docker containers at boot time;

* Demo code for microservices:
	* dispatcher (EDGE_dispatcher) to send gathered data to the cloud;
	* weahter handler (FEINSTAUB_publisher) to implement the interface to the environmental weather station;
	* energy handler (IOTAWATT_publisher) to interface to the energy station;
	* test handler (HTU21D_publisher) for the directly connected HTU21D sensor for temperature and humidity data recording;
	* Docker container with a running InfluxDB database;
	* Docker container with a running instance of Grafana to enable the local visualization of data
	* Docker container with the local brocker of MQTT messages of Mosquitto handlers
	
The prototype has been integrated with two measure system after a phase of evaluation and testing and, in the case of the environmental weather station, a firmware customization/extension to add new functionalities.
The integrated systems are: 

* Energy monitoring system IotaWatt, with 5 current probes e 1 voltage probe connected to the main electrical panel and to the HVAC;
* Environmental weather station Stuttgart Fine Dust Sensor (Feinstub code) with:
	* Argent system anemometer and wind direction sensor;
	* Davis anemometer and wind direction sensor;
	* Argent system rain gauge sensor;
	* Hydreon RG11 infrared rain sensor;
	* temperature/Humidity sensro DHT22;
	* Nova Fitness SDS11 particulate sensor for PM10 and PM 2.5
	
Edge Gateway with  HTU21D test sensor                            | 
--------------------------------------------------------------------- | 
![Edge Gateway con il sensore di prova HTU21D](../../img/edge-htu21d.jpg) |

Environmental weather station with particulate and temperature/humidity sensor                                     |
------------------------------------------------------------------------------------------------- |
![La stazione meteo/ambientale col sensore di particolato e T/H](../../img/outdoor-meteo-station.jpg) |

Argent system anemometer, wind direction and rain gauge sensor                           |
------------------------------------------------------------------ |
![Sensori vento e pioggia Argent System](../../img/argent-system.jpg) |

Davis anemometer, wind direction sensors                   |
--------------------------------------------------- |
![Anemomentro e Segnavento Davis](../../img/davis.jpg)  |

Energy monitoring station IotaWatt  connected to the main electrical panel                        |
-------------------------------------------------------------------------------------- |
![La stazione di monitoraggio elettrico IotaWatt installata](../../img/iotawatt-panel.jpg) |

Laboratory testing of the standalone solution with the Edge Gateway and environmental weather station                              |
------------------------------------------------------------------------------------------------------------ |
![Testing in laboratorio della soluzione standalone Edge Gateway – Stazione Meteo](../../img/edge-meteo-lab.jpg) |
