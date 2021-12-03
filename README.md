# Software Multiplexed Display Report

## Introduction:
This week we were tasked with creating a second counter in hexadecimal using the 7-segment display. We need to ensure that the second counter is within a 10% margin of accuracy. To test the accuracy of the timer we will use timers on our phones and allow the 7-segment display to count to 0x20 or 32 seconds. If the time on the timer from the phone is within 3.2 seconds of 32 seconds when 0x20 is displayed then our design has met the standards. 
## Plan:
Team FLAC continued using the upside up and scrolling pattern 0,3,1,2. The GPIO mapping for the registers for the debug, comm, and segment were also set. The GPIO register is connected to the GPIO pins on the board which in turn are connected to the pins on the 7-segment display and the debug LEDs.
__
The display decoder was created in C this week as opposed to Verilog. Within the main loop of firmware.c file there is a function (segmen_decode) that takes in the hex value from hex_to_display and returns the binary representation of it on a 7 segment display. This value is then used to create the reg gpio value. After this there is a switch statement which controls the scrolling order. Within each case the statement sets the next values for the display digit, comm, and hex to display. 
__
In verilog these steps were handled in a similar fashion, but instead of needing functions and variables, values were updated and read from registers with two case statements. One for the hex2display, and another for the comm based on the refresh counter register. 
## Implementation:
Our first step was in setting up the register. The register, reg_gpio, is used to enable the GPIO pins for digital input/output. For this, we had to mask and shift the bits in order to map to the correct pins, which the debug leds resided.
__
With this completed we were able to complete the switch statement to control the scrolling order as well as take into account the speed. Now, each segment of this particular display consists of an led that lights-up when positive voltage is applied to the anode, and this switch statement is to control the next values in our order, 0312, and sets the next values for the display, the COMM variable and the hex. 
__
## Results:
After running our first test, the timing of our counter was clearly off. To get more information on this we ran a few timers on our phones simultaneously to compare. We stopped our timers when the display reached 20 in hexadecimal, in which we would expect to be at 32 seconds, but it was too fast. To fix this we needed to change cycles_to_wait in the following spin_wait_ms() 
__
When increasing this number, the speed of the counting slowed down. We kept adjusting this number and testing the result with our timers until we fell within a 10% variation of that 32 seconds target. The number we settled on to reach this level of accuracy was 40, as seen above. The following video is our finished project
## Conclusion :
Overall we were able to create a second counter using the 7-segment display just as expected. We ran into a few issues while creating the timer; first being the comm bits being inverted. The board would turn on three of the bits on the display while leaving one off, and the one turned off was the only one bit that we wanted on. To fix this issue we placed a not in front of each reference to comm in the case statements. Our other issue was that the display would count significantly faster than a second. From there we adjusted the delay of the timer and slowed the count down. Finally we confirmed that our design was within 3.2 seconds after allowing it to run for 32 seconds. We did that by using the timers on our phones. In the end we created a design that met all of the criteria.
