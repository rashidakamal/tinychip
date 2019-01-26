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

1. [Purpose](https://github.com/rashidakamal/tinychip#purpose-why-use-an-ic-instead-of-a-arduino-uno)
2. [Materials](https://github.com/rashidakamal/tinychip#materials)
3. [Setup](https://github.com/rashidakamal/tinychip#setup)
4. [Fun](https://github.com/rashidakamal/tinychip#fun)
5. [Troubleshooting](https://github.com/rashidakamal/tinychip#troubleshooting)
6. [Miscellaneous](https://github.com/rashidakamal/tinychip#miscellaneous)

## Purpose: Why Use an IC instead of a Arduino Uno?

Our Arduino Unos use an ATmel AVR chips as their onboard microcontroller (AVR stands for "automatic voltage regulation"). If you look closely at your board, you'll see that it has a ATmega328P. AVR chips are a type of IC, or integrated circuit.

For most purposes, don't need all the features onboard the Arduino Uno (for example, it has an external oscillator that can bump up its clock date to 16 MHZ).

For our workshop, we're going to focus on the ATTiny85. It's a bit smaller than the ATmega328P, but can do most

**PROs**

+ It's way cheaper
+ It's way smaller!
+ It can do most things!

**CONs**
+ Harder for beginners
+ It's a bit slower. The onboard oscillator can go as fast as 8mHZ, which is not noticeable for most use cases but  its clock rate is slower than your Arduino Uno.  
+ No UART serial communications (but yes, I2C and SPI)

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

<p align="center">
  <img src="/images/pins.png">
</p>

It's got 3 analog pins, 2 PWM pins, all of the I/O pins can be used as digital pins.

**Program our Arduino to act as an AVR programmer**

1. Open the Arduino IDE. Load the ArduinoISP sketch from **File -> Examples -> ArduinoISP**. Upload this sketch onto your Arduino board. You only need to do this once, at this step.

**Update settings in our Arduino IDE so it can talk to our ATTiny85**

2. We also need to add the ATtiny85 board to our Board Manager. Go to **File -> Preferences** and under **Additional Boards Manager** add this url: `https://raw.githubusercontent.com/damellis/attiny/ide-1.6.x-boards-manager/package_damellis_attiny_index.json`.


![](/images/1.png)

3. Select **ATtiny25/45/85** from **Tools -> Board...** You may have to have to search and install the ATtiny25/45/85 from the **Boards Manager** (also found under Tools -> Board...).

<p align="center">
  <img src="/images/2.png">
	<img src="/images/3.png">
</p>

4. Change Processor to ATTiny85 (**Tools -> Processor -> ATTiny85**).
5. Select **8 MHz (internal)** under **Tools -> Clock**. By default, the clock rate of ATTiny85 runs at 1 Mhz, but this will be too slow for most things we want to do.
6. Select **"Arduino as ISP"** from **Tools > Programmer**.

Your settings under Tools should look something like this:

<p align="center">
  <img src="/images/4.png">
</p>

**Connect and initialize our ATTiny85 chip**

7. Next, wire your Arduino to your ATTiny85 so we can actually start talking to it! Wire it as seen below:

<p align="center">
  <img src="/images/isp.png">
</p>

- 10 μF capacitor between RESET & GND on your Arduino
- IC Pin 8 to 5V
- IC Pin 7 to Arduino Pin 13
- IC Pin 6 to Arduino Pin 12
- IC Pin 5 to Arduino Pin 11
- IC Pin 1 to Arduino Pin 10
- IC Pin 4 to GND


8. By default, the internal clock date of the ATtiny85 is 1 MHz, which is too slow. In order to reset its clock rate to 8 MHz (as per our settings above), we need to burn the bootloader! Go to **Tools > Burn Bootloader**.

> **Note:** You only need to burn bootloader once (when you first initialize your ATtiny/reset its clock). Afterwards, you can just go directly to step 9, assuming that you're connected to your ATtiny85 as described in step 7.

9. Once the bootloader has been successfully burned, you can upload whatever program you want onto your IC. ([Blink](https://create.arduino.cc/projecthub/arjun/programming-attiny85-with-arduino-uno-afb829#toc-testing-attiny85-blink-4) is probably a good place to start.)

**Load whatever program you want onto your ATTiny85**

Now, your Blink (or whatever program) can run independently of your Arduino, though you will still need to give it a power supply. Your Arduino is free to be used for something else.
If you use Blink from examples, change the Pin # to 0 (which is in position 5 on the ATtiny85 IC). [This tutorial](https://create.arduino.cc/projecthub/arjun/programming-attiny85-with-arduino-uno-afb829#toc-testing-attiny85-blink-4) shows you how to wire your ATTiny85 so that can see Blink in action.  

**Next steps**

Beyond the scope of this workshop, but the next step is:

10. Solder your desired circuit (+ IC chip) onto perf board. Be free of your breadboard!

## Fun

We can see more complex programs too! Let's try to program some Neopixels. [This Instructable](https://www.instructables.com/id/Use-a-1-ATTiny-to-drive-addressable-RGB-LEDs/) shows the basic wiring of an ATTiny85 to a Neopixel strand.

## Troubleshooting

+ Error burning the bootloader: double check your wiring and then click Burn Bootloader again!
+ Double check your wiring using a multimeter.
+ Make sure that you have the `ArduinoISP` sketch loaded onto your Arduino, NOT your ATTiny85 chip. You only want to upload the target script (like, Blink or Strandtest) to your ATTiny85. You can double check the settings under Tools to confirm you're talking to the right board.
+ Make sure that your serial port is talking to the Arduino IDE (disconnect and reconnect your Arduino as necessary, and re-select the correct port).
+ If you're using different development board (Arduino Zero, Curie 101, etc.) you may have to adjust the pins either in the `ArduinoISP` sketch or adjust which pins you connect the ATTiny85 when programming it from your board.


## Miscellaneous

A bunch of other resources and questions I encountered while learning about this:

+ [What exactly is a AVR microcontroller](https://en.wikipedia.org/wiki/AVR_microcontrollers)
+ [What is a bootloader? When do you burn it??](https://www.arduino.cc/en/Hacking/Bootloader?from=Tutorial.Bootloader)
+ [Tiny AVR Programmer Hookup Guide](https://learn.sparkfun.com/tutorials/tiny-avr-programmer-hookup-guide/attiny85-use-hints)
+ [Comparison of ATmel chips](https://thewanderingengineer.com/2013/04/28/comparison-of-atmel-chips/)
+ [What advantages does AVR have over Arduino and vice versa?](https://www.quora.com/What-advantages-does-AVR-have-over-Arduino-and-vice-versa)
+ [Sparkfun Electronics ATtiny85 Arduino Quick Reference Sheet](https://cdn.sparkfun.com/assets/2/8/b/a/a/Tiny_QuickRef_v2_2.pdf)
