## 4. THE EDGE GATEWAY SOFTWARE PLATFORM
### 4.1 GENERAL ARCHITECTURE
The Edge Gateway has the task of interfacing with local sensors or remote
measurement stations present in the building in which it is installed. Although
a set of these have been identified as prototypes to be used for development
and testing, the aim of the Edge Gateway is to allow a transparent integration
of new sensors, data types and interfacing methods. Its software must therefore
allow a high flexibility and at the same time a good resilience to errors and
problems induced by new components.

The software architecture chosen for the implementation of the Edge Gateway is
defined as '*Choreography of loosely-coupled, containerized microservices*',
i.e. it is a composition of loosely coupled microservices distributed in
application containers. The heart of the architecture is therefore constituted
by a network of microservices connected together in such a way that the
unavailability of one or more of them does not affect the functioning of the
remaining ones and of the system as a whole.

A complete reference implementation has been carried out in the framework of
the TDM project and is made available on ***GITHUB*** at the following address:
<https://github.com/tdm-project>.

Block Diagram of the Edge Gateway                           |
-------------------------------------------------------------| 
![Block Diagramof the Edge Gateway](../../img/EG-2018-05.png) |

### 4.2 MICROSERVICES 
In a monolithic application, all functionality are implemented in a single
executable unit. All the internal components share resources such as the same
memory space and file handlers, and a bug in one of the modules or threads can
result in the corruption of resources used by the rest of the application, that
is in the propagation of the bug. Updates and bug-fixing also needs to be
developed, tested and released for the entire application, complicating all
phases and introducing new breakpoints. In the same way the addition of new
features involves a review of the entire application and its replacement, with
times and costs not easily determined in advance.

In an application based on microservices, however, the various macro-functions
are separated and assigned to smaller applications dedicated to providing only
that specific functionality. This brings several advantages:

* once the tasks and communication interfaces have been defined, the various
  microservices can be developed simultaneously and independently, released at
different times and maintained separately;
* the reduction of the size of the single micro-service allows an easier
  revision of the code, reducing the possibility of introducing bugs and
facilitating their identification;
* the overall application is more resilient, reducing the possibility of single
  point-of-failure and the propagation of failures or interruptions from one
module to others;
* updating or adding new modules does not require replacement or shutdown of
  the others.

### 4.3 APPLICATION CONTAINERS

The software architecture of the Edge Gateway provides for the distribution of
microservices through '*application containers*'. Containers can be considered
software packages containing the main application to be executed and all and
only the libraries and files required by it. Unlike a virtual machine, to which
they can be compared, they do not require a complete operating system and their
own running kernel, but use the functionality of the running kernel in the host
system. Unlike a virtual machine, they also share the same resources made
available by the host system such as disk space, memory, and network, avoiding
unnecessary limitations and fragmentation while maintaining separation of
running processes and exclusive access to the resources used.

The advantages of using containerisation for the execution of microservices
becomes evident, however, considering the aspect of distribution. Different
applications may require different versions for the same library. In a shared
environment such as the host operating system, this could lead to a break in
the dependency chain: the adoption of one version prevents the execution of the
others. This problem, although not initially clear, can occur later in the life
of a product when an update is needed: the update of a library required by a
micro-service can damage another micro-service that did not need that update.
This problem is avoided by distributing an application along with its own copy
of the libraries in a dedicated and isolated environment. Another problem for
which containerisation is a solution is reproducibility. The container is
distributed with the application, files and libraries with which it has been
developed and tested. If the application works in the container when it is
released, since the host operating environment does not affect the container's
operation, it will work once deployed.

The technology used for containerizing microservices in the Edge Gateway is the
Docker platform. The tools provided by Docker allow the creation, updating,
*versioning* and execution of containers for microservices. In particular, a
tool, *docker-compose*, allows the start-up and composition of different
microservices configuring aspects such as connections, storage and access to
specific hardware peripherals of the underlying host system, through a single
configuration file.

### 4.4 INTERNAL AND EXTERNAL COMMUNICATIONS
Microservices, contained in Docker containers and in execution, need to
communicate with each other in order to perform the tasks assigned to the Edge
Gateway. To avoid that internal communication can be a constraint for the
flexibility and reliability of the system, with high latencies, load, timeout
and error propagation in the presence of problems in only one component, we
have adopted the asynchronous communication paradigm of Publish/Subscribe with
boker. In Publish/Subscribe the sender of a message, the *publisher*, sends its
message to a central intermediary called *broker* without the need to specify
the recipient but only the *topic*, a sort of mailbox. The recipients of the
message, or the consumers, called *subscribers*, in turn subscribe at the
broker to a specific topic, a kind of postal subscription, to receive messages
sent to the relevant topic. Like the publisher, the subscriber also does not
know the sender of the message. Multiple publishers can publish on the same
topic, and multiple subscribers can subscribe to the same topic. Or
alternatively, a subscriber can receive messages from multiple publishers and
the same message can be delivered to multiple subscribers.

