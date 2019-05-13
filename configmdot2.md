This note documents a process to bring up a Debian linux wwan interface using the
 Qualcomm QMI interface with the *RAW-IP* data link format.

These notes were tested on a Raspberry Pi3
```
cat /etc/os-release
PRETTY_NAME="Raspbian GNU/Linux 9 (stretch)"
NAME="Raspbian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
```


# QMI
Many Sierra Wireless cellular modules based on Qualcomm chipsets implements
 the Qualcomm MSM (QMI) Interface

## Some Sierra Wireless modules versus data link format 

*802.3* 
Sierra Wireless MC73**/EM73**

*RAW-IP* 
Sierra Wireless MC74**/EM74** 
Sierra Wireless EM75**

## QMI - devices
The cellular modules QMI control interface are usually named cdc-wdm* e.g.
/dev/cdc-wdm0




# Install required Linux apps

Installs qmicli command line application (and others)
```
sudo apt-get install libqmi-utils
```

Installs microcom - a terminal application useful for manually configuring the module
 via AT commands
```
sudo apt-get install minicom
```

Install udhcpu  
It's used to get an ip address from a dhcp server.
```
sudo apt-get install udhcpc
```

# Check the drivers
Verify that you have the Linux in-kernel qmi_wwan driver installed for
 the cellular modules exposed QMI interface endpoint over USB:
lsusb -t
Can look e.g. like this:
```
|__ Port 1: Dev 3, If 2, Class=Vendor Specific Class, Driver=qmi_wwan, 480M
```

If the driver is not correctly loaded, please verify that the module is set
 to expose the correct USB endpoints configuration toward the host system and
 that you have followed the provided guides from the cellular module vendors,
 regarding how to implement the module in Linux.

# Configure the module usb composition
If the modules needs configuration
Check that module serial ports are available
```
ls /dev/ttyU*
/dev/ttyUSB0  /dev/ttyUSB1  /dev/ttyUSB2
```

The last usb serial port is the AT command port. So in this case
```
minicom -D /dev/ttyUSB2

Check the usb composition
```
at!entercnd="A710"
OK
AT!UDUSBCOMP?
!UDUSBCOMP: 6

OK
```
6== DM, NMEA, AT, QMI


# EM7455 - composition doesn't have AT!UDUSBCOMP

```
at!usbcomp?
Config Index: 1
Config Type:  1 (Generic)
Interface bitmask: 0000050D (diag,nmea,modem,rmnet0,rmnet1)
```



If the composition is not 6 - reconfigure the module
```
at!entercnd="A710"
OK
AT!UDUSBCOMP=6

OK
at!reset 
```

Exit minicom with  
<Ctrl> a q


# Linux shell - useful commands

If the SIM pin is configured:
```
qmicli --device=/dev/cdc-wdm0 -p --dms-uim-verify-pin=PIN,1234
```

List the module network interface with:
```
sudo qmicli --device=/dev/cdc-wdm0 --device-open-proxy --get-wwan-iface
```

# QMI data format configuration

Check the qmi data format
```
sudo qmicli --device=/dev/cdc-wdm0 --get-expected-data-format
802-3
```
This because the default setting is
```
cat /sys/class/net/wwan0/qmi/raw_ip
N
```
First make sure the network interface is down (it should be)
```
sudo ip link set dev wwan0 down
```
Configure the qmi data format
```
echo Y > /sys/class/net/wwan0/qmi/raw_ip
```

Bring the network interfaces up:
```
ip link set dev wwan0 up
```

# set the APN and start the network connection
You can also set other credentials - please refer to the qmicli manual

```
qmicli --device=/dev/cdc-wdm0 --device-open-proxy --wds-start-network="ip-type=4,apn=three.co.uk" --client-no-release-cid
```

Once "Network started" is displayed, you can send a DHCP request on the
 network interface.
Please note that not all DHCP clients in Linux can support Raw-IP format,
 however udhcpc supports IPv4 over Raw-IP.

```
udhcpc -q -f -n -i wwan0
```

# Disconnect

Disconnect the data bearer and data connection over QMI.

The command below disconnects the session.

Provide the network handle and CID returned at connection activation
```
qmicli --device=/dev/cdc-wdm0 --device-open-proxy --wds-stop-network=NETWORK_HANDLE --client-cid=CID
```




