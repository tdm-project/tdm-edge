## 3. GENERAL CHARACTERISTICS OF THE EDGE GATEWAY HARDWARE PLATFORM
### 3.1 GENERAL CONSIDERATIONS

The hardware platform for the Edge Gateway, the Edge Device, must be able to to
be able to properly execute the aggregation, processing and transmission tasks
as described above, and allow the possible integration of further future
components.

In addition to these technical requirements, the choice of the hardware
platform must also take into account other requirements related to the
addressed user. The target users for these sensors, in addition to the
institutional one, is, in fact, increasingly made up of high school students
and private citizens who voluntarily intend to buy or assemble the devices
themselves.  Specific experiments in this sense will be made in the TDM
project, but this mode of use is increasingly widespread worldwide.

It is necessary, therefore, that the hardware platform has a limited cost and
is easily available, possibly with a large community of developers and users
possible. It must also be free from commercial constraints on royalties and
licenses, and possibly have a *'educational'* connotation.

In particular, two of these general requirements, the low cost and the wide
user communities narrow down the scope of choices to:

* hobbies Single Board Computer (SBC) for the platform, and
* Linux distribution as the operating system.

### 3.2 I/O AND CONNECTIVITY REQUIREMENTS
The main role of the Edge Gateway is to collect and aggregate measurements and
data acquired locally by the various sensors and measuring stations in the
field and prepare them for sending to the cloud for subsequent storage and
processing.

From the point of view of I/O capabilities, the hardware platform must
therefore be able to communicate with the sensors using both simple electric
buses and more sophisticated network protocols. In particular, for sensors
directly connected to the Edge, it is possible to use mainly I2C, USART and
custom protocols implemented on GPIO, while for remote detection units such as
energy consumption meters and weather stations / air quality, the easiest and
most effective way is the WiFi connectivity through home network, if present in
the place of installation, or possibly creating it through the Edge Gateway
itself. The possibility of future expansions should also be taken into
consideration: the desired and non-binding requirement for the hardware
platform is therefore the presence of additional electrical buses and
communication protocols between embedded devices.

As regards communications from the Edge Gateway to a Cloud, such as that of the
TDM project, it was decided to use Internet connectivity in the installations
it has reached, while for installations in rural areas or not yet served, it is
possible to provide the use of LTE connectivity through specific chips or
modems/USB dongles.

To sum up, the hardware platform for the Edge Gateway must have the following
peripherals:

* **I2C** bus/port;
* **UART/USART** bus/port;
* free **GPIO** pins.

The presence of the following protocols, although not binding, is a desired one:

* **CAN** bus/port;
* **ADC** ports/peripherals;
* ports with **PWM** output;
* **Video** outputs and/or **LCD** controller;

***Must*** have network devices:

* ***WiFi***;
* ***Ethernet***;
* ***USB*** Host.

### 3.3.COMPUTATIONAL REQUIREMENTS

As for the computational requirements, i.e. those requirements that allow the
software architecture of the Edge Gateway to be executed effectively, these are
of three types: the processor and the computing power, the amount of RAM
memory, and the storage capacity.

The requirements for the Edge Gateway processor are as follows: 

* must be supported by the Linux kernel and be able to run a full Linux
  operating system; 
* have low power consumption to allow use via low voltage battery;
* have good software support and possibly a large community of users and
  developers;
* have free and royalty free SDKs and compilers available;
* have low cost and full development boards accessible;
* given the architectural choice to use multiple microservices running
  concurrently, the processor must have more than one core.

Many processors for embedded applications are capable of running a complete
Linux operating system. Among these, the ARM-CORTEX family offers several
choices, both as processors and development boards, that meet these
requirements.

As far as RAM memory is concerned, the above points are worth noting. The
provision of RAM for the Edge Gateway must:

* be sufficient to run a complete Linux operating system;
* have a low power consumption;
* be large enough to serve multiple competing microservices running on Docker
  containers;
* support access from multiple cores.

Storage on the Edge Gateway means the storage medium both for the operating
system and the Edge applications, and for the storage of data collected over
time by the various sensors and measuring stations connected. More in detail,
the requirements for storage are:

* economic, accessible and flexible, i.e. solutions easily available on the
  market, in different capacities and easily replaceable and upgradable;
* sufficient capacity to hold the operating system, the Edge applications and a
  reasonably large history of data collected;
* wide speed of data transfer.

The most common solution that addresses to these requirements is the SD
technology, and, therefore, it is considered that the hardware solution for the
Edge Gateway will need to have support for SD/microSD cards. Empirically it has
been established that a size of 8GB, class 10, is sufficient for the purposes
of the Edge Gateway, however, given the availability of higher capacity SD with
reduced costs, we recommend the minimum size of 16GB, class 10.

### 3.4 DISCUSSION

The above requirements can be met by many hardware solutions, with various
cost-performance ratios. In the following section, we describe the software
architecture implemented, which relies on the above requirements and has no
dependencies on the specific solution chosen. In section 5 we will discuss,
instead, the hardware implementation of the reference prototype.
