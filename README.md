# Multilanguage README

[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/Fellipem1/PLCnext_Node-RED/tree/main#how-to-install-node-red-in-plcnext-with-podman)
[![es](https://img.shields.io/badge/lang-es-yellow.svg)](https://github.com/Fellipem1/PLCnext_Node-RED#como-instalar-node-red-en-plcnext-utilizando-podman)

# English:

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


# Español:

# Como instalar Node-RED en PLCnext utilizando Podman
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](MIT_LICENSE)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

Desde la versión de firmware 2023.0, Podman (https://podman.io/) viene instalado en el sistema operativo PLCnext. Podman cumple la misma función que Docker. Esta implementación garantiza una instalación de Node-RED más sencilla para el usuario final.

> [!IMPORTANT]
> Debido a los cambios que sufre PLCnext con cada actualización de firmware, estos procedimientos pueden dejar de funcionar en el futuro. Si este es el caso, este documento se actualizará.

|Descripción         | Fecha      |
|--------------------|------------|
|Creación            | 05-03-2024 |
|Ultima Modificación | 11-03-2024 |

## Requisitos

|Descripción          | Valor      |
|---------------------|------------|
|Controlador          | AXC F 1152 - AXC F 2152 - AXC F 3152 |
|Almacenamiento       | Memoria SD de 2GB o superior para AXC F 1152 / 2152 |
|Firmware             | 2023.0 LTS o superior |

## 1. Pasos de instalación:

### 1.1. Acceso a través de un cliente SSH:

- Debe acceder usando ```Putty``` o su cliente ```SSH client``` favorito utilizando la IP de su PLC. Recuerde que la IP predeterminada del PLC es ```192.168.1.10```.
  
- Inicie sesión con el nombre de usuario ```admin``` y la contraseña de su PLCnext.

### 1.2. Acceso como usuario root:

- Una vez que haya iniciado sesión en PLCnext, necesita acceder como usuario root usando el comando ```su```

```
su
```
- Si aún no ha creado una contraseña root, créela con el comando ```sudo passwd root```
  
```
sudo passwd root
```
  
> [!NOTE]
> Los usuarios ```admin``` y ```root``` son diferentes. Establecer una contraseña diferente para el usuario root no afectará al usuario ```admin```.

### 1.3. Instalación de Node-RED:

- Una vez que haya iniciado sesión como usuario root, puede instalar la imagen oficial de Node-RED en su versión 2.2.2 usando el siguiente comando:

```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/nodered/node-red:2.2.2
```

### 1.4. Instalación de ejemplos:

- He creado contenedores de Node-RED para los PLCnext AXC F 1152 / 2152 que incluyen bibliotecas para trabajar con PLCnext. Estos contenedores también incluyen ejemplos de envío de alarmas, escritura y lectura usando OPC UA. Para instalar cualquiera de estos ejemplos, use los siguientes comandos:

- Para instalar el ejemplo "Envío de alarma":
```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/fellipem/noderedpxc:alarms
```

- Para instalar el ejemplo "Lectura y escritura OPC UA":
```
podman run -d -p 1880:1880 --name nodered --restart always docker.io/fellipem/noderedpxc:opcua
```

### 1.5. Acceso a Node-RED:

- Una vez que haya terminado, puede comenzar a usar Node-RED ingresando ```IP_PLC:1880``` en su navegador web. Por ejemplo, si la dirección IP de su PLC es 192.168.1.10, ingresaría ```192.168.1.10:1880```.

> [!WARNING]
> A diferencia de Docker, Podman no se ejecuta como un servicio dentro de PLCnext. Por lo tanto, Node-RED no se iniciará automáticamente cuando se encienda el PLC. Para que Node-RED se inicie con el PLC, siga estos pasos:

## 2. Pasos para iniciar Node-RED automáticamente:

### 2.1. Crear un script de inicio:

- Use el siguiente comando en su PLC para descargar este proyecto:
```
git clone https://github.com/Fellipem1/PLCnext_Node-RED.git
```
### 2.2. Mover el archivo:

- Mueva el archivo ```nodered.sh``` a la ruta ```/etc/init.d/``` usando el siguiente comando:
```
mv /opt/plcnext/PLCnext_Node-RED/nodered.sh /etc/init.d/
```

### 2.3. Cambiar permisos del archivo:

- Ejecute el siguiente comando para cambiar los permisos del archivo ```nodered.sh```:
```
chmod +x /etc/init.d/nodered.sh
```

### 2.4. Agregar el script al inicio:

- El método para agregar el script al inicio varía según la distribución de Linux que esté usando. En el caso de PLCnext, debe usar el siguiente comando:
```
update-rc.d nodered.sh defaults
```

- Siguiendo los pasos anteriores, Node-RED estará listo para iniciarse automáticamente cada vez que apague o reinicie el PLCnext.

> [!NOTE]
> Recuerde realizar siempre estos pasos como usuario root.

## 3. Pasos para instalar librerías en Node-RED:

### 3.1. Iniciar Node-RED y acceder al contenedor:

- Acceda como usuario root y escriba los siguientes comandos:
  
```
podman start nodered
```
```
podman exec -it nodered /bin/bash
```
```
cd /data
```
### 3.2. Instalar la librería:

- Ahora puede instalar cualquier librería usando comandos ```npm```, por ejemplo:
```
npm i node-red-contrib-iiot-opcua@3.11.1-4
```
### 3.3. Salir del contenedor y reiniciar Node-RED:

- Una vez finalizado, escriba ```Exit``` y reinicie Node-RED con los siguientes comandos:
```
podman stop nodered
```
```
podman start nodered
```

## 4. Errores comunes

### 4.1. Error 1: mkdir no such file or directory

- El siguiente error ocurre porque el usuario ```admin``` no tiene suficientes privilegios para instalar Node-RED. Para solucionar el problema, debe iniciar sesión como usuario ```root```.

### 4.2. Error 2: Certificate Expired

- El siguiente error ocurre porque el PLCnext tiene configurada la hora y la fecha incorrectas. Puede verificar la hora y la fecha con el comando ```date```.
- Para cambiar la hora en el PLCnext, use el siguiente comando:  
```
date -s “29 FEB 2024 11:25:39”
```
- Recuerde modificar el comando con la fecha y hora UTC actuales: https://time.is/es/UTC

### 4.3. Error 3: Failed, retrying timeout

- Este error ocurre porque el PLCnext no tiene acceso a internet. Puede verificar esto con el comando ```ping```, por ejemplo:
```
ping google.cl
```
- Si dice ```bad address```, debe verificar la configuración de IP, máscara y Gateway del PLCnext. Puede hacerlo desde el servidor web, sección Network.
```
ping google.cl
```
- If it says ```bad address```, you must verify the IP, mask, and Gateway configuration of the PLCnext. You can do this from the web server, Network section.
  
