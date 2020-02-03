## 2. INSTALLATION

The installation requirements are:

* A PC with a Linux distribution and a SD/microSD card reader;
* ‘*root*’ privileges;
* SD/microSD adapter;
* WiFi connection;

The current Edge Gateway version is composed by:

* A Raspberry Pi 3;
* power supply 220V/5V able to deliver at least 2.5A;
* class 10 microSD (min 8GB, suggested 16GB);

The described installation procedure allow to:

* Install the customized operating system on the Edge Gateway;
* configure the local WiFi;
* install EDGE microservices;
* configure the TDM software and start services 

Moreover an example of connection between the Edge system and a temperature/humidity sensor ( HTD21D) is shown.

### 2.1 INSTALLATION OF THE OPERATING SYSTEM TO THE EDGE GATEWAY

***Note: The power supply of the Raspberry Pi 3 must be able to deliver al least 2.5A to avoid malfunctioning and filesystem corruption.***

***Note: The procedure here described is partially based on the installation of Archh Linux to the ARMv7 architecture (available at: <https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3>).***

To complete successfully the installation, the commands have to be typed on a Linux console as ‘***root***’ user or with administrator privileges. A ‘***sudo***’ shell can be used as well.
To start a ‘***sudo***’ shell type:

```bash
sudo -s
```

and insert the user password.



### **WARNING: the following instructions will erase all filesystems on microSD. Every file will be deleted. Pay close attention to the names of the devices to which the commands are directed.**

Replace ***sdX*** with the name of the SD device as shown on the display.
The command *fdisk -l* can be used before and after the insertion of the SD card to find out the right device ID.

1. Insert the microSD to the SD card reader (use the adapter if needed);
2. before partitioning the microSD check if the operating system mounted some existing partition on the SD. To do it, take a look at the *mount* command output. If one or more partition with  **sdX** suffix is present (for instance ***sdX1***) unmount by typing:


  ```bash
umount /dev/sdX{partition_number}
  ``` 
  
3. Create partitions on the microSD by using the ***fdisk*** command:

  ```bash
fdisk /dev/sdX
  ```
  
4. At fdisk prompt, remove old partitions and create a new one:

  **a.** Type **o + ENTER**. All the partitions will be erased
  
  **b.** Type **p + ENTER** to list the partition. The output should be empty.
  
  **c.** Type **n + ENTER**, then **p** to select primary partition, **1** to specify the first partition on the device. Accept the default first sector pressing **ENTER** and type **+100M** to specify the last sector. If required, press **y** to remove the *vfat* or *ext4* signature from the first partition.
  
  **d.** Type **t + ENTER**, then **c** to set the first partition type to W95 FAT32 (LBA).
  
  **e.** Type **n + ENTER**, then **p** to select primary partition, **2** to specify the second partition on the device. Press twice **ENTER** to accept the default values of first and last sector. If required, press **y** to remove the *vfat* or *ext4* signature from the first partition.
  
  **f.** Type **w + ENTER** to write out the partition table and quit fdisk.
  
5. Creation and mount of the FAT filesystem:

  ```bash
cd /tmp/
mkfs.vfat /dev/sdX1
mkdir boot
mount /dev/sdX1 boot
  ```

6. Creation and mount of the ext4 filesystem:

  ```bash
mkfs.ext4 /dev/sdX2
mkdir root
mount /dev/sdX2 root
  ```
  
7. Download and extraction of the root filesystem (to preserve file permissions, run the command as *root* user instead of using the *sudo* command):

  ```bash
wget --content-disposition https://space.crs4.it/s/ywvVDh3tuOBWCZ2/download
tar -xpf TDM-Arm-image-latest.tar.gz -C root
sync
  ```

	The ‘*tar*’ command could issue error messages:

  * tar: Ignoring unknown extended header keyword 'SCHILY.fflags'
  * tar: Ignoring unknown extended header keyword 'LIBARCHIVE.xattr.security.capability'

  On some system it is a common behavior and can be ignored.


