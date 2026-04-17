DATA INTERFACE:   
	<Car Operating System>----<Driver Control Unit>  
Form of Communications:  
•	Communication between the Driver Control Unit (DCU) and Car Operating System (COS) uses TCP sockets on port 9113 with the COS acting as server and the DCU as client.  
•	Communication is event-based, meaning that messages are sent when something changes rather than at regular intervals.  
•	Messages are sequences of 8-bit characters.  
•	Each message has a command followed by a value (optional). The delimiter for the command string is [0x03] and the delimiter for the value string (and for the entire message) is [0x04].  
Form of Commands:  
•	Each command has three forms:  
1)	a normal form that specifies a value for a state variable  
2)	a _STATUS form used to report values back to the DCU as confirmation or in response to a query  
3)	a _QUERY form used by the DCU to find out the current value of a state variable  
•	All letters in command names and values are upper case.   
•	Underscores separate words.    
•	Numbers are sequences of ASCII/Unicode characters. There are no leading zeros, so a one will be 1, a ten will be 10, and one hundred will be 100.   
•	The only numbers are integers.  
Implemented Commands:  
•	Normal form of commands (each command name corresponds to the name of a state variable):  
FORWARD <number in range 0 to 255 with 0 for no throttle, 255 for full throttle>  
REVERSE <number in range 0 to 255 with 0 for no throttle, 255 for full throttle>  
STEERING <number in range -127 to 128 with -127 for full left and 128 for full right>  
BRAKES <number in range 0 to 255 with 0 for no braking and 255 for full braking>  
HEADLIGHTS OFF (no light) LOW_BEAM (normal low-beam headlights) HIGH_BEAM (high-beam headlights)  
TURN_SIGNAL OFF LEFT RIGHT  
REVERSE_LIGHTS OFF ON  
BRAKE_LIGHTS OFF ON  
WIPERS_FRONT OFF ON INT1 INT2 ... INT6 (for intermittent or intervals, with INT1 being the longest time with the wipers off and INT6 with the shortest time with wipers off) REGULAR FAST  
WIPERS_BACK same values as WIPERS_FRONT  
WASHER_FRONT OFF ON  
WASHER_BACK OFF ON  
HORN OFF ON  
HAZARD_LIGHTS OFF ON  
   
Notes on Commands:  
•	The DCU must make sure that the reverse lights and brake lights are turned on when the driver puts the car in reverse or brakes.  
•	The DCU must blink the turn signals and hazard lights on and off by sending on and off signals to the COS.   
  
Sample Sequences:  
  
•	Driver brakes  
DCU to COS:  FORWARD[0x03]25[0x04]  
COS to DCU:  FORWARD_STATUS[0x03]25[0x04]  
DCU to COS:  BRAKES[0x03]100[0x04]  
COS to DCU:  BRAKES_STATUS[0x03]100[0x04]  
DCU to COS:  BRAKE_LIGHTS[0x03]ON[0x04]  
COS to DCU:  BRAKE_LIGHTS_STATUS[0x03]ON[0x04]  
  
•	Driver releases brakes  
DCU to COS:  BRAKES[0x03]0[0x04]  
COS to DCU:  BRAKES_STATUS[0x03]0[0x04]  
DCU to COS:  BRAKE_LIGHTS[0x03]OFF[0x04]  
COS to DCU:  BRAKE_LIGHTS_STATUS[0x03]OFF[0x04]  
  
•	DCU is updating driver display:  
DCU to COS:  HEADLIGHTS_QUERY[0x03][0x04]  
COS to DCU:  HEADLIGHTS_STATUS[0x03]OFF[0x04]  
DCU to COS:  WIPERS_FRONT_QUERY[0x03][0x04]  
COS to DCU:  WIPERS_FRONT_STATUS[0x03]INT3[0x04]  
DCU to COS:  WIPERS_BACK_QUERY[0x03][0x04]  
COS to DCU:  WIPERS_BACK_STATUS[0x03]INT1[0x04]  
