# NRF24L01-Tutorial

NRF24L01 Tutorial - Arduino Wireless Communication
--------------------------------------------------
[![LhBIRMMAGGE](https://img.youtube.com/vi/LhBIRMMAGGE/0.jpg)](https://www.youtube.com/watch?v=LhBIRMMAGGE)

In my last tutorial I created a NodeMCU based Duino Coin Miner. It is an awesome little miner that sits on my desk and mines few cents a day. 
However, adding these miners to my home network choked my WiFi router. Home Appliances and Smart Devices connected to the router constantly started dropping off. To my understanding, most of the wireless routers and access points can support upto 250 devices connected at once. So, what's happening here? 
To clarify my doubts I called my ISP. The answer they gave was absolutely shocking. "ONLY 30 devices can successfully connect and exchange data via their router at any given time". Bloody hell!! 
So, to overcome this limitation I added another router to the network to scale up the load.

But, I was not happy with this solution. So, I did a bit of research and found this "NRF24L01 RF Transceiver Module" which I can use to create a mesh of wirelessly connected microcontrollers. 
In this tutorial, I am going to show you guys how to use this transceiver module to add wireless communication between two or more Arduino boards. I will be using this module for many of my upcoming home automation projects. Bang Problem solved..


Video: https://www.youtube.com/watch?v=LhBIRMMAGGE

What is a NRF24L01 RF Transceiver Module?
-----------------------------------------
![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_EPMHS1s8Y-GPP85TM6D4c82dF4imiVh_NsXjuCSqDjPZ-ulaubH5tqVaZiPJqjDMJ4K-JKvx1mdrMZqoKcVSxOg37nQ4nUycXWqUlKvIM3j0V0hJXN-eaAzCYFJltbNcQVNdGtraXxYjWge1HAkd_PzpRe_HoGco4MNru7wKeCP2CILweOoJpEjR/w640-h360/03.png)

So far, I have always used WiFi for wireless communication between microcontrollers. While this is easy enough to do, it is not exactly suitable for battery operated nodes. WiFi modules consume a lot of current when transmitting data plus they also have a slight delay when initiating the transmission as the module has to first connect to the WiFi network.

After getting crippled by the abilities of my wireless router, I found this cheap, very popular and widely used "RF Transceiver Module" which you can hook up to any microcontroller (MCU). This module is called a RF transceiver because a single module can work both as a transmitter and a receiver.

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiekvEGIeZl9Au5GHLcMee6UNT8fZmR1Zrk_DAoeqbz6_Qt4h7vtOoA05osYQGKxLejWwvK4WsN4ilONWOqf67xyt8OY4r7BCjAX2_TvSrX-ZUBpw48hvPfFzgja1UM9d7QoBotpIZVDzIXacGMlj3VHJAfybdMqddAc3rmE3E5UxeX6FU_2ucpGkcu/w640-h360/04.png)

 - The module operates at a frequency of 2.4GHz, which is one of the ISM band which means it is open to use in most of the countries around the World.
 - Data transfer rate is between 250kbps to 2Mbps baud.
 - Power consumption? This module is designed for ultra low power wireless applications. It has 2 power saving modes operating at 22uA Standby-I mode and 900nA in power down mode - which makes these modules suitable for battery operated nodes. The high air data rate combined with two power saving modes makes the nRF24L01 module very suitable for ultra low power designs. The power consumption of this module is just around 12 milliamps during transmission (TX) which is even lower than a single led.	   
 - Operating voltage is between 1.9V to 3.6V. All other pins on this board are 5V tolerant making it easy to connect to an Arduino without using a logic level converter. It has an integrated (on chip) voltage regulator.
 - The Range of this module as per its datasheet is 100m but it works up to 50 to 60 meters in real world conditions.
 - The module has 125 independent RF channels giving the possibility to have a network of "125 independently working modems" in one place. Each channel can have up to "6 addresses or 6 data pipes" or in other words, each unit can communicate with up to 6 other units at the same time (1:6 star networks).
 - The module is configured and operated through a Serial Peripheral Interface (SPI).
 - It uses Enhanced ShockBurst™ for automatic packet assembly and timing, automatic acknowledgement and retransmission of packets. Enhanced ShockBurst™ enables the implementation of ultra low power, high performance communication with low cost host microcontrollers. The features enable significant improvements of power efficiency for bi-directional and uni-directional systems, without adding complexity on the host controller side.

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi-oSLNQUSG61wSQoXNUJ7z1Vi9ei3cyZbPFL3ukFgpXtqJ0kcYYFLCvCPaEuJMMdDyQHQvcLNSoJ8-wFqdc6_HtM5y1xHO10YAsNwpGB8OFcbEjWNr2afu9N_GweUrYrmJsK59MrT6PBByhs8QAzDPOzpfPEfDjSNL_a6c4d8Z2kPXZq81ot0cqUpX/w640-h360/05.png)