8. Move startup files to the first partition:

  ```bash
mv root/boot/* boot
  ```
  
9. Unmount both partitions:

  ```bash
umount boot root
  ```
  
If the previous commands were issued from a ‘***sudo***’ shell, exit by pressing **CTRL+D** or typing:

  ```bash
exit
  ```

### 2.2 FIRST ACCESS TO THE EDGE GATEWAY AND WIFI CONFIGURATION
At the first boot the Edge Gateway exposes an access point, with a own private WiFi network, useful for accessing the system. 

1. Insert the microSD card to the Raspberry Pi and connect to the 5V power supply;
2. Wait for the OS startup
3. From the networking interface of the PC look for a WiFi network with a name similar to ‘TDM_XXXXXXXX’, then connect using the password ‘***tdmedgegateway***’;

  ### **WARNING**: leaving the local WiFi of the Edge active **without changing the default password is a risk because expose the system to unauthorized accesses**. It is recommended to change the password at the first access or turn off the local WiFi of the Edge by using the commands specified in the '***Change of the user password and WiFi passphrase***' section.
  
4. Connect to the Edge via SSH using the IP address ‘***192.168.2.1***’. 
  
  ```bash
ssh alarm@192.168.2.1  
  ```
  
  * At the login, use ‘*alarm*’ both for the username and the password.

  At this first login it is mandatory to change the access password with the following procedure:
  
  * type the current password ‘*alarm*’
  * type the new password (it should be composed of at least 8 characters)
  * re-type the new password to confirm it
  
  At this point the SSH connection is terminated and it is possible to re-connect using the new password.
  

#### Configuration of the local WiFi on the Edge Gateway

**WARNING**: The ‘*root*’ user is disabled for security concerns. Administrative tasks are possible only by using the ‘***sudo***’ command.

5. Access to a privileged session typing the command:

  ```bash
sudo -s
  ```
  
  then insert the password.

6. Configure the Edge Gateway to enable the connection to the local WiFi; replace <ESSID> with the SSID name of the local WiFi in the following command:

  ```bash
wpa_passphrase "<ESSID>" > /etc/wpa_supplicant/wpa_supplicant-wlan0.conf
  ```
  
7. The previous command does not provide an output but waits for an input from the user; insert the password of the local WiFi and press **ENTER**;

8. Restart the wireless interface to make changes effective:

  ```bash
  systemctl restart wpa_supplicant@wlan0
  ```

9. If all the previous steps have been completed without errors, the IP address assigned from the local WiFi router can be retrieved by using the following command:

  ```bash
  ifconfig wlan0
  ```
  Note down the IP so that it can be used for later access and for configuring the monitoring stations.
  
10. If the previous commands were issued from a ‘***sudo***’ shell, exit by pressing **CTRL+D** or typing:

  ```bash
  exit
  ```


#### Change of the user password and Wifi passphrase

For later changes of the password of the ‘*alarm*’ user type:

  ```bash
passwd
  ```

then

  * type the current password ‘*alarm*’
  * type the new password (it should be composed of at least 8 characters)
  * re-type the new password to confirm it
  
To change the passphrase of the WiFi network activated by Edge Gateway, type:

  ```bash
sudo hostap_chpsk
  ```

then
  
  * type the new passphrase (it should be composed of at least 8 and max. 63 characters)
  * re-type the new passphrase to confirm it.

It is possible to disable the WiFi network activated by Edge Gateway if not needed by using the following commands:

  ```bash
sudo systemctl disable start_ap
sudo systemctl stop start_ap
  ```

### 2.3 INSTALLATION OF THE TDM SOFTWARE
To execute the operation described in the next sections, access via SSH to the Edge Gateway by using the previously annotated IP address and the ‘*alarm*’ user. Then, access to a privileged session typing:

  ```bash
sudo -s
  ```

and insert the password.

#### 2.3.1 Development version 

  ```bash
cd /opt
git clone https://github.com/tdm-project/tdm-edge.git
cd tdm-edge
git checkout develop
  ```
