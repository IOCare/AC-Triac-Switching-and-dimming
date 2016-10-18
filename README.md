# AC-Triac-Switching-and-dimming
Theories About AC Triac Switching and dimming

#The gate resistor: a bit of theory

When cruyising the internet for Triac switches, you may have come across a large diversion 
in the value of the gate resistor value. My choice usually is to take the lowest value that 
will still protect the gate and the optocoupler. Reader Mali Lagunas did some research in 
the theory behind the criteria to choose the value of the gate resistor. 
Which i will copy below:

The resistor when placed in this circuit, will have two main effects:
a) It will limit/provide the current going into the gate of the triac (I_{GT})
b) It will cause the voltage to drop when the triac is on (V_R)

The lowest value this resistor may have (for 220 V AC) is R=220*sqrt(2)/I_{TMS}, where I_{TMS} 
is the maximum peak current allowed in the photocoupler’s phototriac. These are surge values, 
thus they are transient and account for a limit before breakdown. Therefore in your circuit R 
would be R=220*sqrt(2)/1=311.12 or 330 ohms, since the MOC3021′s I_{TMS}=1A. This is consistent 
with I_{GM} which is the peak gate current of the TIC206. In your schematic you use 1K which 
would limit the current to 311mA.

This “surge” case may take place only when a pulse is received by the phototriac and it is able 
to conduct I_{GT}, and of course for a line value of 220*sqrt(2). Charge will then accumulate in 
the triac’s gate until V_{GT} gets build up and the the triac gets activated.

In quadrant I, ( V_{GT} and A1 are more positive than A2) in order for sufficient charge to 
build up and V_{GT} in the main triac to bee reached, the voltage across the triac must equal 
V_R+V_{TM}+V_{GT} Of course V_R=I_{GT}*R . Commonly, V_{TM}+V_{GT} will both account for 
approximately 3V (datasheet). At the same time, the resistor must provide sufficient current 
to the Triac’s gate, let’s say a minimum of 25 mA (sensitivity of the Triac), thus

V_{triac}= 330ohms*25mA+1.3V+1.1V=10.65V and
V_{triac}= 1k-ohms*25mA+1.3V+1.1V=27.4V (the value in your circuit)

Which is the voltage needed to activate the triac. Therefore, the smaller the resistor the 
less voltage is required to switch on the main triac. What goes on afterwards is mainly that 
there is a voltage drop across A1 and A2 and therefore the phototriac voltage and current will 
drop causing turn-off state (of the phototriac). The main triac will be kept switched on if 
the holding current I_H is respected. When the load current is below I_H, the main triac is 
switched off until a pulse from the photodiode is emitted again in order to polarize V_{GT} 
and build the required charge in the next cycle. Q1 and Q3 are the quadrants for this setup.



EDITED:
![alt text][logo]![alt text][logo]![alt text][logo]![alt text][logo]![alt text][logo]![alt text][logo]
![TRIAC Control(http://images.elektroda.net/78_1263612065.jpg "TRIAC Control")

firstly, remember that the circuit works in two conditions:

1. The MOC3020 is not conducting so although there is high voltage across it, the current is very small (leakage only) 
so the power dissipated (V*I) is small.

2. The MOC3020 is conducting so the voltage across it is decided by the forward drop of the triac in it's conducting state.

There is a brief middle state where the triac is partially conducting.

The resistors are calculated to allow sufficient gate current, taking into account the drop across the MOC3020 and possibly 
current through the capacitor from the voltage across the A1 and A2 pins. Remember that once 'fired' the triac remains 
conducting by itself until its current/voltage drops close to zero.

I would advise against using very small resistors although by calculation they may be adequate. There are two reasons, 
the most important being that most 0.25W rated resistors are also only rated at 250V and the circuit will have 
at least √2 times the RMS voltage across it and quite likely more due to power spikes and load switching. 
The other reason is that during the turn on and off periods, the dissipation could be significantly higher than 
in a steady condition. The sudden heat rise in small resistors can result in the carbon film cracking or rupturing. 
For the sake of a few pennies a larger resistor will overcome these problems.
