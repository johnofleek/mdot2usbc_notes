## USB devices with microsoft USBView


**Tosh External USB 3.0 harddrive**  
plugged into USB 3.1 A port

```
       ---===>Device Information<===---
English product name: "External USB 3.0"

ConnectionStatus:                  
Current Config Value:              0x01  -> Device Bus Speed: SuperSpeed
```

**EM adapter with EM7455 fitted**  
plugged into USB 3.1 A port

Note with no network the speed is "high" not superspeed
```
       ---===>Device Information<===---
English product name: "Sierra Wireless EM7455 Qualcomm® Snapdragon™ X7 LTE-A"

ConnectionStatus:                  
Current Config Value:              0x01  -> Device Bus Speed: SuperSpeed
Device Address:                    0x1B
Open Pipes:                          14
```

**Mouse - low speed**
```
       ---===>Device Information<===---
English product name: "Microsoft 3-Button Mouse with IntelliEye(TM)"

ConnectionStatus:                  
Current Config Value:              0x01  -> Device Bus Speed: Low
Device Address:                    0x17
Open Pipes:                           1
```

**USB RS232 - high speed**
```
       ---===>Device Information<===---
English product name: "US232R"

ConnectionStatus:                  
Current Config Value:              0x01  -> Device Bus Speed: Full (is not SuperSpeed or higher capable)
Device Address:                    0x1C
Open Pipes:                           2
```

AT+CGDCONT=5,"IP","three.co.uk"
