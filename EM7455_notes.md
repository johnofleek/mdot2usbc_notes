qmi-network /dev/cdc-wdm0 stop

Loading profile at /etc/qmi-network.conf...
    APN: 3internet
    APN user: unset
    APN password: unset
    qmi-proxy: no
Network already stopped
Clearing state at /tmp/qmi-network-state-cdc-wdm0...

sudo ifconfig wwan0 down

sudo su

echo Y > /sys/class/net/wwan0/qmi/raw_ip


root@raspberrypi:/home/pi# qmicli --device=/dev/cdc-wdm1 --device-open-proxy --wds-start-network="ip-type=4,apn=three.co.uk" --client-no-release-cid
[/dev/cdc-wdm1] Network started
	Packet data handle: '63037024'
[/dev/cdc-wdm1] Client ID not released:
	Service: 'wds'
	    CID: '36'


#####

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
root@raspberrypi:/home/pi# ip link set dev wwan0 up
root@raspberrypi:/home/pi# qmicli --device=/dev/cdc-wdm0 --device-open-proxy --wds-start-network="ip-type=4,apn=three.co.uk" --client-no-release-cid
[/dev/cdc-wdm0] Network started
	Packet data handle: '63269680'
[/dev/cdc-wdm0] Client ID not released:
	Service: 'wds'



root@raspberrypi:/home/pi# udhcpc -q -f -n -i wwan0
udhcpc (v1.22.1) started
No resolv.conf for interface wwan0.udhcpc
Sending discover...
Sending select for 10.190.115.76...
Lease of 10.190.115.76 obtained, lease time 7200
Too few arguments.
Too few arguments

root@raspberrypi:/home/pi# ping bbc.co.uk
PING bbc.co.uk (151.101.128.81) 56(84) bytes of data.
64 bytes from 151.101.128.81 (151.101.128.81): icmp_seq=1 ttl=54 time=44.1 ms
64 bytes from 151.101.128.81 (151.101.128.81): icmp_seq=2 ttl=54 time=41.8 ms

--- bbc.co.uk ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 41.870/43.001/44.132/1.131 ms