The module used in this video has an in-built PCB antenna making it compact. However, you can also buy a variant that supports an external antenna allowing much higher range of about 1000M in line of sight.


nRF24L01 Pinout
---------------
![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgnmDTk-rwowWSUzyURtNRHvGe5FnH27XC9OJ8i2o3u1QTU-Hw0256IMulP_O0cif6PekjQnql84xvnT5gaJe5w3nvL4mCtrEm44B_fAXh-0TRuciYEY-u-HbAq8ePYtWXDTriFJ20Pfx2MsfTo1MB1SVDFGcdka9LTr0p4bLTEVsiBpuhV0GFahG4y/w640-h360/06.png)

Now, lets have a look at the pinouts and specifications of the NRF24L01 module:
 * GND: is the Ground Pin. It is placed inside a square for easy identification.
 * VCC: supplies power to the module. Voltage can range from 1.9v to 3.9v. So, you can connect it directly to the 3.3V pin of our Arduino. Remember connecting it to a 5V pin will likely destroy your nRF24L01+ module!
 * CE: (Chip Enable) is an active-HIGH pin. When selected the module will either transmit or receive, depending upon which mode it is currently in.
 * CSN: (Chip Select Not) is an active-LOW pin and is normally kept HIGH. When this pin goes low, the module begins listening on its SPI port for data and processes it accordingly.
 * SCK: (Serial Clock) it accepts clock pulses provided by the SPI bus Master.
 * MOSI: (Master Out Slave In) It is SPI input to the module. It is used to receive data from the microcontroller.
 * MISO: (Master In Slave Out) It is SPI output from the module. It is used to send data to the microcontroller.
 * IRQ: It is the interrupt pin that alerts the master when new data is available to process.
	

Setup and Schematic
-------------------
![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIBT7bFYyKHa2ogWmjoYPy_GI2NmNOebkCgtWwrFfXlAVPJrV-FK2lsTAgrPA9RGLYwTey1dsyfq9PedIReK8JAGqHhyOmcZP4dIsBfMgifDGEnFdL2lRTZlYE9U8QuMHJ0sbUQ1Bwr67pJYiMHbGE6O7nZe23ThhAmsWxepCq1Cw6Py4xcQUsoSgr/w640-h360/06a.png)

In order to get this working, we need two such NRF24L01 Modules and two Arduino Boards. For this tutorial I am going to use 2 Arduino Nanos.

Just remember, we cannot use a breadboard with these modules because the pin spacing on these modules are not enough to place it in the middle and if you place it anywhere else, then you will end up shorting the pins. This means that you will either have to solder the wires directly to the modules or use some sort of jumper cables.

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj9lPgF7JPCbz2ZaIPGAK1ZkMCNhNwaDhL8QmVhJ0cZEk4HnXbrYBiZ-UBo8NHo2I92BBR8B_r5hLH-hu3e-AMSoa4anQ1bDEfEU3ryJ897FqmPacfdzn8kqBGbwbil-RVTkedyGWiByBsC3HF-YcOk7BEu5PTCI_CT-fep6ndyyienfg0iwyPuddQi/w640-h360/07.png)

The connection is exactly the same on both the transmitter and receiver end. 
Connect the GND pin to -ve and VCC pin to 3.3v pin of Arduino. The signals generated by these modules are very sensitive to power supply noises. So, adding a decoupling capacitor (anything from 10uF to 100uF) across the power supply line is always a very good idea.
Then connect the CSN pin to D8, CE to D9, MOSI to D11, MISO to D12, and SCK to D13 pin of the Arduino.

Since the nRF24L01+ module requires a lot of data transfer, it will give the best performance when connected to the hardware SPI pins on the microcontroller. Note that each Arduino board has different SPI pins that must be connected accordingly. Have a look at the table onscreen for quick understanding.