There are different protocols and frameworks for implementing the
Publish/Subscribe paradigm with brokers. For the Edge Gateway the standard MQTT
protocol has been adopted: besides being one of the most widely used protocols
in the Internet of Things, it is extremely light, flexible and has
implementations in different programming languages and for different processor
architectures. In particular, its flexibility lies in the fact, among other
things, to be agnostic regarding the content of the message, dealing only with
its transmission and reception.

Internally, therefore, the microservices that produce data, such as the
handlers of sensors and stations, publish their messages on separate topics
according to the domain to which they belong, weather, environment, energy. The
microservices that consume these messages, such as the dispatcher, instead
subscribe to those topics waiting for messages. Asynchronous communication
therefore prevents microservices from constantly keeping an open channel
between them while waiting for data or confirmation of receipt. Moreover, the
addition of a new publisher or subscriber is completely transparent to the
system, as it is not necessary to create new routes or assign addresses or
ports.

The same applies to external transmission. The adoption of the MQTT protocol
for communications from the Edge Gateway to the central collection system, in
addition to the aforementioned advantages, offers the possibility to
authenticate and encrypt messages through SSL/TLS, a better efficiency of the
network entering the cloud and the scalability of the service.

### 4.5 EDGE GATEWAY MICROSERVICES
The Edge Gateway consists of three categories of microservices:

* the '*sensor handlers*' and '*station handlers*' or simply the '*handlers*';
* the '*dispatcher*';
* the '*ancillary services*'.

