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
