# Notes 2019-05-13

## modem type
```
ati
Manufacturer: Sierra Wireless, Incorporated
Model: EM7455                                                                                                                            
Revision: SWI9X30C_02.24.05.06 r7040 CARMD-EV-FRMWR2 2017/05/19 06:23:09                                                                 
MEID: 35907306114129                                                                                                                     
IMEI: 359073061141291                                                                                                                    
IMEI SV: 12                                                                                                                              
FSN: LF826428310410                                                                                                                      
+GCAP: +CGSM  
```

## Check the qmi_wwan driver
```
lsusb -t
...
        |__ Port 4: Dev 7, If 8, Class=Vendor Specific Class, Driver=qmi_wwan, 480M
        |__ Port 4: Dev 7, If 10, Class=Vendor Specific Class, Driver=qmi_wwan, 480M
        |__ Port 4: Dev 7, If 2, Class=Vendor Specific Class, Driver=qcserial, 480M
        |__ Port 4: Dev 7, If 0, Class=Vendor Specific Class, Driver=qcserial, 480M
        |__ Port 4: Dev 7, If 3, Class=Vendor Specific Class, Driver=qcserial, 480M

```

## check usb composition in terminal (this is default)
```
at!usbcomp?
Config Index: 1
Config Type:  1 (Generic)
Interface bitmask: 0000050D (diag,nmea,modem,rmnet0,rmnet1)
```


qmi-network /dev/cdc-wdm0 stop

Loading profile at /etc/qmi-network.conf...
    APN: 3internet
    APN user: unset
    APN password: unset
    qmi-proxy: no
Network already stopped
Clearing state at /tmp/qmi-network-state-cdc-wdm0...

qmi-network /dev/cdc-wdm1 stop

sudo ifconfig wwan0 down
sudo ifconfig wwan1 down

#####

## config interface

sudo su



```
root@raspberrypi:/home/pi# cat /sys/class/net/wwan1/qmi/raw_ip
N
root@raspberrypi:/home/pi# echo Y > /sys/class/net/wwan1/qmi/raw_ip
root@raspberrypi:/home/pi# cat /sys/class/net/wwan1/qmi/raw_ip
Y
root@raspberrypi:/home/pi# qmicli --device=/dev/cdc-wdm0 --device-open-proxy --get-wwan-iface
wwan0
root@raspberrypi:/home/pi# qmicli --device=/dev/cdc-wdm1 --device-open-proxy --get-wwan-iface
wwan1
root@raspberrypi:/home/pi# sudo qmicli --device=/dev/cdc-wdm0 --get-expected-data-format
raw-ip
root@raspberrypi:/home/pi# sudo qmicli --device=/dev/cdc-wdm1 --get-expected-data-format
raw-ip
root@raspberrypi:/home/pi# ip link set dev wwan0 down
root@raspberrypi:/home/pi# ip link set dev wwan1 down
```

## start interface
```
root@raspberrypi:/home/pi# ip link set dev wwan0 up
root@raspberrypi:/home/pi# qmicli --device=/dev/cdc-wdm0 --device-open-proxy --wds-start-network="ip-type=4,apn=three.co.uk" --client-no-release-cid
[/dev/cdc-wdm0] Network started
	Packet data handle: '63269680'
[/dev/cdc-wdm0] Client ID not released:
	Service: 'wds'
```

## DHCP request

```
root@raspberrypi:/home/pi# udhcpc -q -f -n -i wwan0
udhcpc (v1.22.1) started
No resolv.conf for interface wwan0.udhcpc
Sending discover...
Sending select for 10.190.115.76...
Lease of 10.190.115.76 obtained, lease time 7200
Too few arguments.
Too few arguments
```

## ping test

```
root@raspberrypi:/home/pi# ping bbc.co.uk
PING bbc.co.uk (151.101.128.81) 56(84) bytes of data.
64 bytes from 151.101.128.81 (151.101.128.81): icmp_seq=1 ttl=54 time=44.1 ms
64 bytes from 151.101.128.81 (151.101.128.81): icmp_seq=2 ttl=54 time=41.8 ms

--- bbc.co.uk ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 41.870/43.001/44.132/1.131 ms


####

## Speed test

22Mbps down
8 Mbps up