#### 4.5.1 HANDLERS 
The handlers are microservices in charge of managing sensors that are
physically connected to the Edge Gateway and single remote sensors ('*sensor
handlers*'), or the communications with remote measuring stations consisting of
several different sensors ('*station handlers*'). In particular, their tasks
are, in the case of a sensor handler:

* configure the communication channel or bus and initialize the sensor;
* poll periodically the sensor to get the measurements of the quantities of
  interest;
* send packages of acquired data to the local broker.

For the station handlers (weather, energy, etc. ...), the tasks are:

* establish the connection with the remote station or open a listening network
  socket;
* receive the data sent by the stations;
* save the data in the local database;
* convert and forward the acquired data package to the local broker.

The handlers are written in Python version 3 and have a similar code structure:

* can be configured either via configuration files or via command line options;
* use the same type of configuration file with common fields and specific
  fields;
* all handlers can refer to a single file divided into sections, each with the
  name of the handler it configures.

#### Meteo Handler

The handler of the remote weather station is named Feinstaub Publisher after
the name of the weather and air quality station adopted. Since this station
sends its data via WiFi in a format and protocol suitable for ingestion in an
InfluxDB database, the handler simulates a listening InfluxDB server and
intercepts the incoming data. These data are then entered into the local
InfluxDB database for subsequent consultation by the other microservices and at
the same time converted into a standard message that is sent to the local MQTT
broker for event management or forwarding to the outside.

#### Energy Handler

The handler of the remote energy measurement station is named Iotawatt
Publisher after the name of the station adopted. Since also this station too
sends its data via WiFi for ingestion in a InfluxDB database, the handler is
very similar to the meteo handler which borrows most of the code. It simulates
a listening InfluxDB server, receives its data, inserts them in the local
InfluxDB database and converts them into a standard message that is sent to the
local MQTT broker.

#### Internal Sensors Handler

The Edge Gateway can have sensors directly attached to its connectors.
Depending on the type of interface or bus to which they are connected, a
different micro-service takes care of establishing the connection, configuring
the sensors on that interface and polling them periodically. For the sensor
used in this reference design, the HTU21D temperature and humidity sensor, the
handler is called HTU21D Publisher. This handler opens the interface on the I2C
bus on which the sensor operates and, at time interval set in the configuration
file or command line, pools the sensor and sends to the local MQTT broker the
message with the parameters detected, in this case temperature, humidity and
the dew point calculated from the first two.

Data flow inside a handler |
---------------------------|
![Data flow inside a handler ](../../img/Handler-Internals.png) |

#### 4.5.2 THE DISPATCHER

The *dispatcher* is a micro service listening on the local broker, or MQTT
*subscriber*, that receives the messages sent by the handlers. The dispatcher
has the main task of forwarding the acquired data to the remote system of
collection and processing. It responds to two main needs:

* the sensors and the stations can locally generate such a quantity of data and
  at a such a rate that they cannot be sent directly and in real time to the
remote cloud, under penalty of congestion and overload of both the external
network and the remote systems; it is therefore necessary to regulate the
transmission according to the capacity of the network and the quantity of data
to be sent;
* to guarantee the security and confidentiality of the transmission to the
  remote system of data collection through the Internet, it is necessary to
authenticate and encrypt the communication by means of access credentials such
as username and password and digital certificates; for greater security, it is
preferable that only a micro-service can have access and can use these
credentials to present itself to the remote broker.

The dispatcher then performs the task of:

* decouple the production of the data from its forward in order to be able to
  temporarily store, compress and send it at intervals configurable according
to the capacity of the network;
* present to the remote cloud the credentials for access and sending;
* locally manage any communication interruptions or communication
  deterioration.

#### 4.5.3 AUXILIARY SERVICES
Auxiliary services are microservices that implement shared functionalities,
i.e. not specific to other microservices but used by them. These auxiliary
services are:

* the local MQTT broker;
* the local database;
* the local dashboard.

#### The local MQTT broker
The local MQTT broker implements the asynchronous communication bus for the
various microservices. To the local broker the *data producers*, the handlers,
publish their data on special topics, and to it the *data consumers*, the
dispatcher and other possible microservices for event management, subscribe for
the notification and forwarding of the published data. The software used to
provide the MQTT broker service is called '*mosquitto*'. It was chosen because
it is among the lightest in terms of resources required, implements all the
specifications of the MQTT protocol although its internal use does not require
special needs, and has proven to be the most suitable to be used in an
environment with limited resources.

#### The Local Database
The local database has the task of storing data from sensors and remote
measurement stations for subsequent display by the user, by the local graphical
interface, or use by additional preprocessing microservices. The database used
is ***InfluxDB*** and was chosen because is one of the target database normally
used by the weather and energy remote measurement stations.

#### The Local Dashboard
The local dashboard is the software that allows the user to view the data
collected and stored by his Edge Gateway. The software used in this case is
***Grafana***, an extremely popular web visualization tool used for the display
and creation of graphs and dashboards based on historical data sets. Grafana
allows the user, through the access from a web browser, not only to view their
data, but also to create their own graphs and indicators based on their
processing.

Grafana Dashboard running on the Edge Gateway |
----------------------------------------------|
![Grafana Dashboard running on the Edge Gateway](../../img/Grafana-Dashboard.png) |

#### 4.5.4 DATA MODELS

The design of the Edge Gateway involves the use of sensors and measuring
stations from different manufacturers and models. These may differ, from the
point of view of data transmission, both in the protocol used and in the format
of the data itself. In order for data of the same type but produced by
different objects and in different formats to be collected and used by the TDM
project, conversion to a common, predefined format is necessary. As in the TDM
project it was decided to adhere to the guidelines of the ***OASC initiative***
and its technological platform of reference, the ***FIWARE*** ecosystem, where
possible and where already defined, it was decided to use the FIWARE
*Harmonized Data Models*. For the sensor domains for which there is no FIWARE
Harmonized Data Model or it has not been possible to identify a suitable one
among those present, it has been decided to define new ones on the model of the
existing ones.

#### The 'Weather' message type - WeatherObserved

For the weather stations we chose to use the harmonized data model
'***WeatherObserved***'
(<https://fiwaredatamodels.readthedocs.io/en/latest/Weather/WeatherObserved/doc/spec/index.html>).
However, since some of the sensors in the weather station initially used also
concern air quality measurements, and the relative model did not appear to be
sufficient, the 'WeatherObserved' was extended by adding the necessary fields.

The 'WeatherObserved' data model is a '*JSON*' type message with mandatory and
optional fields. The message containing the required fields but in the absence
of some optional fields is still considered valid. This is the case of weather
stations that do not have all the required sensors. However, not all the fields
provided by the model are present in the message. Some of these are in fact
populated and used by the components of the FIWARE platform in the TDM cloud
during processing.

The required fields are:

* **id**: station unique identifier;
* **type**: type of the message, must be '**WeatherObserved**';
* **location**: location of the weather station (GeoJSON) required if address
  is not present;
* **address**: street address of the station;
* **dateObserved**: date and time of observation (ISO8601 UTC).

The optional fields used are:

* **temperatures**: air temperature (numeric, degrees Celsius);
* **relativeHumidity**: relative air humidity (numeric, percentage);
* **precipitation**: amount of rain (numerical, mm);
* **windDirection**: wind direction (numeric, decimal degrees North);
* **windSpeed**: wind speed (numeric m/s);
* **atmosphericPressure**: atmospheric pressure (numeric, hPa);
* **illuminance**: light (numeric, lux or lumen/sqm).

Optional fields added:

* **timestamp**: timestamp of message creation (numeric, UNIX Epoch);
* **infraredLight**: infrared light (numeric, sensor-related measurement);
* **CO**: amount of CO (numeric, sensor-related measurement);
* **NO**: quantity of NO (numeric, sensor-related measurement);
* **NO2**: Amount of NO2 (numeric, sensor-related measurement);
* **NOx**: Amount of NOx (numerical, sensor-related measurement);
* **SO2**: Quantity of SO2 (numerical, sensor-related measurement);
* **PM10**: quantity of dust PM10 (numerical, sensor-related measurement);
* **PM2.5**: amount of dust PM 2.5 (numerical, sensor-related measurement).

Since the **dateObserved** field can be modified by the FIWARE platform
components, this is not sent by the EDGE but the timestamp field has been added
to communicate the **timestamp** of the detection by the sensors.

Example of the ‘WeatherObserved’ FIWARE message used |
------------------------------------------------------|
```json
{
    "id": "Cagliari-Pirri-ExDistilleria",
    "type": "WeatherObserved",
    "dateObserved": "2016-11-30T07:00:00.00Z",
    "location":
     {
        "type": "Point",
        "coordinates":
        [ -4.754444444, 41.640833333 ]
    },
    "temperature": 3.3,
    "relativeHumidity": 1,
    "atmosphericPressure": 938.9,
    "windDirection": -45,
    "windSpeed": 2,
    "precipitation": 0,
    "illuminance": 1000,
    "infraredLight": 850,
    "CO": 500,
    "NO": 45,
    "NO2": 69,
    "NOx": 139,
    "SO2": 11,
    "PM10": 11,
    "PM2.5": 11,
}
```

#### The 'Energy' message type - EnergyMonitor

For the energy monitoring stations a suitable data model has not been
identified and an ad-hoc one has been created that can be extended and
submitted to the FIWARE community for possible discussion and adoption. As for
the message 'WeatherObserved', also the message 'EnergyMonitor' is in 'JSON'
format with mandatory fields and optional fields, and a message 'EnergyMonitor'
containing mandatory fields but in default of some optional fields is still
considered valid.

The required fields are:

* **id**: the station unique identifier;
* **type**: type of message, must be '**EnergyMonitor**';
* **location**: location of the weather station (GeoJSON) required if address
  is not present;
* **address**: street address of the station;
* **dateObserved**: date and time of observation (ISO8601 UTC).

The optional fields used are:

* **timestamp**: timestamp of message creation (numeric, UNIX Epoch);
* **voltage**: voltage of the power line (numeric, Volt);
* **current**: current flowing through the power line (numeric, Ampere);
* **apparentPower**: apparent power absorbed by the load (numeric,
  Volt-Ampere);
* **realPower**: real power absorbed by the load (numeric, Watt);
* **powerFactor**: power factor (numeric, between 0 and 1);
* **consumedEnergy**: energy consumed (numeric, Wh);
* **frequency**: frequency of the electricity line (numeric, Hz).

Since the **dateObserved** field can be modified by the FIWARE platform
components, this is not sent by the EDGE but the **timestamp** field has been
added to communicate the timestamp of the detection by the sensors.

Example of the NGSIv1 message 'EnergyMonitor' used |
----------------------------------------------------|
```json
{
    "id": "Cagliari-Pirri-ExDistilleria",
    "type": "EnergyMonitor",
    "dateObserved": "2016-11-30T07:00:00.00Z",
    "location":
    {
        "type": "Point",
        "coordinates":
        [-4.754444444, 41.640833333]
    },
    "voltage": 235.90,
    "current": 3.74,
    "apparentPower": 882.9,
    "realPower": 250.8,
    "powerFactor": 0.66
}
```

### 4.6 TOPICS

In order to be able to forward messages correctly and according to the
transmission policy, they must be published internally in specific MQTT topics.
These generally have the form:

***MessageType/StationId.SensorModel***

#### 4.6.1 TOPICS FOR 'WEATHER'

Topics for weather messages begin with 'WeatherObserved'. For example, for a
*feinstaub* type station, the temperature and humidity messages measured by the
DHT22 sensor will be published on the topic:

***WeatherObserved/esp8266-XXXX.DHT22***

#### 4.6.2 TOPICS FOR 'ENERGY'

Topics for energy-related messages start with 'EnergyMonitor'. For example, for
a *IotaWatt* station, messages about current, voltage, and other parameters
measured on the line of an air conditioner, HVAC, will be published on the
topic:

***EnergyMonitor/IOTAWATT.HVAC***
