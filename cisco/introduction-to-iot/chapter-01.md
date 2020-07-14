### Spis treści
- [Chapter 1: Everything is connected](#chapter-1-everything-is-connected)
  - [1.1 Digital transformation](#11-digital-transformation)
  - [1.2 Devices that connect to the IoT](#12-devices-that-connect-to-the-iot)

# Chapter 1: Everything is connected

## 1.1 Digital transformation

`Sensor` - device that detects or measures an event

Netowrk types:
- PAN - Personal Area Network, small networks where connected wireless devices are within personal reach eg. connecting smartphone to car using Bluetooth
- LAN - Local Area Network
- WAN - Wide Area Network
- Internet
- Wireless networks
- The cloud - collection of data centers or groups of connected servers that are used to store analyze data, provide access to online applications and backup services
- The egde - edge refers to the physical “edge” of a corporate network.
- Fog computing - used to store the sensor data securely and closer so analyzed data can be used quickly and effectively to update or modify processes. Usually the fog is located at the edge of a business or corporate network

    >Servers and computer programs allow the data to be `pre-processed` for immediate use. Then the pre-processed data can be sent to the cloud for more in-depth computing if required.

## 1.2 Devices that connect to the IoT

`IoT` - the connection of milions of smart devices and sensors connected to the Internet. These devices and sensors collect and share data for use and evaluation by many organizations. The IoT has been possible, in part, due to the advent of cheap processors and wireless networks.

>Some examples of intelligent connected sensors are: smart doorbells, garage doors, thermostats, sports wearables, pacemakers, traffic lights, parking spots, and many others.

Benefits from the data collected, saved, analyzed from sensors:
- businesses have more information about product that they sell and who is purchasing them
- retailers are able to do more target marketing, reduce losses based on unsold products
- manufacturing saves money, improves efficiency, and improves productivity of manufacturing processes and operations
- governmnets monitor monitor environmnetal, social issues
- cities have tha ability to control traffic based on time of day or major events, control garbage and recycling, monitor health
- individuals can reap improved fitness and health benefits, reduce costs of energy and heating systems

Disadvantages of IoT:
- companies that create wearable devices know a lot of very personal information about users
- retailers know everything that you are buying
- individuals are receiving more "spam" emails
- network failure can be catastrophic
- reliance on online shopping may cost a jobs

Sample IoT topology:
- `sensors` - collects data from environment, then the gathered data is send by network connection to a controller
- `controllers` - collects data from sensors and can make immediate decisions, or they may send data to a more powerful computer analysis
- `actuators` - device which take electrical input and transform it into physical action

>If a sensor detects excess heat in a room, the sensor sends the temperature reading to the microcontroller. The microcontroller can send the data to an actuator which would then turn on the air conditioner.

>Many sensors are “out in the field” and are powered by batteries or solar panels, consideration must be given to power consumption. Low-powered connection options must be used to optimize and extend the availability of the sensor. Most of them require wireless connection.

>The business may define that a contract employee is given access to only a specific set of data and applications. This is the intent. In an intent-based networking system (`IBN`), all the network devices will be automatically configured to fulfil this requirement across the network, no matter where the employee is connected. VLAN, subnet, ACL and all other details will be automatically defined and configured following best practices. The intent has to be defined once in a central management console, and then, the network will continuously assure it, even if there are changes in the network.

---

<div align="right">
<a href="chapter-02.md">Next: Chapter 2</a>
</div>