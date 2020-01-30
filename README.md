# mdot2usbc_notes/README.md
This git (https://github.com/johnofleek/mdot2usbc_notes/) contains information about the M.2 module to USB C adapter board

John Thompson 20190307

*What does the M.2 to USB C board (PCA) do?*   
It interfaces (adapts) USB C to M.2 plug in modules  
  
  
## Connectors
### USB C  
Supports
* USB 3.1 G1 and G2 up to 10 Gbps  
* USB 2.0 Hi-Speed up to 480 Mbps  

### M.2  
Based on the PCI Express M.2 standard with USB 2.0 and USB 3.0 interfaces  
Connector is an M.2 KEY B  

### 5V DC power connector J3
The power connector fitted to the board is a  
MOLEX mini-SPOX  2.5mm Header, Right Angle, Shrouded, Friction Lock, 2 circuits *22-05-7025*

### Micro SIM 
The board supports 1x micro SIM and one embedded (solder down SIM) 
  
  
## DC power input
The board is powered from a nominal 5V DC input  
There are two power source input connections to the board  
1. J3_PIN1 (+5V) J3_PIN2 ground (0V)  
2. USB C VBUS (+5V) USB C GND (0V)  

Note that the J3 and and USB C 5V signals are diode or'd so that the highest input voltage will supply the current to the boards internal DC power supply  

The board has a regulator which powers the M.2 module with 3.4V  

### Preferred DC power source - method 1
* Apply 5V DC power J3
* USB C 5V controls the power sequence to M.2 module  
  
This is the preferred method because  
1. It enables the board to control the M.2 module power down sequence - *see M.2 module datasheet*  
2. The 5V power source current capability is not dependant on the hosts USB current capability

### Non-referred DC power input - method 2
* No power source is connected to 5V DC power J3
* USB C 5V provides DC power and controls the power sequence to M.2 module

This is the non-preferred method because  
1. The M.2 module power down sequence is uncontrolled - *see M.2 module datasheet*  
2. The 5V power source current capability is dependant on the hosts USB current capability
  
  
## M.2 module signal sequencing  
The board has a microcontroller which sequences the power up and power down of the M.2 board. The current sequence with an external 5V power source and USB C hot plugged in and out - is like this  

![Image of power sequence](https://github.com/johnofleek/mdot2usbc_notes/blob/master/M_2_sequence20190307.png)  
  
  
## Board dimensions
The board is 61.5mm by 30.5mm - the following drawing is of the initial product build  

![Image of board](https://github.com/johnofleek/mdot2usbc_notes/blob/master/M2PCB_20190306.jpg)  
  
  
## Board supply
The board can be purchased from   
[Linkwave Technologies](http://linkwave.co.uk)   
    
    
# Host connectivity
The git contains some information on connectivity using qmi.

There is an alternative connectivity option MBIM, this requires configuration of the EM module.

The following link is a site which covers our Raspberry pi product - but there is also some documentation describing using MBIM with the EM modules
[link](https://johnofleek.github.io/PiloT/docs/networkManagerDocs/instructions_EM7455.html)  

Note that MBIM is supported by Windows 10

    
# This git
## Checkin
```
git add .
git commit -m "first commit"
git push -u origin master
```
  
## Clone
```
git clone https://github.com/johnofleek/mdot2usbc_notes.git
```

