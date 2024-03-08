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
  
<p align="center">
  <img src="https://i.ibb.co/6s8Jt29/Putty.png">
</p>

- Log in with the username ```admin``` and the password of your PLCnext.
  
<p align="center">
  <img src="https://i.ibb.co/VYDhXqL/Putty1.png">
</p>

### 1.2. Accessing as root user:

- Once you are logged in to the PLCnext, you need to access as a root user using the command ```su```
```
su
```
- If you haven't created a root password yet, create it with the command ```sudo passwd root```
  
<p align="center">
  <img src="https://i.ibb.co/Smz00bh/Putty3.png">
</p>
  
```
sudo passwd root
```
- First, enter the PLC's password, then enter and confirm the password for the root user.
- Remember that the ```admin``` and ```root``` users are different. Setting a different password for the root user will not affect the "admin" user.
