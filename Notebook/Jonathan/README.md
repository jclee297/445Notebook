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
2. What battery should we use?
9V for now
3. What's powering the ESP32-CAM
Likely also a 9V battery stepped down to 5V via LM317
4. How much power does the ESP32-CAM need?
5V 2A according to datasheet
5. Allowable voltage for alarm speakers?
Up to 24V
6. How does ESP32-CAM connect to the actual camera?
Actual camera is OV2640, connection is built onto ESP32-CAM. Datasheet calls for 5V and 180mA
[datasheet](https://www.handsontec.com/dataspecs/module/ESP32-CAM.pdf)
