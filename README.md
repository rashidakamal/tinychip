# Tiny chip, Mega fun
An ATTiny85 &amp; ATmega328P microcontroller chip workshop for ITP's 2019 Unconference.

## Resources

This is where I learned everything!
+ [Instructable: Use a $1 ATTiny to Drive Addressable RGB LEDs](https://www.instructables.com/id/Use-a-1-ATTiny-to-drive-addressable-RGB-LEDs/)
+ [Arduino board as ATtiny programmer](http://highlowtech.org/?p=1706)
+ [From Arduino to a Microcontroller on a Breadboard](https://www.arduino.cc/en/Tutorial/ArduinoToBreadboard)
+ [Programming ATtiny85 with Arduino Uno](https://create.arduino.cc/projecthub/arjun/programming-attiny85-with-arduino-uno-afb829)

## Agenda

1. Purpose
2. Materials
3. Setup
4. Fun
5. Miscellaneous

### Purpose

### Materials

+ Arduino Uno (or other microcontroller board you are comfortable with)
+ ATTiny85 chip
+ .10 uF capacitor
+ 100 ohm resistor
+ 5V power supply (can be from your Arduino, battery, independent power supply, etc.)
+ Male-to-male jumpers

### Setup

We have a few options for programming our ATTiny85 chips. We can use a [Tiny AVR Programmer](https://www.sparkfun.com/products/11801?_ga=2.254947915.295978248.1548287842-129504373.1548287842) (which costs $15.95!!!) or, we can temporarily reprogram our Arduinos to act as an ISP, or "in-system programmer."

**About the ATTiny85**

To understand how we will go about communicating with our ATTiny85 chip, let's take a look at its pins:

![](/images/pins.png)

It's got 3 analog pins, 2 PWM pins ...

**Let's reprogram our Arduinos so that we can program our ATTiny.**

1. Open the Arduino IDE. Load the ArduinoISP sketch from File -> Examples -> ArduinoISP. Upload this sketch onto your Arduino board.
2. Next, wire your Arduino to your ATTiny85 chip as seen below:

![](/images/isp.png)

3. Select ATtiny25/45/85 from Tools -> Board... You may have to have to search and install the ATtiny25/45/85 from the Boards Manager (also found under Tools -> Board...).
4. Select 8 MHz (internal) under Tools -> Clock. By default, the clock rate of ATTiny85 runs at 1 Mhz, but this will be too slow for most things we want to do.
5. Select "Arduino as ISP" from Tools > Programmer
6. Run Tools > Burn Bootloader. This step is necessary to reset the clock rate on the ATTiny85.
7. Once the bootloader has been successfully burned, you can upload whatever program you want onto your IC. (Blink is probably a good place to start.)


Now, your Blink (or whatever program) can run independently of your Arduino, though you will still need to give it a power supply. Your Arduino is free to be used for something else.

Beyond the scope of this workshop, but the next step is:

8. Solder your desired circuit (+ IC chip) onto perf board. Be free of your breadboard!

### Fun

We can see more complex programs too! Let's try to program some Neopixels.

### Miscellaneous

A bunch of other resources and questions I encountered while learning about this:

+ [What exactly is a AVR microcontroller](https://en.wikipedia.org/wiki/AVR_microcontrollers)
+ What is a bootloader? When do you burn it??
