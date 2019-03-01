
## Config the modem 
Shell - then select QMI mode (windows use mode 8)

minicom -D /dev/ttyUSB2

```
at!entercnd="A710"
AT!UDUSBCOMP=6
at!reset 
```
CTRL A Q to exit minicom

# Set the apn - probably not needed
at+cgdcont=1,"IP","3internet","",""  

Shell

```
sudo su
echo "APN=three.co.uk" >/etc/qmi-network.conf
```

# Start the session
sudo qmicli -d /dev/cdc-wdm0 --wda-set-data-format=raw-ip

sudo qmicli -d /dev/cdc-wdm0 --wda-set-data-format=802-3
sudo qmi-network /dev/cdc-wdm0 start
sudo dhclient -v wwan0

qmicli -d /dev/cdc-wdm0 --wds-start-network="apn="3internet" --client-no-release-cid -v

qmicli -d /dev/cdc-wdm0 --wds-start-network="apn="3internet,ip-type=4" --client-no-release-cid -v

[/dev/cdc-wdm0] Network started
        Packet data handle: '41221944'
[/dev/cdc-wdm0] Client ID not released:
        Service: 'wds'
            CID: '4'

sudo qmicli -d /dev/cdc-wdm0 --wds-start-network --client-no-release-cid --device-open-mbim


qmicli --device=/dev/cdc-wdm0 --device-open-proxy --wds-start-network="ip-type=4,apn=3internet" --client-no-release-cid


https://techship.com/faq/how-to-step-by-step-set-up-a-data-connection-over-qmi-interface-using-qmicli-and-in-kernel-driver-qmi-wwan-in-linux/