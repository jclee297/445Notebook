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

Depending on the opamp, both cases (switch open/closed) will cause a  GPIO pin to read as high. Need to change circuit/opamp so an open circuit actually causes a low GPIO pin input. Tried using LTspice to get a general idea of what the circuit would look like and how it would work, but tests were inconclusive and couldn't experiment with the actual MOSFET chip I wanted to use.

Open circuit detection can also be done without the opamp by using the microcontroller GPIO pins to act as a detector of voltage, and have the wire running through the bike chain powered by a constant voltage that the microcontroller can handle and read as high. Voltage regulator output of 3.3V should work?

Spent time designing PCB in time for second order. Purposely making our first rendition larger to give myself enough space to place traces and components from our block diagram. (First time designing a PCB at all)

## March 14

Voltage regulator circuit and IO pins placed on the schematic. Time spent trying to understand the method for uploading code to the microcontroller, and how we should connect FTDI USB to the PCB.

## March 15

Noted the surface mount components needed so far for the PCB.

## March 19

PCB schematic finished. Had to find ESP32 footprint online and edit the vias to be the right size
![image](https://github.com/jclee297/ECE445/assets/168769106/ecbc2f23-e2be-43c4-b2ec-bd0677663b6c)


## March 20

Took the time to order all the parts needed for the first rendition and should still have enough for the second and possibly third, depending on how many PCBs we solder and test.

## March 26

Designed the second rendition of the PCB. Not many changes except added more GPIO pins to use in case the few that I connected in the first rendition didn't work for whatever reason.
![image](https://github.com/jclee297/ECE445/assets/168769106/a5cea1f4-b965-4c6a-a144-7a56399e2b28)


## April 5

Finally got the first rendition PCB. Worked on soldering the components, but realized I forgot to order BJTs and switches. 1uF capacitors were missing so had to reorder anyways. Tested the output of the voltage regulator and read 2.3V. Tried changing resistors with other values to get different voltage outputs, but after a couple hours realized that I originally had the wrong value because the resistors were in the wrong spots. 3.3K goes on the bottom and 2.2K goes on the top. While testing, learned that to read the right output, the voltage regulator needs to have a load resistor, otherwise the output is higher (~5V).

## April 9

Designed the third rendition of our PCB to be ordered by the 5th round of PCB ordering this upcoming Tuesday. Planning to make this the official PCB so made the design smaller. Hoping this works since haven't been able to fully test our first PCB yet due to late shipment of our first rendition. Same schematic as second rendition, but overall PCB is smaller to better fit on top of the bike lock. Height reduced from 84mm to 53mm. 

*R8 airwire left intentionally since the strapping pin default value is fine and doesn't need to be changed*

*R1 airwire left intentionally for the same reason*

![image](https://github.com/jclee297/ECE445/assets/168769106/9b9c9bd2-9257-4cf4-9ed2-5689eab0a710)


## April 12

Got the missing components and soldered it together. Also got the microcontroller so soldered the first PCB completely. I misunderstood the circuit that automatically switches the strapping pin/chip enable buttons and thought those were the code data pins. RX and TX are how code actually gets transmitted, and I forgot to provide outputs from the PCB. Had to cut open a micro USB cable and solder wire from the D+ D- pins on the microcontroller to the microUSB.

## April 13

Faced with ESP32-S3-WROOM constantly conencting and disconnecting from the laptop. Lots of debugging. Pinpointed the cause as debouncing capacitors, with a different datasheet showing that the capacitors near the buttons controlling the strapping pin/chip enable pin should not be placed. After removing them, the code was able to be uploaded fine.

## April 15

Soldered the second rendition of our PCB (pcb with more GPIO pins). Voltage regulator worked fine, attached PCB and decided to continuity test to make sure soldering was good. As I was doing so, I forgot I had the battery connected from the voltage regulator testing, and ended up creating magical smoke... 1/3 ESP32s fried.

## April 19

Got the final rendition of our PCB. Soldered everything and tested the voltage regulator. Outputting 5V even with microcontroller connected. Testing was done by tapping the 9V battery to the terminals so hoping the microcontroller isn't fried. Code has been uploaded to our first rendition so this will be our backup in case this microcontroller is fried.

Connected power supply pad on PCB to breadboard and applied output to a load resistor, but still recording 5V. Removed other resistors connecting to the voltage regulator in case the two output pins were connected internally, a possible cause for error.

- Removed R2
- Removed R8

  Voltage goes from battery to screw terminal to voltage regulator, and output goes to 3.3K and 2.2K resistors to set output voltage to 3.3V, but still nothing is working. Going to have to proceed with first rendition.

  Testing the alarm and MOSFET circuit via breadboard, but alarms are going off as soon as battery is connected. Going to scrap this idea since alarms are still pretty loud when powered by the microcontroller from the first rendition

## April 20-22

Confirmed using first rendition for final product. Enclosure boxes were ordered and started to put the PCB inside main enclosure box and ESP32-CAM inside its smaller enclosure box. Drilled a couple holes for the wire to connect through the bike chain with Natasha, hot glued it to the bike chain, and drilled some holes in the ESP32-CAM enclosure box for an LED and button to indicate battery life. Also drilled a hole for the camera to poke through, as well as one for the camera's LED flash.

Design Issue 1: ESP32-CAM needed 5V and resistor ratio that was calculated was not providing proper output (5.5V output but needed it to be within 4.6V-5.25V). 

Solution 1: Had to find old resistors from past lab kits. Used 10k and 3.3K + 380 ohm resistors to achieve 5.19V, acceptable to power the ESP32-CAM.

Design Issue 2: Originally planned to have ESP32-CAM sit on a breadboard inside enclosure box to be more secure and stick wires into breadboard to power it, but corner screwholes in enclosure box impeded the area of the breadboard. Taped ESP32-CAM to the hole on the lid of enclosure box, and will have to power it without a breadboard to connect.

Solution 2: Soldered wires to a surface mount LM317, connected those wires to male-male wire connectors, and had to have the output voltage adjusting resistors float inside the box. Very small box so I shortened the leads on the through-hole resistors and soldered them together, bending them in such a way to take up less space and minimize contact chance with other exposed wires, especially ones from the battery. Needed multiple connections to male-male wire connectors, but was lucky that I could fit a resistor lead and 1 wire inside, which took care of points of multiple contact.

![image](https://github.com/jclee297/ECE445/assets/168769106/2beb3e7e-f7cc-4399-be14-099b0ebe7d7a)

Shoved the wire loop inside the bike chain, hot glued it through the enclosure box for the main PCB, stuck the alarms to the inside of the box with Natasha.
Natasha and Zhuoyuan responsible for the coding, but helped out with testing when things didn't work.

Quick Issues:
- Battery dying mid test or between tests
- ESP32-CAM enclosure box wires falling apart

## April 23

  PCB fully soldered and enclosed, waterproofed with hot glue or foam on the grooves of the enclosure box lids, project fully functional (as long as battery doesn't disconnect/die), any issues at this point are coding issues that were resolved by the end of today and the 24th.
