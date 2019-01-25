# Tiny chip, Mega fun
An ATTiny85 &amp; ATmega328P microcontroller chip workshop for ITP's 2019 Unconference.

## Resources

This is where I learned everything!

+ [Arduino board as ATtiny programmer](http://highlowtech.org/?p=1706)
+ [From Arduino to a Microcontroller on a Breadboard](https://www.arduino.cc/en/Tutorial/ArduinoToBreadboard)
+ [Programming ATtiny85 with Arduino Uno](https://create.arduino.cc/projecthub/arjun/programming-attiny85-with-arduino-uno-afb829)
+ [Programming ATtiny85 with Arduino Uno - Blink](https://create.arduino.cc/projecthub/arjun/programming-attiny85-with-arduino-uno-afb829#toc-testing-attiny85-blink-4)
+ [Instructable: Use a $1 ATTiny to Drive Addressable RGB LEDs](https://www.instructables.com/id/Use-a-1-ATTiny-to-drive-addressable-RGB-LEDs/)

## Agenda

1. Purpose
2. Materials
3. Setup
4. Fun
5. Troubleshooting
6. Miscellaneous

## Purpose: Why Use an IC instead of a Arduino Uno?

Our Arduino Unos use an ATmel AVR chips as their onboard microcontroller (AVR stands for "automatic voltage regulation"). If you look closely at your board, you'll see that it has a ATmega328P. AVR chips are a type of IC, or integrated circuit.

For most purposes, don't need all the features onboard the Arduino Uno (for example, it has an external oscillator that can bump up its clock date to 16 MHZ).

For our workshop, we're going to focus on the ATTiny85. It's a bit smaller than the ATmega328P, but can do most

**PROs**

+ It's way cheaper
+ It't way smaller!

**CONs**
+ The onboard oscillator can go as fast as 8mHZ (meaning that its clock rate is slower than your Arduino Uno).  

## Materials

+ Arduino Uno (or other microcontroller board you are comfortable with)
+ ATTiny85 chip
+ 10 μF capacitor (the shop is out of 10 uF, so I'm using 47 μF)
+ Male-to-male jumpers

**For Blink:**
+ 5V power supply (can be from your Arduino, battery, independent power supply, etc.)
+ LED

**For Neopixels**
+ Neopixel strand
+ 47 μF capacitor
+ Maybe a resistor of low resistance

## Setup

We have a few options for programming our ATTiny85 chips. We can use a [Tiny AVR Programmer](https://www.sparkfun.com/products/11801?_ga=2.254947915.295978248.1548287842-129504373.1548287842) (which costs $15.95!!!) or, we can temporarily reprogram our Arduinos to act as an ISP, or "in-system programmer."

**About the ATTiny85**

To understand how we will go about communicating with our ATTiny85 chip, let's take a look at its pins:

![](/images/pins.png)

It's got 3 analog pins, 2 PWM pins ...

**Program our Arduino to act as an AVR programmer**

1. Open the Arduino IDE. Load the ArduinoISP sketch from File -> Examples -> ArduinoISP. Upload this sketch onto your Arduino board.

**Update settings in our Arduino IDE so it can talk to our ATTiny85**

2. We also need to add the ATtiny85 board to our Board Manager. Go to File -> Preferences and under Additional Boards Manager add this url: `https://raw.githubusercontent.com/damellis/attiny/ide-1.6.x-boards-manager/package_damellis_attiny_index.json`.


3. Select ATtiny25/45/85 from Tools -> Board... You may have to have to search and install the ATtiny25/45/85 from the Boards Manager (also found under Tools -> Board...).
4. Change Processor to ATTiny85 (Tools -> Processor -> ATTiny85).
5. Select 8 MHz (internal) under Tools -> Clock. By default, the clock rate of ATTiny85 runs at 1 Mhz, but this will be too slow for most things we want to do.
6. Select "Arduino as ISP" from Tools > Programmer.

**Connect and initialize our ATTiny85 chip**

7. Next, wire your Arduino to your ATTiny85 so we can actually start talking to it! Wire it as seen below:


![](/images/isp.png)


- 10 μF capacitor between RESET & GND on your Arduino
- IC Pin 8 to 5V
- IC Pin 7 to Arduino Pin 13
- IC Pin 6 to Arduino Pin 12
- IC Pin 5 to Arduino Pin 11
- IC Pin 1 to Arduino Pin 10
- IC Pin 4 to GND


8. By default, the internal clock date of the ATtiny85 is 1 MHz, which is too slow. In order to reset its clock rate, we need to burn the bootloader! Go to Tools > Burn Bootloader.
9. Once the bootloader has been successfully burned, you can upload whatever program you want onto your IC. ([Blink](https://create.arduino.cc/projecthub/arjun/programming-attiny85-with-arduino-uno-afb829#toc-testing-attiny85-blink-4) is probably a good place to start.)

**Load whatever program you want onto your ATTiny85**

Now, your Blink (or whatever program) can run independently of your Arduino, though you will still need to give it a power supply. Your Arduino is free to be used for something else.
If you use Blink from examples, change the Pin # to 0 (which is in position 5 on the ATtiny85 IC).

**Next steps**

Beyond the scope of this workshop, but the next step is:

10. Solder your desired circuit (+ IC chip) onto perf board. Be free of your breadboard!

## Fun

We can see more complex programs too! Let's try to program some Neopixels.

## Troubleshooting

+ Error burning the bootloader: double check your wiring and then click Burn Bootloader again!
+ Double check your wiring using a multimeter.

## Miscellaneous

A bunch of other resources and questions I encountered while learning about this:

+ [What exactly is a AVR microcontroller](https://en.wikipedia.org/wiki/AVR_microcontrollers)
+ [What is a bootloader? When do you burn it??](https://www.arduino.cc/en/Hacking/Bootloader?from=Tutorial.Bootloader)
