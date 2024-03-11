# Multilanguage README

[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/Fellipem1/PLCnext_Node-RED/tree/main#how-to-install-node-red-in-plcnext-with-podman)
[![es](https://img.shields.io/badge/lang-es-yellow.svg)](https://github.com/jonatasemidio/multilanguage-readme-pattern/blob/master/README.es.md)

# How to install Node-RED in PLCnext with Podman
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](MIT_LICENSE)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

Since firmware 2023.0, Podman (https://podman.io/) has been installed on the PLCnext operating system. Podman fulfills the same function as Docker. This implementation guarantees a simpler Node-RED installation for the end user.

> [!IMPORTANT]
> Due to the changes that PLCnext undergoes with each firmware update, these procedures may stop working in the future. If this is the case, this document will be updated.

|Description   | Date       |
|--------------|------------|
|Created       | 05-03-2024 |
|Last modified | 11-03-2024 |

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
> The ```admin``` and ```root``` users are different. Setting a different password for the root user will not affect the ```admin``` user.

### 1.3. Installing Node-RED:

- Once you have logged in as the root user, you can install the official Node-RED image in its version 2.2.2 using the following command:

```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/nodered/node-red:2.2.2
```

### 1.4. Installing Examples:

- I have created Node-RED containers for the PLCnext AXC F 1152 / 2152 that include libraries to work with PLCnext. These containers also include examples of sending alarms, writing and reading using OPC UA. To install any of these examples, use the following commands:

- To install the "Alarm Sending" example:
```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/fellipem/noderedpxc:alarms
```

- To install the "Reading and writing OPC UA" example:
```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/fellipem/noderedpxc:opcua
```

### 1.5. Accessing Node-RED:

- Once you have finished, you can start using Node-RED by entering ```IP_PLC:1880``` in your web browser. For example, if your PLC's IP address is 192.168.1.10, you would enter ```192.168.1.10:1880```.

> [!WARNING]
> Unlike Docker, Podman does not run as a service within PLCnext. Therefore, Node-RED will not start automatically when the PLC is turned on. To make Node-RED start with the PLC, follow these steps:

## 2. Steps to start Node-RED automatically:

### 2.1. Create a startup script:

- Use the following command on your PLC to download this project:
```
git clone https://github.com/Fellipem1/PLCnext_Node-RED.git
```
### 2.2. Move the file:

- Move the ```nodered.sh``` file to the ```/etc/init.d/``` path using the following command:
```
mv /opt/plcnext/PLCnext_Node-RED/nodered.sh /etc/init.d/
```

### 2.3. Change file permissions:

- Run the following command to change the permissions of the ```nodered.sh``` file:
```
chmod +x /etc/init.d/nodered.sh
```

### 2.4. Add the script to startup:

- The method for adding the script to startup varies depending on the Linux distribution you are using. In the case of PLCnext, you should use the following command:
```
update-rc.d nodered.sh defaults
```

- By following the steps above, Node-RED will be ready to start automatically every time you turn off or restart the PLCnext.

> [!NOTE]
> Remember to always perform these steps using the root user.

## 3. Steps to install libraries in Node-RED:

### 3.1. Start Node-RED and access the container:

- Access as root user and type the following commands:
  
```
podman start nodered
```
```
podman exec -it nodered /bin/bash
```
```
cd /data
```
### 3.2. Install the library:

- Now you can install any library using ```npm``` commands, for example:
```
npm i node-red-contrib-iiot-opcua@3.11.1-4
```
### 3.3. Exit the container and restart Node-RED:

- Once finished, type ```Exit``` and restart node-RED with the following commands:
```
podman stop nodered
```
```
podman start nodered
```

## 4. Common Errors

### 4.1. Error 1: mkdir no such file or directory

- The following error occurs because the ```admin``` user does not have enough privileges to install Node-RED. To solve the problem, you must log in as the ```root``` user.

### 4.2. Error 2: Certificate Expired

- The following error occurs because the PLCnext has the wrong time and date configured. You can check the time and date with the ```date``` command.
- To change the time on the PLCnext, use the following command:  
```
date -s “29 FEB 2024 11:25:39”
```
- Remember to modify the command with the current UTC time and date: https://time.is/es/UTC

### 4.3. Error 3: Failed, retrying timeout

- This error occurs because the PLCnext does not have internet access. You can check this with the ```ping``` command, for example:
```
ping google.cl
```
- If it says ```bad address```, you must verify the IP, mask, and Gateway configuration of the PLCnext. You can do this from the web server, Network section.
  
