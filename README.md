# mdot2usbc_notes
This git contains information about the M.2 module to USB C adapter board

*What does the M.2 to USB C board (PCA) do?*   
It interfaces (adapts) USB C to M.2 plug in modules  


## DC power input
There are two power sources to the board  
1. J3_PIN1 (+5V) J3_PIN2 ground (0V)  
2. USB C VBUS (+5V) USB C GND (0V)  
  
The preferred method (1) is as follows
* Apply 5V DC power J3
* USB C 5V controls the power sequence to M.2 module  
  
This is the preferred method because  
1. It enables the board to control the M.2 module power down sequence - *see M.2 module datasheet*  
2. The 5V power source current capability is not dependant on the host USB current capability
  
## Board DC power supply
The board is powered from a nominal 5V DC input  
The board has a regulator which powers the M.2 with 3.4V  

## 5V DC power connector J3
The power connector fitted to the board is a  
MOLEX mini-SPOX  2.5mm Header, Right Angle, Shrouded, Friction Lock, 2 circuits *22-05-7025*

## USB C  
Supports
* USB 3.1 G1 and G2 up to 10 Gbps  
* USB 2.0 Hi-Speed up to 480 Mbps  

## M.2  
Based on the PCI Express M.2 standard with USB 2.0 and USB 3.0 interfaces  
Connector is an M.2 KEY B  
Supports 1x mini SIM and one embedded (solder down SIM)  

## M.2 signal sequencing  
The board has a microcontroller which sequences the power up and power down of the M.2 board. The current sequence with an external 5V power source and USB C hot plugged in and out - is like this  

![Image of power sequence](https://github.com/johnofleek/mdot2usbc_notes/blob/master/M_2_sequence20190307.png)  

## Board dimensions

![Image of board](https://github.com/johnofleek/mdot2usbc_notes/blob/master/M2PCB_20190306.jpg)  




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