Library Used
------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRAvsouUuJTk2UUw0_LhB9d3dHgvBnHLkYko1uk0Jdc6qQgG-ZqTV8vVXf_rMco6JJDLywAc542ejtYex11JZo9QrEtCOW121TalU34zDI78yeJJV3ZjvgxCYxLEtcQ2XATYs2pw8MU3eroiJEWswTvvPMJis4Y7m4aIH6_aiL6cbOT3MHO6p2jgDc/w640-h360/08.png)

For this tutorial I am going to use the "TMRh20/RF24" OSI Layer-2 driver for nRF24L01 on Arduino & Raspberry Pi/Linux Devices. You can download the library from the link provided in the description below: https://github.com/tmrh20/RF24/.


Code 1 - Sending Text
---------------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhTvc2-YantPttj51nj04XbjrFRKIdzq1Vd1SS3ML66tzddNvUBrygvEf9BcnttaRVA8JEi6h6_XyZ9jkMxanO1MfDHtW6yolBSXc9kN8c_y0NpI1sbkLy10Qf86BHx0131a4lr60-_IvmODntCdqn6OY89q2bwz5Ln8eadhuhKfQsH5h44cnh_hvbZ/w640-h360/08a.png)

In my first example, I am going to send a character array from one module to the other. Using a split screen I am going to demonstrate this example. On my left is the Transmitter Code and on my right is the Receiver Code.

Lets start by including the "SPI library" followed by the "RF modules library" downloaded from github in the code.
Then we are creating a RF24 object by passing the CSN and CE as the two arguments to the radio() function.

Next, we are creating an array of the addresses that the modules will use to communicate amongst themselves. The address can literally be anything, however, it has to be the "same" on both the transmitter and the receiver modules. This is how the RF modules know who they have to communicate with.

In the setup section we first initialize the radio object.
Then, using the radio.openWritingPipe() function we set the address of the transmitter which we will use to send data to the receiver module. On the receiving end we use the radio.openReadingPipe() function with the same address to read the data from the data pipe.
Next we set the power amplifier level. Since the modules in this demo are sitting next to each other, I am using the "Minimum Level". 

Next, in the transmitter code we need to tell the module to stop listening using the radio.stopListening() function. This sets the module as a transmitter. On the receiver module we need to start listening using the radio.startListening() function. This sets the module as a receiver. 

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxzBieqpPHFMuwaljdTxAuSZnvn12lyfumuHSSlmpu2rQyWBgYU9N-rtf9AdBKhyVgvRPHoI5fzC0NEyhicXga7CoJGoTJRqThESN_qrweGuZGMpDU8C5ou2EfAhgyfKIkKlsKqY4SLRMGlyNy2I_Ep0gZkX7U2NZUxlJs7s-0DzcPBzJJo28lM6F6/w640-h360/09.png)

After that, in the loop() section of the transmitter, we send an array of characters using the radio.write() function and on the receiver end we read the array using the radio.read() function and display it on the serial monitor every second.


Code 2 - Lighting Up LEDs
-------------------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEghUrA2YwnT3-aXpG0cnX1QENu7k4sLLvOe5uRJWxPbprT-Kn8KMIUImmgHJyZ_0uC5cPn7FADOoCsxN9mW944ttyyMlA2FTWi6mgCpAvciAevVkrIoiJnSFfT-bu5ti_KArgGPgKHtrhSL8tVVAhzone59qf4DpQrsjh2fj9kNSlUz08dFJ5tMgDc3/w640-h360/10.png)

In the second example, I am going to light up two LEDs on the receiver-end based on whichever button is pressed on the transmitter-end.

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjxDxaV3cx4PgxShUeeliZufAyPfsb-VMIf8Xcpbe9GJR2uH0c2fOOwVESsDr03KFWCDsgzbLG3I8v7L9DZGOjIW3FNepi0ce7TEB9MS8zEejFuDT337BXQ5KzNIifilKDhxEFb1QYpe5rn_00eCYnQB630KlOKWxLxFWPmNHbc0Qf3s5NgAD_uo9c2/w640-h360/09a.png)

To achieve this I have added 2 LEDs on the receiver end and 2 Push Button switches on the transmitter end.

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh9AZG8HwKNwYPskmH3LuIYDwQ6Jg1fyjkIUvlBKa7Whw9G9o9r_58AfQfperMBbJEWxTadxnPfrOYbEsDHSf94pUek_TF9bMHQ9H1Ev4la1Slnor50vm__8dIvoQhxzHdQNts_Syl2ItnOORAaJh6Th6DHi2ZtB0QEnast7yZEULGQ9X2xsqeCg8Xq/w640-h360/11.png)

When Button B1 is pressed the transmitter sends "B1" using the radio.write() function and when Button B2 is pressed the transmitter sends "B2" using the radio.write() function to the receiver module.
The "switch statement" in the receiver code then lights up the corresponding LED based on whichever button was pressed on the transmitter end.


Code 3 - Same Node Acting as TX and RX [Bidirectional Communication)
------------------------------------------------------------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg8Xz-DeJN6jn1Dw1D93myq9iQmsFTW4yj3CGiNRoW2MjOi1UYOA8XTFn7cLlHQfGv-8etiCTFJhgvQMh8mHOZYRu_ofJlxfFi1qgifkv9IEUfQEjEjl-CQ2xOzoHD4rchj7-zZFfAyKGcaAKUXWXGAhQrgmtGExcOb011TqSJCEqhks8XzMIySc6Kg/w640-h360/11a.png)

In my 3rd example I will show you guys how a single node can act as both transmitter and receiver. Just remember you "cannot" send and receive data at the same time. Using the "stopListening()" and "startListening()" functions we will toggle between sending and receiving of data on the data pipes.

What's different here from the previous code is that we are creating two pipes or addresses for the bi-directional communication.
const byte addresses[][10] = {"ADDRESS01", "ADDRESS02"};

In the setup section we need to define both pipes in a way that the sending address of the 1st module is the receiving address of the 2nd module and vice versa the receiving address of the 1st module needs to be the sending address of the 2nd module.

Now in the loop section of the 1st Arduino, we use the radio.stopListening() function to turn the node into a transmitter and send the data using the radio.write() function. 
On the receiver end we use the radio.startListening() function to read the incoming data. While there is incoming data (radio.available()) we read it using the radio.read() function. Then we add a bit of delay to the code.

After that, we set the 1st Arduino to receiving mode using the radio.startListening() function and the 2nd Arduino to transmitting mode using the radio.stopListening() function.

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhdFxYhxZdj6WW01n8HMXRyTf9MzNZyC8XRWVsGldod8OA_6BSE9KsSCM2NR6b6uj8BFq1Vl3yT09EIxqeRLYMgfYxE60DkQUGLqsvBYG8y6Ms9az-x54y1Gsn0Z0hdyxqmUdyok_5wikDaU1lbkxyeForZHCs8XJL3tO-fMz5hRamikL7IYyQhchmb/w640-h360/12.png)

The data is then displayed on screen using the Serial Monitor.


Code 4 - Multiple Nodes [Mesh - Multiceiver Network]
--------------------------------------------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhWEjBRJ6GcHC5KBRPsKWzWu1sj9sC5wUqp7B_F5q86QO-_vWQTivmOYgd69mlIHVYs5D4TQtbjDH7eWBPtpEnLwLzAFwhirfLls_PA0sEAwPJ0Y6JJ7h-zblKAzoLmLkngCVPlrIzJ6V45zBK8s47WRWLbdQIyex1pL5wVr3HajiAgfMrlbuBTwG3L/w640-h360/13.png)

The nRF24L01+ has a feature called Multiceiver. It is an abbreviation for Multiple Transmitter Single Receiver. In my 4th example, I am going to show you guys how to connect multiple transmitters to a single receiver.

In a Multiceiver network each RF channel is logically divided into 6 parallel data channels or the data pipes. Each data pipe has its own unique data pipe address. Only one data pipe can receive a packet at a time.
So basically, the primary receiver in the middle is collecting data from 6 different transmitting nodes simultaneously. The primary receiver can stop listening any time and start acting like a transmitter. 

This way you can create a mesh of network where each node can act as a repeater. There is a different library "RF24Mesh" you need to use to create this mesh network. Since I don't have that many modules handy at this moment, I am unable to show you guys the working bit of it. However, I will create a 2nd tutorial dedicated just to the mesh network, so stay tuned.


