# Test 3G mode WP76

at!entercnd="A710"
OK


at!daftmact
290300

OK
at!dasband=9		//3G 2100MHz
9

OK
at!daschan=9750		//Middleof the band
9750

OK

+CREG: 0

+CGREG: 0

+CEREG: 0
at!dawsparange=3	//Set PA to high power
3

OK
at!dastxon		//Turn transmitter on


OK
at!dawstxpwr=1,23		//Set power to 23dBm here bench power supply started drawing 700mA+


OK
at!dastxoff


OK
