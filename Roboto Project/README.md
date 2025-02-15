# Roboto Project for NYP robotics CCA
<br>
Repository for all things needed for the robot to work
<br>


## Below are the steps taken to make the robot

<br>

 1. [link to tutorial](https://www.explainingcomputers.com/pi_pico_w_robot.html "explainingcomputer's Pi Pico W WiFi Controlled Robot") this is where to get the base code for the bot

 2. [video showing how to build the bot](https://youtu.be/iTo4Qh2R6m4) <br> Requires: Flathead Screwdriver, Wire Cutters, 3D Printers, Allen Wrench, Solder, Soldering Iron, Female to Female jumper wires (short), 4 AA batteries, pliers<br>

### Youtube tutorial
<br>

[![Raspberry Pi Pico W: WiFi  Controlled Robot](https://i.ytimg.com/vi/iTo4Qh2R6m4/maxresdefault.jpg)](https://www.youtube.com/watch?v=iTo4Qh2R6m4 "Raspberry Pi Pico W: WiFi  Controlled Robot")
<br>

### Wiring Schematic<br>
<br>

![Is it all been worthwhile](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/assets/58255472/11210b64-0726-4693-98d1-ed80c6eb4e47)

 

 3. [link to thonny micropython IDE](https://thonny.org/)<br>

    Thonny is a Python IDE that is beginner-friendly. Click the link above to install.

     ## Conﬁgure Thonny IDE

      1. Open up Thonny IDE software.
      2. Go to ‘Tools’ -> ‘Options’, and under ‘Interpreter’ tab,  select ‘MicroPython (Raspberry Pi Pico)’.
      3. Click ‘OK’ to close.

    ![imaawdaw](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/assets/58255472/af488293-29de-4972-b5bb-a899537d1d10)

 5. [link to micropython pico W](https://micropython.org/download/RPI_PICO_W/) <br> 

    [The RPI_PICO_W-20240105-v1.22.1.uf2 file is the Pico W board's firmware](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/UF2%20bootloader%20(Micropython)/RPI_PICO_W-20240105-v1.22.1.uf2)

    ## Installation instructions <br>
    
      1. To get started on the Raspberry Pi Pico W, you need to connect a micro USB cable to the Pico W board.
      2. Before you plug the other end into the computer,  press and hold the BOOTSEL button.
      3. Continue to hold the  button for a few more  seconds after connecting  to the computer before releasing it.
      4. A new drive (RPI-RP2) will  appear on your computer.
      5. Drag and drop the UF2 ﬁle  onto the RPI-RP2 drive.
      6. The Pico W will auto reboot and run MicroPython.

 6. [link to reset the pico w](https://github.com/dwelch67/raspberrypi-pico/blob/main/flash_nuke.uf2)
    <br>
    hopefully optional
    <br>
 
    [The flash_nuke.uf2 file is used to wipe a "bricked" Pico W board](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/UF2%20bootloader%20(Micropython)/Flash_NUKE%20(unbricks%20the%20Pico%20W).uf2)

    # Read the below carefully <br>
    if you get the following error<br>
    (edit: this error is only fatal after removing the micro USB cable from the Pico W. Otherwise most of the time you can just click through it)

    ![thonny back end error](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/assets/58255472/4050c316-421f-4643-bdb2-88139d6c8c46) <br>

    follow the instructions below
    
    ## De Bricking instructions <br>

      1. Hold down the BOOTSEL button while plugging the board into USB.
      2. When the USB mass storage device window appears copy [the flash_nuke.uf2 file](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/UF2%20bootloader%20(Micropython)/Flash_NUKE%20(unbricks%20the%20Pico%20W).uf2) into the Pico W board
      3. Wait for the window to close then it will open again. <br>
         This time copy [the (Latest Version) RPI_PICO_W-20240222-v1.22.2.uf2 file which is the Pico W board's firmware](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/UF2%20bootloader%20(Micropython)/(Latest%20Version)%20RPI_PICO_W-20240222-v1.22.2.uf2) into the Pico W board.
      4. The Pico W board should now work again. (hopefully)


<br>

## The final code used in the video can be downloaded or copy pasted from below as follows: <br>
### 1. [Web Control](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/Working%20Python%20Test%20files/Web%20Control.py)

```
import network
import socket
from time import sleep
import machine

# Yes, these could be in another file. But on the Pico! So no more secure. :)
ssid = 'Your_Network_Name'
password = 'Your_WiFi_Password'

def move_forward():
    print ("Forward")
    
def move_backward():
    print ("Backward")
    
def move_stop():
    print ("Stop")
    
def move_left():
    print ("Left")
    
def move_right():
    print ("Right")
    
def connect():
    #Connect to WLAN
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, password)
    while wlan.isconnected() == False:
        print('Waiting for connection...')
        sleep(1)
    ip = wlan.ifconfig()[0]
    print(f'Connected on {ip}')
    return ip
    
def open_socket(ip):
    # Open a socket
    address = (ip, 80)
    connection = socket.socket()
    connection.bind(address)
    connection.listen(1)
    return connection

def webpage():
    #Template HTML
    html = f"""
            <!DOCTYPE html>
            <html>
            <head>
            <title>Zumo Robot Control</title>
            </head>
            <center><b>
            <form action="./forward">
            <input type="submit" value="Forward" style="height:120px; width:120px" />
            </form>
            <table><tr>
            <td><form action="./left">
            <input type="submit" value="Left" style="height:120px; width:120px" />
            </form></td>
            <td><form action="./stop">
            <input type="submit" value="Stop" style="height:120px; width:120px" />
            </form></td>
            <td><form action="./right">
            <input type="submit" value="Right" style="height:120px; width:120px" />
            </form></td>
            </tr></table>
            <form action="./back">
            <input type="submit" value="Back" style="height:120px; width:120px" />
            </form>
            <br>
            <table><tr>
            <form action="./High">
            <input type="submit" value="High" style="height:120px; width:120px" />
            </form></td>
            <form action="./Medium">
            <input type="submit" value="Medium" style="height:120px; width:120px" />
            </form></td>
            <form action="./Low">
            <input type="submit" value="Low" style="height:120px; width:120px" />
            </form></td>
            </body>
            </html>
            """
    return str(html)

def serve(connection):
    #Start web server
    while True:
        client = connection.accept()[0]
        request = client.recv(1024)
        request = str(request)
        try:
            request = request.split()[1]
        except IndexError:
            pass
        if request == '/forward?':
            move_forward()
        elif request =='/left?':
            move_left()
        elif request =='/stop?':
            move_stop()
        elif request =='/right?':
            move_right()
        elif request =='/back?':
            move_backward()
        elif request =='/High?':
            EN_A.duty_u16(65535)
            EN_B.duty_u16(65535)
        elif request =='/Medium?':
            EN_A.duty_u16(44350)
            EN_B.duty_u16(44350)
        elif request =='/Low?':
            EN_A.duty_u16(39321)
            EN_B.duty_u16(39321)
        html = webpage()
        client.send(html)
        client.close()

try:
    ip = connect()
    connection = open_socket(ip)
    serve(connection)
except KeyboardInterrupt:
    machine.reset()
```
### 2. [Motor Test](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/Working%20Python%20Test%20files/Motor%20Test.py)
```
from time import sleep
from machine import PWM, Pin

Mot_A_Forward = Pin(18, Pin.OUT)
Mot_A_Back = Pin(19, Pin.OUT)
Mot_B_Forward = Pin(20, Pin.OUT)
Mot_B_Back = Pin(21, Pin.OUT)

# Set PWM
EN_A = PWM(Pin(8))
EN_B = PWM(Pin(2))
# Defining frequency for enable pins
EN_A.freq(1500)
EN_B.freq(1500)
# Setting maximum duty cycle for maximum speed (0 to 65025)
EN_A.duty_u16(65025)
EN_B.duty_u16(65025)

def move_forward():
    Mot_A_Forward.value(1)
    Mot_B_Forward.value(1)
    Mot_A_Back.value(0)
    Mot_B_Back.value(0)
    
def move_backward():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(1)
    Mot_B_Back.value(1)

def move_stop():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(0)
    Mot_B_Back.value(0)

def move_left():
    Mot_A_Forward.value(1)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(0)
    Mot_B_Back.value(1)

def move_right():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(1)
    Mot_A_Back.value(1)
    Mot_B_Back.value(0)

move_stop()
sleep(2)

print ("Forward test")

move_forward()
sleep(2)

print ("Backward test")

move_backward()
sleep(2)

print ("Spin left test")

move_left()
sleep(2)

print ("Spin right test")

move_right()
sleep(2)

print ("'Time for bed' said Zeberdee.")

move_stop()
```
### 3. [Final Code](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/Stuff%20to%20Upload%20into%20Pico%20W/Main.py)
```
import network
import socket
from time import sleep
import machine
from machine import Pin,PWM #importing PIN and PWM

# Yes, these could be in another file. But on the Pico! So no more secure. :)
ssid = 'Your_Network_Name'
password = 'Your_WiFi_Password'

# Define pins to pin motors!
Mot_A_Forward = Pin(18, Pin.OUT)
Mot_A_Back = Pin(19, Pin.OUT)
Mot_B_Forward = Pin(20, Pin.OUT)
Mot_B_Back = Pin(21, Pin.OUT)

# Set PWM
EN_A = PWM(Pin(8))
EN_B = PWM(Pin(2))
# Defining frequency for enable pins
EN_A.freq(2000)
EN_B.freq(2000)
# Setting maximum duty cycle for maximum speed (0 to 65025)
EN_A.duty_u16(65535)
EN_B.duty_u16(65535)

def move_forward():
    Mot_A_Forward.value(1)
    Mot_B_Forward.value(1)
    Mot_A_Back.value(0)
    Mot_B_Back.value(0)
    
def move_backward():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(1)
    Mot_B_Back.value(1)

def move_stop():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(0)
    Mot_B_Back.value(0)

def move_left():
    Mot_A_Forward.value(1)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(0)
    Mot_B_Back.value(1)

def move_right():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(1)
    Mot_A_Back.value(1)
    Mot_B_Back.value(0)

#Stop the robot as soon as possible
move_stop()
    
def connect():
    #Connect to WLAN
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, password)
    while wlan.isconnected() == False:
        print('Waiting for connection...')
        sleep(1)
    ip = wlan.ifconfig()[0]
    print(f'Connected on {ip}')
    return ip
    
def open_socket(ip):
    # Open a socket
    address = (ip, 80)
    connection = socket.socket()
    connection.bind(address)
    connection.listen(1)
    return connection

def webpage():
    #Template HTML
    html = f"""
            <!DOCTYPE html>
            <html>
            <head>
            <title>Zumo Robot Control</title>
            </head>
            <center><b>
            <form action="./forward">
            <input type="submit" value="Forward" style="height:120px; width:120px" />
            </form>
            <table><tr>
            <td><form action="./left">
            <input type="submit" value="Left" style="height:120px; width:120px" />
            </form></td>
            <td><form action="./stop">
            <input type="submit" value="Stop" style="height:120px; width:120px" />
            </form></td>
            <td><form action="./right">
            <input type="submit" value="Right" style="height:120px; width:120px" />
            </form></td>
            </tr></table>
            <form action="./back">
            <input type="submit" value="Back" style="height:120px; width:120px" />
            </form>
            <br>
            <table><tr>
            <form action="./High">
            <input type="submit" value="High" style="height:120px; width:120px" />
            </form></td>
            <form action="./Medium">
            <input type="submit" value="Medium" style="height:120px; width:120px" />
            </form></td>
            <form action="./Low">
            <input type="submit" value="Low" style="height:120px; width:120px" />
            </form></td>
            </body>
            </html>
            """
    return str(html)

def serve(connection):
    #Start web server
    while True:
        client = connection.accept()[0]
        request = client.recv(1024)
        request = str(request)
        try:
            request = request.split()[1]
        except IndexError:
            pass
        if request == '/forward?':
            move_forward()
        elif request =='/left?':
            move_left()
        elif request =='/stop?':
            move_stop()
        elif request =='/right?':
            move_right()
        elif request =='/back?':
            move_backward()
        elif request =='/High?':
            EN_A.duty_u16(65535)
            EN_B.duty_u16(65535)
        elif request =='/Medium?':
            EN_A.duty_u16(44350)
            EN_B.duty_u16(44350)
        elif request =='/Low?':
            EN_A.duty_u16(39321)
            EN_B.duty_u16(39321)
        html = webpage()
        client.send(html)
        client.close()

try:
    ip = connect()
    connection = open_socket(ip)
    serve(connection)
except KeyboardInterrupt:
    machine.reset()
```
<br>
Be sure to ask the Cher for the password to the wifi <br> (only works locally)<br>
<br>
this might be out of date ...
  
  
  <br>
  
  - ~~ssid = 'DWR-921-F8C5'~~
  
  - ~~password = 'eTvwcPX5'~~

  <br>

#### Credit to Christopher Barnatt for providing all the Python code above
<br>

# installation
<br>

 1. `git clone` or download the repo.
<br>

 2. then open the Thonny IDE, and connect your Pico W to your PC. 
<br>

 3. Drag [lib and Main.py](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/tree/Master-Repo/Stuff%20to%20Upload%20into%20Pico%20W) into the Pico W.

# Once you have done all of the above ...
<br>
you should now have a working robot that looks like this...
<br>

https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/assets/58255472/66910106-0d2b-46a2-97a9-151321dd1794

credit to me for the video


# Extra Code

## [IR sensor code](https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/blob/Master-Repo/Working%20Python%20Test%20files/IR%20sensor.py)
```
from time import sleep
from machine import Pin
import random

Mot_A_Forward = Pin(18, Pin.OUT)
Mot_A_Back = Pin(19, Pin.OUT)
Mot_B_Forward = Pin(20, Pin.OUT)
Mot_B_Back = Pin(21, Pin.OUT)
sensor = Pin(3, Pin.IN)

def move_forward():
    Mot_A_Forward.value(1)
    Mot_B_Forward.value(1)
    Mot_A_Back.value(0)
    Mot_B_Back.value(0)
    
def move_backward():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(1)
    Mot_B_Back.value(1)

def move_stop():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(0)
    Mot_B_Back.value(0)

def move_left():
    Mot_A_Forward.value(1)
    Mot_B_Forward.value(0)
    Mot_A_Back.value(0)
    Mot_B_Back.value(1)

def move_right():
    Mot_A_Forward.value(0)
    Mot_B_Forward.value(1)
    Mot_A_Back.value(1)
    Mot_B_Back.value(0)

movement_functions = [move_backward, move_left, move_right]
movement_function = [move_forward, move_left, move_right]
x = random.choice(movement_functions)
y = random.choice(movement_function)

while True:
    if sensor.value() == 1:
        print("1")
        y = random.choice(movement_function)
        y()
        sleep(1)
        move_stop()
    if sensor.value() == 0:
        print("0")
        x = random.choice(movement_functions)
        x()
        sleep(1)
        move_stop()
```
Python code above done by me and Roy.

# After some modding and adding new code...
<br>
you should have a working robot that looks like this...
<br>


https://github.com/zacw-243L/Roboto-Project-for-NYP-robotics-CCA/assets/58255472/8891ed82-aafc-4d91-a408-55f4f8501dbf

a working romba that moves in random directions<br>

Credit to me for the video

<br>

Footnote: the project is still being worked on don't look under experimental features...

For more info about the Pico W click here[^1] <br>
For more info about How to make a Pico W HTTP server click here[^2]

[^1]: [Pico W documentation. they did this better than me](https://github.com/raspberrypi/documentation/blob/develop/documentation/asciidoc/microcontrollers/raspberry-pi-pico/about_pico.adoc)
[^2]: [Pico W's connectivity](https://datasheets.raspberrypi.com/picow/connecting-to-the-internet-with-pico-w.pdf)


