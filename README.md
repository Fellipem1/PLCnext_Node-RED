# How to install Node-RED in PLCnext with Podman
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](MIT_LICENSE)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

Since firmware 2023.0, Podman (https://podman.io/) has been installed on the PLCnext operating system. Podman fulfills the same function as Docker. This implementation guarantees a simpler Node-RED installation for the end user.

> [!IMPORTANT]
> Due to the changes that PLCnext undergoes with each firmware update, these procedures may stop working in the future. If this is the case, this document will be updated.

|Description   | Date       |
|--------------|------------|
|Created       | 05-03-2024 |
|Last modified | 08-03-2024 |

## Requirements

|Description   | Value      |
|--------------|------------|
|Controller    | AXC F 1152 - AXC F 2152 - AXC F 3152 |
|Storage       | SD memory 2GB or higher for AXC F 1152 / 2152 |
|Firmware      | 2023.0 LTS or higher |

## 1. Installation steps:

### 1.1. Accessing via SSH client:

- You must access using ```Putty``` or your favorite ```SSH client``` using the IP of your PLC. Remember that the default IP is ```192.168.1.10```.
  
- Log in with the username ```admin``` and the password of your PLCnext.

### 1.2. Accessing as root user:

- Once you are logged in to the PLCnext, you need to access as a root user using the command ```su```

```
su
```
- If you haven't created a root password yet, create it with the command ```sudo passwd root```
  
```
sudo passwd root
```
  
> [!NOTE]
> The ```admin``` and ```root``` users are different. Setting a different password for the root user will not affect the "admin" user.

### 1.3. Installing Node-RED:

- Once you have logged in as the root user, you can install the official Node-RED image in its version 2.2.2 using the following command:

```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/nodered/node-red:2.2.2
```

> [!TIP]
> I have created Node-RED containers for the PLCnext AXC F 1152 / 2152 that include libraries to work with PLCnext. These containers also include examples of sending alarms, writing and reading using OPC UA. To install any of these examples, use the following commands:

- To install the "Alarm Sending" example:
```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/fellipem/noderedpxc:alarms
```

- To install the "Reading and writing OPC UA" example:
```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/fellipem/noderedpxc:opcua
```

### 1.4. Accessing Node-RED:

Once you have finished, you can start using Node-RED by entering ```IP_PLC:1880``` in your web browser. For example, if your PLC's IP address is 192.168.1.10, you would enter ```192.168.1.10:1880```.

> [!WARNING]
> Unlike Docker, Podman does not run as a service within PLCnext. Therefore, Node-RED will not start automatically when the PLC is turned on. To make Node-RED start with the PLC, follow these steps:
