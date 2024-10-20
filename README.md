java cSCC.369 Coursework 1: Working with GPIO
Moodle submission 16:00 Friday week 4; weighting 33% of module.
Aim
Having familiarized yourself with the C development environment and the basic process of writing to registers to control the GPIO pins, in this coursework you will explore some of the other functionality that’s possible with GPIO pins. Along the way you will get more comfortable working with registers to control the MCU and a better idea of the capabilities of the nRF52 series of MCUs. You will also start working with breadboards and electronic components.
Instructions for all subtasks
The coursework is split into a number of subtasks, listed below. Implement each one of them from first principles using pure C. This means that you are NOT permitted to use any library functions that you have not written yourself (apart from for debugging over serial) – only #include "MicroBit.h". In addition to C keywords and C operators, you can use typedefs, structs and literals from the nRF SDK.
We will be looking at and testing your code with the help of some automated scripts, so it’s super important that you follow the following guidelines. If you do not, you will lose marks:
1. Write and submit your CW in a file called CW1.cpp.
2. Start with the template CW1.cpp file on Moodle because it has all the functions correctly
listed, you just need to write the code for each one!
3. Within CW1.cpp, write your code for each subtask within the indicated section of the file.
4. Do not change the specified function prototypes (i.e. function name, parameters and return
type) for each subtask, use the ones given in the CW1.cpp template.
5. Do not include a main() function or main.cpp file in your submission, although you will of
course need to use one for your own testing. You might like to use the main() in MainSubtaskRunner.cpp because that’s what we will use when we test your code.
For each subtask, 20-30% of the marks will depend on code quality. The kinds of things we will be looking for include:
• Visually well-formatted and readable code
• Good, elegant code structure and style, e.g.:
o Appropriate use of loops, helper functions, literals etc.
o Initialise MCU peripherals only once where possible, e.g. don’t keep setting the
direction register of a GPIO port if the directions don’t keep changing.
o Only change the bits of a register that you need to, e.g. AND or OR in the bits you
need to change.
• Ample and thoughtful comments including:
o Before function definitions explaining function purpose, parameters etc. o What variables are used for
o The choice of bit patterns and/or literals being written to registers
o The purpose of writing to registers
o The purpose of loops etc.
• No commented-out code with no explanation!
Remember to have fun . Use the labs to ask about anything you don’t understand!
1

Subtask 1, 20%: Display a binary number that counts up at 5Hz
This subtask requires you to write two functions as follows:
Function prototype: void displayBinary(uint8_t value);
Set the bit pattern of a row of LEDs on the micro:bit to match the least significant 5 bits of the unsigned 8-bit value provided as a parameter. The least significant bit should be on the right when looking at the micro:bit with the USB cable pointing up. A ‘1’ in a bit position should turn the corresponding LED on, a ‘0’ should turn the LED off. You can use any row of LEDs on the micro:bit to show this 5 bit number, but only use one row – the LEDs on the other rows should not light up.
The first time displayBinary() is called it will need to initialise GPIOs. It’s good practice not to repeatedly re-initialise registers with the same value, so you could use a local static variable to record the first time displayBinary() is called so that subsequent calls don’t repeatedly initialise.
Function prototype: void countUpBinary(uint8_t initialValue);
Write a function that causes the number on the row you chose above to count up in binary, one count at a time, starting at the value passed in. You should call your displayBinary() function from above. After reaching a displayed count of 0b11111 the counter should ‘keep going’, i.e. wrap around to 0b00000. The frequency of counting should be 5Hz, i.e. 5 counts per second or 200ms per count. Think about how you can test the frequency of counting; the stretch goal is to see if you can adjust it to be within 5% of the target.
Subtask 2, 20%: Display a binary number that counts down/up with buttons A/B
For this subtask you will need to use two GPIO pins as inputs; use the ones connected to buttons A and B on the micro:bit. Check the micro:bit schematic to see which GPIOs they use. There is only one function to write:
Function prototype: void countWithButtonsBinary(uint8_t initialValue);
This function displays the initial count value passed in, using the displayBinary() function from Subtask 1, and updates the display with a new value when a micro:bit button is pressed. Button A should decrement the value by one count, and button B should increment it by one. To make this work well you will need to debounce the button inputs. The count should wrap around to 0b11111 when decremented below zero, and vice-versa. The count should only change on a button press, not on a button release, and it should not keep incrementing whi代 写SCC.369、C++
代做程序编程语言le a button is held down. Remember to use the relevant PIN_CNF[x] register to access all the settings you need.
Subtask 3, 25%: Measure and display an analogue voltage
NB the week 3 lecture will explain aspects of this Subtask.
For this subtask you will configure the GPIO connected to micro:bit pin P0 as an analogue input and read the voltage present on that pin. To test this you will need to apply a variable analogue voltage to that pin. You’ll need a breadboard, a micro:bit breakout adapter, a variable resistor and some jumper wires.
Wire up the ends of the variable resistor to power and ground, and connect the slider to P0.
For this subtask, in addition to your code please submit a photo of your working micro:bit/breadboard setup in .jpg format for some easy marks! Please name it ST3.jpg.
Function prototype: uint8_t sampleVoltage(void);
  2

Write a function to measure the magnitude of the analogue voltage on the large P0 pin of the micro:bit edge connector. There are many ways to configure the analogue-to-digital converter (ADC) on the nRF, but the important thing is that this function returns an 8-bit unsigned value where 0 represents an input of 0V and 255 represents an input of 3V (that the MCU is being powered from). Wire the variable resistor so that fully anticlockwise produces 0V on the wiper and fully clockwise 3V.
Function prototype: void displayVoltageBinary(void);
Write a function to repeatedly display in binary the magnitude of the analogue voltage measured on the large P0 pin. Use your displayBinary() function from Subtask 1 and make sure to display the five most significant bits of the sampled voltage so that the display reaches 0b00000 when the variable resistor is turned fully anticlockwise and 0b11111 when it’s turned fully clockwise.
Subtask 4, 25%: Drive an RGB LED
NB the week 3 lecture will explain aspects of this Subtask.
For this subtask you will connect an RGB LED to P1 (red), P8 (blue) and P9 (green) on the micro:bit edge connector, each via a current-limiting resistor. Use a 220R resistor for red and 100R for blue and green. The LED we are using is a common anode type.
Function prototype: void driveRGB(void);
You can drive the P1, P8 and P9 pins as regular GPIO outputs if you want to see how the LED works with one or more elements lit up. But for the coursework, control each pin with a PWM signal at roughly 1kHz. Driving all three colours at a fixed ratio of 50% on, 50% off gets you over half the marks. Making the LED ‘breathe’ by repeatedly fading from completely off to fully on and back over the course of 2-4 seconds for a full cycle gets more marks, and the stretch is to have the variable resistor from Subtask 3 control the colour at the same time the LED is breathing – a full turn of the resistor knob should run through a wide range of colours such that there are no obvious switches from one colour to another – a nice, gentle fade through a wide colour palette!
For this subtask, in addition to your code please submit a photo of your working micro:bit/breadboard setup in .jpg format for some easy marks! Please name it ST4.jpg.
Subtask 5, 10%: Display a binary number that counts up/resets on touch input
NB the week 3 lecture will explain aspects of this Subtask.
The final subtask has a lower weighting but is here to stretch you!
It’s like Subtask 2 but the display should count up by one count when you touch the golden micro:bit “face” logo above the LEDs. No need to worry about counting down for this subtask though.
Function prototype: void countWithTouchesBinary(uint8_t initialValue);
This function displays the initial count value passed in, using the displayBinary() function from Subtask 1, and increments the displayed number by one when the golden micro:bit face logo is touched. A “long-touch” to reset the count to the initialValue will get you extra marks 😊.
Mark Scheme
For each subtask, 70-80% of the marks will be awarded for meeting the functional requirements given. 20-30% of the marks will depend on code quality as described on the first page above. If you
   3

do not use the filename, function prototypes and hardware configuration specified (all repeated in red below) you will lose marks. Your work will be assessed by a combination of automatic processing and manual inspection. Your final grade will be based on a weighted mean of your subtask marks.
        Subtask
Hardware config
Weight
To be submitted
(submit code in CW1.cpp)
        1: Display a binary number that counts up at 5Hz
One row of micro:bit display
20%
displayBinary() countUpBinary()
         2: Display a binary number
that counts down/up with buttons A/B
Same row of micro:bit display; the micro:bit buttons
20%
countWithButtonsBinary()
        3: Measure and display an analogue voltage
Same row of micro:bit display; variable resistor wired to edge connector pin P0
25%
sampleVoltage() displayVoltageBinary() ST3.jpg photo of hardware
        4: Drive an RGB LED
RGB LED wired to edge
connector pins P1, P8 and P9
25%
driveRGB() ST4.jpg photo of hardware
        5: Display a binary number that counts up/resets on touch input
Same row of micro:bit display; the touch-sensitive micro:bit ‘face’ logo
10%
countWithTouchesBinary()
    4

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
