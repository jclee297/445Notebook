# Jonathan's Lab Notebook
## February 8
Original block diagram made:
![image](https://github.com/jclee297/ECE445/assets/168769106/88b3398b-493f-4612-a793-c4dcb57c4d92)
## February 11
Picked the LM317 linear voltage regulator to step down voltage from battery to the microcontroller:
![image](https://github.com/jclee297/ECE445/assets/168769106/04a657f4-92a1-4701-b7c5-f45326d278e3)

Finding ratio for 5V output

![image](https://github.com/jclee297/ECE445/assets/168769106/1bde5c05-7cba-4909-b877-385e34f86db3)

**Block Diagram Q&A**

1. What's powering the main microcontroller?

  LM317 Regulator

3. What battery should we use?

  9V for now

4. What's powering the ESP32-CAM

  Likely also a 9V battery stepped down to 5V via LM317

6. How much power does the ESP32-CAM need?

  5V 2A according to datasheet

7. Allowable voltage for alarm speakers?

  Up to 24V

8. How does ESP32-CAM connect to the actual camera?

  Actual camera is OV2640, connection is built onto ESP32-CAM. Datasheet calls for 5V and 180mA
  [datasheet](https://www.handsontec.com/dataspecs/module/ESP32-CAM.pdf)

9. How will the microcontroller communicate with the ESP32-CAM?

    ESP32-S3-WROOM can use Bluetooth 5


## Febuary 18

New block diagram made. ESP32-CAM has it's own voltage regulator that can support up to 15V. Can directly power this via 9V battery. 9V battery might not have enough capacity to power the master microcontroller for at least 2.5 hours depending on the current draw and the current active/sleep mode it's in.
![image](https://github.com/jclee297/ECE445/assets/168769106/5b1b83f7-e3a7-4813-b9d7-4b39ea65a511)

Design issue: How will we unlock and lock chain without the alarms going off? 

Instead of sending a wire from our microcontroller through the entire length of the bike chain and out the other end to the microcontroller, insert wire in a loop (U shape). Bike lock can lock and unlock safely without tripping the alarm, and will only trip the alarm when the chain is actually cut.

![image](https://github.com/jclee297/ECE445/assets/168769106/f11a6eb9-698c-4d56-ba28-b179714193ea)

## Febuary 22

Trying to figure out MOSFET circuit
Is Drain resistor necessary?

![image](https://github.com/jclee297/ECE445/assets/168769106/73644bc7-e987-4acf-978a-994d9536436c)


16550 3.7V Li-ion battery? Has 2500mAh but can't step down to 3.3 to power ESP32 due to dropout voltage being about 2 volts. Can't power ES32-CAM since many forum posts saying 3.3V power not recommended and buggy. ESP32-CAM will have to use 5V

## March 13

Did research for current detection via a BJT circuit. Seemed a little confusing so explored other methods.

![image](https://github.com/jclee297/ECE445/assets/168769106/d47f8f45-6471-4700-94c2-f278f9ebac5a)

According to ESP32-S3-WROOM datasheet, VIH has to be above 0.75 x VDD and VIL must be below 0.25 x VDD

![image](https://github.com/jclee297/ECE445/assets/168769106/19afc6d2-2c5f-4033-9e57-9f9fcf5b9f56)

Depending on the opamp, both cases (switch open/closed) will cause a  GPIO pin to read as high. Need to change circuit/opamp so an open circuit actually causes a low GPIO pin input.

Open circuit detection can also be done without the opamp by using the microcontroller GPIO pins to act as a detector of voltage, and have the wire running through the bike chain powered by a constant voltage that the microcontroller can handle and read as high. Voltage regulator output of 3.3V should work?

Spent time designing PCB in time for second order. Purposely making our first rendition larger to give myself enough space to place traces and components from our block diagram. (First time designing a PCB at all)

## March 14

Voltage regulator circuit and IO pins placed on the schematic. Time spent trying to understand the method for uploading code to the microcontroller, and how we should connect FTDI USB to the PCB.

## March 15

Noted the surface mount components needed so far for the PCB.

## March 19

PCB schematic finished. Had to find ESP32 footprint online and edit the vias to be the right size

## March 20

Took the time to order all the parts needed for the first rendition and should still have enough for the second and possibly third, depending on how many PCBs we solder and test

## April 5

Finally got PCB. Worked on soldering the components, but realized I forgot to order BJTs and switches. 1uF capacitors were missing so had to reorder anyways. Tested the output of the voltage regulator and read 2.3V. Tried changing resistors with other values to get different voltage outputs, but after a couple hours realized that I originally had the wrong value because the resistors were in the wrong spots. 3.3K goes on the bottom and 2.2K goes on the top. While testing, learned that to read the right output, the voltage regulator needs to have a load resistor, otherwise the output is higher (~5V).

## April 12

Got the missing componenets and soldered it together. Also got the microcontroller so soldered the first PCB. Tested the voltage regulator output and realized 
