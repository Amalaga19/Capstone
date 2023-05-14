# Bridging the Digital Divide: Open-Source Access Point

## The Project

  This is a Wi-Fi access point that can be assembled and activated using open-source components and software. It runs on a Raspberry Pi 3B+ and is housed inside a 3-D printed casing, of which the design was modified to suit the functions of this device, and can be powered by a portable battery or any power source that can reliably provide a current of at least 2 Amperes. It also uses an SSD 1306 OLED display that can be programmed to display different kinds of messages and information from the device, and a USB Wi-Fi dongle in place of the built-in antenna. This device is connected to a network through an Ethernet cable and emits Wi-Fi through either its built-in antenna or the plugged-in dongle. Once this is assembled, further improvements can be made, such as adding more devices as repeaters or implementing a mesh network.

## Why I am doing this

  I am from Lima, Peru’s capital and largest city. Although in Lima internet connection and access is available and fairly priced, it is not the case in rural and remote areas like the highlands or rainforest regions. In these places, internet service providers (ISPs) have failed to provide a reliable and inexpensive connection, widening the digital divide. The digital divide is the unequal access to technology and connectivity between demographics or different groups of people, which in the case of Peru is evident and exacerbated by the unreliable internet infrastructure in remote areas. Since the “traditional” ISPs’ methods to bring internet connectivity have failed to provide a reliable and cost-effective service, other solutions are needed. In other countries in the Global South such as Thailand and South Africa community-led initiatives have been carried out to provide connectivity to their own communities. These initiatives are cost-effective, easy to install and maintain, sustainable, scalable and replicable, and while they often respond to a deficient internet infrastructure, the currently existent (deficient) infrastructure is sometimes leveraged as part of the initiative. In the case of Peru, the Ministry of Transport and Communications launched the “Conecta Selva” (Connect the Rainforest) program in 2021 with the aim of installing satellite internet connectivity in schools and hospitals in rural communities in the Peruvian Rainforest.

## Cost-Effectiveness and Open Source: Why Raspberry Pi?

  Even though I wanted this device to be cost-effective, it also needed to be replicable and easy to install and maintain. To do this, it was clear that the project needed to use as much open-source technology as possible. This meant that using a Raspberry Pi, although much more expensive than a conventional wi-fi router or modem, was the logical choice due to the fact that it is mass-produced and can run a standard operating system (in this case Raspbian Bullseye). Raspberry Pi runs on Linux, an open-source operating system which also supports OpenWRT firmware, which has been used to program the embedded systems of several router and modem models. As an open-source firmware, the brands that use it need to publish them under a GPL license, making them available for programmers who would like to program their routers to perform different tasks. However, the firmware varies for each model, making it impossible to create a single manual and solution were this route chosen. Hence, a Raspberry Pi provided the perfect open-source alternative with very similar functionality, albeit sacrificing cost-effectiveness for the sake of replicability and ease to install and maintain, as Raspberry Pi has been extensively documented and has a large and active community of users, something individual routers may not have. This community of users will hopefully take this project and improve it or add to it, in hope that it achieves

# Instructions