Using Sleep Mode 
----------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5XcgnBJGZ9Q_MLfdq5ZgKIQpxlorgN6VXZjqza5CqI1e_73tCXk_vvdMckf3MKcIWbIeQwoNDda2gictYYdX-KEchU9ekzgp3e7F1gCiTT_JFO0VkoLYJEi1lt58cAUL-eZksPY6uoADbvK9b4-A7SnCp2ylTg22DPsozCuUERm--J1UyyhCxXxB8/w640-h360/14.png)

There is a way to conserve battery by sending the module to sleep mode. Please read through the "pingpair_sleepy" example for more details, I have provided the link in the description below.


Factor Effecting The Transmission
---------------------------------

![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiHiym0bqiuWhhTUaeSEiY9_dPVB-meh-_Ua6BxPqVkjxB9ik7rKiuab-LM7IvYKqXmudkpPvgVujSMSudGt-QVwZ73u8Ux4vJJBnFFWTAQ-kYkSpLcey8tSh0Y4bw76s-c8qJ3KCcgfskOoWF1nlei0ZfgVzvfL6u9mc2IWJJDecEPPv85llFJgvoe/w640-h360/15.png)

 * These modules work very well when the transmitter and receiver are close to each other. If the distance is too big you may lose the communication. 
 * The signals generated by these modules are very sensitive to power supply noises. Depending upon the amount of noise, the communication rate may vary. 
 * Setting the maximum output power can also improve the communication range.
 * If there’s an obstacle in the line of sight you may see multiple dropouts.
 * Reducing the data rate can significantly improve the performance. A speed of 250kbps is more than enough for most of our projects.
 * Using an external antenna can also significantly improve the transmission rate.
 * Another potential source of noise for RF circuits is WiFi. Especially when someone's network is set on the same channel. Since WiFi mostly uses the lower frequency channels, it is recommended to use the highest 25 channels for your nRF24L01+ modules.


Thanks
------
[![LhBIRMMAGGE](https://img.youtube.com/vi/LhBIRMMAGGE/0.jpg)](https://www.youtube.com/watch?v=LhBIRMMAGGE)
Thanks again for checking my post. I hope it helps you.

If you want to support me subscribe to my YouTube Channel: https://www.youtube.com/user/tarantula3

Blog Posts: https://diyfactory007.blogspot.com/2022/06/NRF24L01-Tutorial.html

Video references: https://youtu.be/LhBIRMMAGGE


DataSheet

https://drive.google.com/file/d/14Wi4DDhG1i5OAB7Sn-hKQhFbWdwkxufF/view?usp=sharing


Schema

Schema - Sending Text: https://drive.google.com/file/d/1bgZ_vP8lZvUyOU_WFzvVthM94oTR91TB/view?usp=sharing

Schema - Lighting Up LEDs: https://drive.google.com/file/d/1bvgOq9IdSfqE5bjyXLU7suMYgC2rDJcn/view?usp=sharing


Code

Code 1 - Sending Text: https://drive.google.com/file/d/115yCQJQf_0v0qZ8TZHyMrcYMvTRNw5-S/view?usp=sharing

Code 2 - Lighting Up LEDs: https://drive.google.com/file/d/14J_BLk9AgCjLmDTDMoOzYiDdQvrzddpx/view?usp=sharing

Code 3 - Bidirectional Communication: https://drive.google.com/file/d/1pSlzdFBgvlCPzXbHxlj9DEb6WwEMrn4f/view?usp=sharing


Libraries Used:

TMRh20/RF24     : https://github.com/tmrh20/RF24/ 

TMRh20/RF24     : https://drive.google.com/file/d/1QI2ysVEmlbmg7iBVQATmC1_LcBTWnNBn/view?usp=sharing

RF24Mesh        : https://nrf24.github.io/RF24Mesh/

pingpair_sleepy : https://maniacbug.github.io/RF24/pingpair_sleepy_8pde-example.html



Support My Work:

BTC:  1M1PdxVxSTPLoMK91XnvEPksVuAa4J4dDp

LTC:  MQFkVkWimYngMwp5SMuSbMP4ADStjysstm

DOGE: DDe7Fws24zf7acZevoT8uERnmisiHwR5st

ETH:  0x939aa4e13ecb4b46663c8017986abc0d204cde60

BAT:  0x939aa4e13ecb4b46663c8017986abc0d204cde60

LBC:  bZ8ANEJFsd2MNFfpoxBhtFNPboh7PmD7M2

Thanks, ca again in my next tutorial.

