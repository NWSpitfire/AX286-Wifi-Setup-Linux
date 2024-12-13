# AX286-Wifi-Setup-Linux
 Setup AX286 USB Wifi adapter in Ubuntu Linux

 ## Adapter Information

 Fenvi AX286

 Wifi Chipset: AIC8800

 # Installation

 ## Install WiFi Driver

1. Navigate to the Fenvi Website and download the driver package to your computer

[Fenvi direct download link - linux](https://download.fenvi.com/support/USB/18286%20USB网卡-Linux驱动.zip)

[Fenvi Driver Page](https://fenvi.com/drive.html?keyword=ax286)

###### NOTE: The folder will likely be labelled in chinese, but contain "AX300". This is correct and works with the AX286

2. Search the downloaded driver folder for "AX300-WiFi-Adapter-Linux-Driver-amd64.deb" file. Use SCP to send this file to the home directory of the linux server.

3. Update the system to latest packages.

    sudo apt update && sudo apt upgrade

4. Install dependencies for linux driver installation

    sudo apt-get install build-essential net-tool wpasupplicant

5. Run AX300 Wifi installer package.

    sudo dpkg -r AX300-WiFi-Adapter-Linux-Driver.deb

###### NOTE: If deb package install fails, you will likely have to force remove the package, and reboot and try again. Instructions TBA.

6. Check adapters to see if WiFi dongle is listed.

    ip a

The adapter should be listed as "wlx90de8093b056" or something similar.

7. Identify the netplan file.

    ls /etc/netplan

The file should be labelled something like "00-installer-config.yaml"

8. Edit the netplan file to add/enable the WiFi dongle. An example file is included below, however you will need to update the WiFi & Ethernet alias' to match your system (these can be obtained by running an "ip a").

###### NOTE: Make sure WPA Supplicant is installed!!

network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: true
  wifis:
        wlx90de8093b056:
            access-points:
                "WiFi-SSID":
                    password: "WiFi-Password"
            dhcp4: true

9. Have Netplan generate the file.

    sudo netplan generate

10. Apply the file using Netplan

    sudo netplan apply

11. Wait 20s for the connection to be established, then run the command to list the network adapter, if all is well you should see an IP address has been given to the WiFi adapter.

    ip a
