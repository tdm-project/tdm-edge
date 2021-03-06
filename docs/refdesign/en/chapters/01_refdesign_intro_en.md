## INTRODUCTION

One of the main objectives of the TDM project is to create a scalable
architecture for the acquisition, integration and analysis of data coming from
heterogeneous sources, that is capable of managing information generated by a
broad metropolitan area. Some of this data will come from sensors spread over
the territory. In order to better manage this devices, the project provides a
*reference design* for a general-purpose platform for sensor-device management
and  the transmission of the collected measures. This platform is the ***Edge
Gateway***.

This document describes the hardware/software architecture of the Edge Gateway
and its components devoted to the acquisition and pre-processing of the
measurements acquired from the distributed sensors of the project, and their
forwarding to the central system for collection, storage and analysis.

The Edge Gateway described here is for general use, and has been specifically
designed to be used with other infrastructure based on the *FIWARE* ecosystem
other than the TDM one. It can be used to build *OASC-compliant* solutions on
cloud platforms such as, for example, *IoT Amazon AWS*, *IBM Watson* and
*Microsoft Azure*.

In particular, the following reference design is based on hardware platforms
easily available on the market and of which all the documentation needed for
operation and integration into other products (the data sheet) is freely
available (the BeagleBone Black board, described later, for example, is
distributed as Open Hardware, i.e. its entire design can be used in the
architecture of other devices). Measurement stations used in conjunction with
the Edge Gateway are also distributed, as well as finished product, as Open
Hardware while the source code of the related firmware is freely available and
distributed under Open Source license. The software architecture designed for
the Edge Gateway is in turn based on Open Source, standard protocols and
example code can be freely used and modified. Communications take place through
the MQTT standard protocol, while the message formats are compatible with
FIWARE and OASC standards and best practices.

All the software developed is available on ***GitHub*** at:
<https://github.com/tdm-project>.

Below is a brief overview of the overall architecture of the project, the
architecture developed for the software, the requirements for the hardware
platform and the components chosen for the prototype.

