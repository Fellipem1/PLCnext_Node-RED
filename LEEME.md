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