## Equipement needed
![A picture of the equipement laid out on a table](https://github.com/Amalaga19/Capstone/blob/main/IMG_5542.JPEG?raw=true "Equipement")

- A Raspberry Pi board (3B+ or newer)
- An SSD 1306 OLED display like [this one](https://www.amazon.com/dp/B085WCRS7C?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- Female-to-female jumper cables
- A MicroUSB cable and a power source capable of providing at least 2 Amps
- A USB Wi-fi dongle (optional)
- An Ethernet cable
- An HDMI cable, a USB keyboard and a USB mouse. (Optional)
- A 3-D printer and PLA filament (or access to a 3-D printing service)

## Setting up the Raspberry Pi
1. Download and install the [Raspberry Pi imager](https://www.raspberrypi.com/software/)
2. Insert a MicroSD card into a USB adapter and connect it to your computer.
3. Select “Raspberry Pi OS (32-Bit)” after clicking the “Choose OS” button and select the MicroSD after clicking the “Choose Storage” button.
4. Click on the settings button (it looks like a cog). Check the “Set Hostname” box and change the hostname, which is like the name of the device, and check “Enable SSH”, keep the “use password authentication” option checked and change the username and password. Take note of the hostname, username and password, and click the “Save” button.
5. Click the “Write” button. Do not remove the MicroSD card from the computer until the imager shows a pop-up message that it is safe to do so.
6. Insert the MicroSD card into the bottom of the Raspberry Pi board, connect an ethernet cable to its ethernet port and connect it to power using its MicroUSB port.
7. Open a command prompt in your computer and make sure it is connected to the same network as the Raspberry Pi. I used Windows Powershell. Enter the following command and press enter, replacing the hostname and username placeholders with the ones you set when installing the OS to the MicroSD card:
```bash
ssh user@hostname.local 
```
Enter your password when prompted. in Windows Powershell there will be no characters shown as you type, so make sure you are typing the correct password, and press enter.
Write the following command and press enter:
```bash
sudo raspi-config
```
Using the arrow keys, select “Localization Options” and “WLAN Country”, scroll down to the country you are currently located in and press enter. Navigate to the “Finish” button and press enter.
Make sure the Operating System is running its latest version. To do so, enter the following command and press enter:
```bash
Copy code
sudo apt-get update && sudo apt-get upgrade -y 
```
Install the necessary drivers, packages and libraries:
The following are required:
```bash
sudo apt-get install python3-pip
sudo pip3 install --upgrade setuptools
cd ~
sudo pip3 install --upgrade adafruit-python-shell
wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/raspi-blinka.py
sudo python3 raspi-blinka.py
pip3 install adafruit-circuitpython-ssd1306
sudo apt-get install python3-pil
sudo apt-get install python3-numpy
```
This installed the necessary software to run the python scripts that will power the OLED display.
The USB Wi-fi dongle I used required the RTL802.11 CU drivers which can be installed by entering this code into the command line:
```bash
sudo apt install -y bc git dkms build-essential raspberrypi-kernel-headers
git clone -b v5.8.7 https://github.com/fastoe/RTL8811CU_for_Raspbian
cd RTL8811CU_for_Raspbian
make
sudo make install
sudo modprobe 8821cu
```
Once the installation is complete, enter “cd” and press enter. Other USB dongles may require a different driver or may be plug-and-play compatible with the Raspberry Pi, meaning the Raspberry Pi is compatible with the dongle without installing any further drivers.
Insert the following command and press enter:
```bash
sudo shutdown -r now
```
This will reboot the Raspberry Pi. After waiting 1-2 minutes, repeat steps 7 and 8.
For the next part, an HDMI cable connected to a screen, a keyboard and a mouse are required. If this equipment is not available, this tutorial (link) may be followed to set the access point up from your computer. If you are using a wi-fi dongle, enter the command “ifconfig” and press enter, the dongle should show up as “WLAN1”, “WLAN0” is the Raspberry Pi’s built-in antenna. On any step that has a “WLAN0” in the code or as a file name, replace it with “WLAN1” to use the dongle’s antenna.
## Setting up the access point
This section is adapted from this [tutorial](https://www.tomshardware.com/how-to/raspberry-pi-access-point) found in Tom’s Hardware
1. Click on the network icon, it looks like two blue arrows: one pointing upwards and one pointing downwards. Click on advanced options -> Create Wireless Hotspot. If using a dongle, the option to do so using “WLAN1” should appear.
2. Name the network and set up a password by changing the security to WPA & WPA2 Personal and click create. Open a command line and enter “sudo shutdown -r now” to reboot the Raspberry Pi.
3. Click the network icon ->Advanced Options -> Edit connections. Select the name of your access point’s network and click the cog icon on the bottom right corner of the window. Open the “General” tab, check the box next to the option “Connect with priority” and set the number to 0. Click “save” and reboot the Raspberry Pi again.
4. Use your phone or another device and attempt to connect to your Raspberry Pi’s network. If the connection is successful, this second device should be able to access the internet. 

## Programming the OLED display
This project used an SSD1306 OLED display. Adafruit provides this [guide](cdn-learn.adafruit.com/downloads/pdf/monochrome-oled-breakouts.pdf) to set up OLED displays which includes how to connect the different types of displays available in the market to the board. NOTE: The case model is designed for a 0.96" 128x64 OLED display and using other models of a different size will require modifying the case model.

1. Connect the female-to-female jumper cables according to Adafruit’s instructions.
2. Connect your computer to the Raspberry Pi by opening a powershell window and repeating steps 7 and 8 of the setup instructions or open a command prompt from the Raspberry Pi if it is connected to a screen and has a keyboard and mouse.
3. Insert “python” and press enter.
4. Connect the display to the Raspberry Pi:
  Insert these two lines and press enter:
```bash
import board
import busio
```
  Paste this and press enter:
```bash
i2c = busio.I2C(board.SCL, board.SDA)
```
  Run these two lines:
```bash
import adafruit_ssd1306
oled = adafruit_ssd1306.SSD1306_I2C(128, 32, i2c)
```
  Paste this and press enter:
```bash
oled = adafruit_ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3d)
```
Finally, enter this command:
```bash
import digitalio
reset_pin = digitalio.DigitalInOut(board.D4) # any pin!
oled = adafruit_ssd1306.SSD1306_I2C(128, 32, i2c, reset=reset_pin)
```
5. The OLED display is now ready to be programmed. To create a python script the display can run, enter the following command to create a file in the text editor and press enter (replace “filename” with the name you want to give to the file):
```bash
nano filename.py
```
This will open the nano text editor. There are several example codes in the Adafruit website and around the internet that can make the OLED display show different kinds of information. In this case, the code will alternate between showing the network’s name and password every five seconds. If you want to do this, enter the following code:
```python
import board
import digitalio
import time
from PIL import Image, ImageDraw, ImageFont
import adafruit_ssd1306

# Define the Reset Pin
oled_reset = digitalio.DigitalInOut(board.D4)

# Change these
# to the right size for your display!
WIDTH = 128
HEIGHT = 32  #
# Use for I2C.
i2c = board.I2C()  # uses board.SCL and board.SDA
# i2c = board.STEMMA_I2C()  # For using the built-in STEMMA QT connector on a microcontroller
oled = adafruit_ssd1306.SSD1306_I2C(WIDTH, HEIGHT, i2c, addr=0x3C, reset=oled_reset)

# Clear display.
oled.fill(0)
oled.show()

# Create blank image for drawing.
# Make sure to create image with mode '1' for 1-bit color.
image = Image.new("1", (oled.width, oled.height))

# Load default font.
font = ImageFont.load_default()

def display_text(text):
    draw = ImageDraw.Draw(image)  # Move this line here
    draw.rectangle((0, 0, oled.width, oled.height), outline=0, fill=0)
    (font_width, font_height) = font.getsize(text)
    draw.text((oled.width // 2 - font_width // 2, oled.height // 2 - font_height // 2), text, font=font, fill=255)
    oled.image(image)
    oled.show()

while True:
    # First alternating message
    display_text("SSID: The name of your network goes here")
    time.sleep(5)  # Display the SSID for 5 seconds

    # Second alternating message
    display_text("Password: Password goes here")
    time.sleep(5)  # Display the Password for 5 seconds
```
  Press ctrl+x, y and then enter to save and close the file.
6. Enter “Python” followed by a space and the filename of the file that was just closed. If the display was connected properly, then information should be shown on the display.
7. To start the display’s program without having to use the command line or another computer, enter the following and press enter:
```bash
sudo nano /etc/rc.local
```
  And insert the following the line before “Exit 0”, replacing “path” with the path to the file and “filename” with the name of the file:
```bash
python3 /path/filename.py &
```
  The path may be found by running the command:
```bash
find / -name "filename"
```
  This will show the path to the file.
8. Reboot the Raspberry Pi. Once it turns on it should automatically show the information on the display.
## The case:
  The STL file for the case can be downloaded [here](https://drive.google.com/file/d/1H-IyX1jAiXuQxYSuHUFYoUYDQk-Jr2FN/view?usp=sharing). It is a modified version of [this model](https://thangs.com/designer/MVLPGaming/3d-model/Customizable-Raspberry-Pi-3B-case-screwless-21595). If you have access to a 3-D printer use the relevant slicing software, if you are using a 3-D printing service contact them to follow their procedures. Regardless, download the file for the case so it can be printed or modified. The following instructions are only relevant for those who will be printing their own cases.

1. Open your 3-D printer’s slicing software. In this case I used Ultimaker Cura and printed on an Ultimaker 3.
2. Use the recommended settings and click “Slice”. I like to have more infill than the recommended, but using the recommended settings will suffice.
3. Export the sliced file to a storage device and plug it into the 3-D printer. Follow the instructions on the 3-D printer’s screen.
4. Once the printing process is complete, remove the product carefully from its tray and remove the supports. Usually these will be fragile enough to use your hands, but sometimes a set of pliers may be needed.

## Assembling the device
1. Place the Raspberry Pi board without the MicroSD card on the bottom part of the casing and place the MicroSD card in the Raspberry Pi through the slot found in the case.
2. Connect the OLED display and align two screw holes on the display with two of the small, round holes in the casing’s top part.
3. Secure the OLED display to the casing. I tied them together, but it can be done with screws or other methods. Be creative if it does not work with screws.
4. Align the top and bottom parts of the case like so and snap them together. Push gently into the “stubborn” areas that are not snapped together yet.
5. Connect an ethernet cable between the router and the ethernet port.
6. If using one, connect the wi-fi dongle to any of the Raspberry Pi’s four USB ports.
7. If using a power bank or portable battery, place it on the hooks and connect its cable to the Raspberry Pi’s MicroUSB port.
  After finishing this last step, you should have a functioning access point.
  
## Use Case and Further Improvements: Materials and Mesh Network

As was mentioned earlier, this project’s ultimate goal is to be a realistic solution to the widening of the digital divide in rural and remote areas. As such, this project was designed with communities in the Peruvian rainforest in mind. In 2021, many of these had their schools or medical centers connected to satellite internet through the “Conecta Selva” program. This device would be connected to the building’s antenna and powered by a battery which would ideally be charged using solar energy to reduce power costs. Since it would be installed outdoors, using PLA for the case is not ideal, and a material like ABS may be more suited for the device after some post-processing to ensure it can withstand the rain and high temperatures of the area. 

With the device on the school’s roof or window, several buildings nearby will now be in reach of the wi-fi signal, and by placing similar devices on their roofs or windows would be able to extend the range of the signal. However, a “traditional” access point with repeaters will lead to connection speed and signal intensity being lost with the repeaters, as it will function as several wi-fi networks instead of a single one. The solution for this is to implement a mesh network with these devices. A mesh network will make all devices behave like a single network instead of multiple with the same name, with each of these devices being a “node”. The main node (gateway) will be the one that is connected to the internet, and all the other devices will be bridge nodes. This will result in a scalable network that has already been proven to work in Thailand, for example, where the Taknet network consists of one gateway node in a public building connected to the local ISP’s antenna and several routers acting as bridges in households throughout the town the network was installed in. This network has been replicated in several towns in Thailand with networks of different sizes, showing this solution can be replicated and scaled. Since this device already works as an access point, no hardware changes area required to program a mesh protocol into the device. This protocol would be BATMAN-Adv, which is already used in other community-owned networks like Freifunk in Germany. With a mesh network, perhaps the OLED displays could be programmed to display information like the other nodes that each device is connected to instead of just cycling through the SSID and password.

Since this device is using open-source technology, I believe that the user and maker community will be able to improve this device’s case and software to bring it closer to reaching its ultimate goal.

## Sources
- Espinoza, David, and David Reed. “Wireless Technologies and Policies for Connecting Rural Areas in Emerging Countries: A Case Study in Rural Peru.” Digital Policy, Regulation and Governance, vol. 20, no. 5, 2018, pp. 479–511., [Link](https://doi.org/10.1108/dprg-03-2018-0009).
- Fried, Limor (listed as “Lady Ada”). “Monochrome OLED Breakouts - Adafruit Industries.” Adafruit, 29 Jan. 2023, [Link](cdn-learn.adafruit.com/downloads/pdf/monochrome-oled-breakouts.pdf).
- Innes, Brian. “Create a Mesh Network over WiFi Using Raspberry Pi.” GitHub, 29 Nov. 2021, [Link](https://github.com/binnes/WiFiMeshRaspberryPi/blob/master/README.md).
- Hymel, Shawn. “Setting up a Raspberry Pi 3 as an Access Point.” SparkFun Learn, [Link](https://learn.sparkfun.com/tutorials/setting-up-a-raspberry-pi-3-as-an-access-point/all).
- Lertsinsrubtavee, Adisorn, et al. “Understanding Internet Usage and Network Locality in a Rural Community Wireless Mesh Network.” Proceedings of the Asian Internet Engineering Conference, 18 Nov. 2015, [Link](https://doi.org/10.1145/2837030.2837033).
- Parra, Raúl. “Conecta Selva, El Internet Satelital Amazónico, Lleva 98% De Avance En Perú.” DPL News, 22 June 2022, [Link](https://dplnews.com/conecta-selva-el-internet-satelital-amazonico-lleva-98-de-avance-en-peru/#:~:text=La%20iniciativa%20Conecta%20Selva%2C%20que,avance%20de%2098.5%20por%20ciento).
- Pounder, Les. “How to Turn a Raspberry Pi into a Wi-Fi Access Point.” Tom’s Hardware, 10 Sept. 2022, [Link](https://www.tomshardware.com/how-to/raspberry-pi-access-point). Accessed 11 May 2023.
- Rey-Moreno, Carlos, et al. “Making a Community Network Legal within the South African Regulatory Framework.” Proceedings of the Seventh International Conference on Information and Communication Technologies and Development, 15 May 2015, [Link](https://doi.org/10.1145/2737856.2737867).
- “TakNet 1 : A Rural Community Wireless Mesh Network.” Community Wireless Mesh Networks | TakNet 1, [Link](https://interlab.ait.ac.th/cwmn/tak1.php).
