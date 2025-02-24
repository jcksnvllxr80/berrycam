
# BerryCam Support

### Steps to get BerryCam up and running on your Raspberry Pi and iOS device

#### Preparation

[1. What you need](#items-you-will-need) / [2. Getting started](#useful-guides-to-get-started)  / [3. Camera set-up](#setting-up-the-camera)

#### Capturing images

[4. The BerryCam script](#installing-and-running-the-berrycam-script) / [5. Using the BerryCam app](#using-the-berrycam-app-to-capture-images) / [6. Troubleshooting](#troubleshooting)

#### Additional setup

[7. Autostart BerryCam on boot](#other-things-to-try) / [8. Copying images from your Raspberry Pi using a shared directory](#other-things-to-try)

#### Raspistill version of the BerryCam.py script

BerryCam originally used Raspistill to capture images. However there is a new version which uses the popular PiCamera package. Both versions are available within this Repo, and designed to work with the latest version of the BerryCam app. **If you wish to use the PiCamera version, please update your BerryCam app from the App Store to the latest version.**

Whilst the code is different in areas, set up remains the same process, although the raspistill version is now called `berryCam-raspistill.py`

You can rename this file to `berryCam.py` and replace the PiCamera version if you wish to use the Raspistill script without the need to change the run commands within this guide.

[Back to top](#top)

---

# Items you will need

It's important that you have the following items to run BerryCam.

1. A Raspberry Pi computer (any model) with WiFi connectivity.
2. An SD card. Most models now take the Micro SD type although some work with standard sized SD cards.
3. All the necessary leads (power supply, HDMI cable, mouse and keyboard, if working from Raspberry Pi OS desktop).
4. A working WiFi connection.
5. A Raspberry Pi camera module (V1, V2, NoIR and HQ Camera all work).
6. An iOS device running iOS 14 (iPhone or iPad).

[Back to top](#top)

---

# Useful guides to get started

> Before we get into the detail of setting up and using BerryCam, here are some useful resources. It's worthwhile taking the time to explore these pages as they will help you get your Raspberry Pi up and running for the first time. If you're familiar with all of this you may wish to [skip this part](#setting-up-the-camera)

[Setting up your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up)
Here you’ll learn about your Raspberry Pi, what things you need to use it, and how to set it up.

[Manually configuring Wifi](https://www.raspberrypi-spy.co.uk/2017/04/manually-setting-up-pi-wifi-using-wpa_supplicant-conf/)
Set up the `wpa_supplicant.conf` so you have Wifi connectivity on first boot.

[Using your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-using)
Learn about Raspberry Pi OS, included software, and how to adjust some key settings to your needs.

 [Remote access using the Terminal/SSH](https://www.raspberrypi.org/documentation/remote-access/)
It's recommended you take a look at the resources here as you will need to use Terminal and some basic commands to install BerryCam and run the Python script.

[Other Frequently Asked Questions](https://www.raspberrypi.org/documentation/faqs/)
A wide range of information related to the hardware and software to get up and running with the various models of Raspberry Pi.

[Raspberry Pi OS](https://downloads.raspberrypi.org/raspios_full_armhf/release_notes.txt) is well maintained and receives regular updates. This may change the instructions here from time to time. If you notice any differences, please let me know by raising an issue and I'll update the documentation, with thanks in advance.

[Back to top](#top)

---

# Setting up the camera

> You will need to physically connect the Raspberry Pi camera module using the supplied ribbon cable. This is generally the same process for all models although the connector may be positioned slightly differently, or in the case of Raspberry Pi Zero, require a different connector ribbon cable. If you've done this already, again you may wish to [skip this part](#installing-and-running-the-berrycam-script)

[Getting started with the Camera Module](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera)
Learn how to connect the Raspberry Pi Camera Module to your Raspberry Pi in preparation for use with BerryCam

[Basic usage of raspistill](https://www.raspberrypi.org/documentation/usage/camera/raspicam/raspistill.md)
BerryCam uses raspistill to capture images on the Raspberry Pi. You won't need this reference guide to use BerryCam yet it is handy if you want to learn more and take things even further.

[Basic usage of PiCamera](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/4)
For more information on PiCamera and how you can use it within your own projects, refer to this getting started guide on the Raspberry Pi website.

[Full API documentation for PiCamera](https://picamera.readthedocs.io/)
A complete guide to the command reference and some example recipes, when you decide to create your own capture scripts.

[A guide to all the Raspberry Pi camera applications](https://www.raspberrypi.org/documentation/raspbian/applications/camera.md)
This useful reference covers all the commands for the applications provided with the Raspberry Pi. These include **raspistill**, **raspivid**, **raspiyuv** and **raspividyuv**. All applications are driven from the [command line](https://www.raspberrypi.org/documentation/remote-access/).

#### A note on image capture failing when using PiCamera with the HQ Camera Module

When you are running the PiCamera version of BerryCam, and the HQ Camera, in some cases you may encounter **out of resource** errors. Image capture will fail when triggered using the app.

This can be addressed using the Raspberry Pi configuration tool. Refer to [Image Capture Failing with HQ Camera Module in Troubleshooting](#imagecapturefailingwithcameramodule) for more detail.

[Back to top](#top)

---

# Installing and running the BerryCam script

> We will be using the **command line** to set up and run the BerryCam Python script. Be sure to read the guides on the [command line](https://www.raspberrypi.org/documentation/usage/terminal/)

There are two ways we can interact with the Raspberry Pi. The easiest and best documented way to do this is using the Raspberry Pi OS desktop. This requires a display, keyboard and mouse. Start by reading ***using the Raspberry Pi OS desktop*** below

If this isn't possible, you can connect using a [VNC client](https://magpi.raspberrypi.org/articles/vnc-raspberry-pi) or directly using terminal on a Mac or an SSH client like [PuTTy](https://www.putty.org) on a Windows PC. If this is how you need to connect, start with ***using the command line from another machine***

Given this section is quite lengthy, you may wish to skip to the parts relevant to you:

**Connecting:**
[Using the Raspberry Pi OS desktop](#usingtheraspberrypiosdesktop)
[Using the command line from another machine](#usingthecommandlinefromanothermachine)

**Installing:**
[Downloading and installing BerryCam onto your Raspberry Pi](#downloadandinstallberrycamontoyourraspberrypi)
[Checking you have PiCamera installed](#checkyouhavepicamerainstalled)

**Running:**
[Running the BerryCam Python script on your Raspberry Pi](#runningtheberrycampythonscriptonyourraspberrypi)

**Existing users:**
[Updating your BerryCam.py script](#updatingtheberrycampyscript)

#### Using the Raspberry Pi OS desktop

Start LXTerminal on the Raspberry Pi using the icon (a small black window icon with a white arrow) on the top tool bar desktop. You will be presented with a window like this.

![\[Raspberry Pi OS Terminal\]](https://raw.githubusercontent.com/fotosyn/berrycam/master/Assets/raspberry-pi-os-terminal.png)

First of all, we will need to find the IP address of the Raspberry Pi on your network. To do this type in

```
ifconfig
```

and press return. This will return quite a bit of information. the only part you will need is highlighted in light grey (to show you where to look) in the screenshow below.

![\[Raspberry Pi OS Terminal IP Address\]](https://raw.githubusercontent.com/fotosyn/berrycam/master/Assets/raspberry-pi-os-ipaddress.png)

Take a note of this number (IP address) as you will need it later on in the BerryCam app to connect.

#### Using the command line from another machine

> **Before you begin** – you will need to know the connected IP address of your Raspberry Pi.  If you can't use the Raspberry Pi OS desktop, you can get this from your broadband router control panel (your provider will have given you this information when it was set up) or if you use a WiFi mesh network like Google WiFi this number (IP address) will be available under Connected Devices in the Google WiFi app. **Take a note of this number (IP address) as you will need it later on in the BerryCam app to connect.**

To connect using a remote command line on a terminal, we need to use a protocol called SSH. You can use Terminal on a Mac (press cmd + space and type 'terminal' to launch this) or  [PuTTy](https://www.putty.org) on a Windows PC.

**When connecting, you'll be using using your Raspberry Pi username and password.**  This is normally `pi`for the username and `raspberry` for the password. It is recommended that you [change this](https://www.raspberrypi.org/documentation/linux/usage/users.md) to something only you know.

Connecting using MacOS Terminal:

![\[MacOS Terminal\]](https://raw.githubusercontent.com/fotosyn/berrycam/master/Assets/macos-terminal.png)

Connecting using PuTTy:

![\[PuTTy Terminal\]](https://raw.githubusercontent.com/fotosyn/berrycam/master/Assets/putty-terminal.jpg)

To connect, enter

```
ssh pi@YOUR_IP_ADDRESS
```

replacing YOUR_IP_ADDRESS with the number you took note of and enter your password when prompted. There will be no typing input when entering the password on some cases, so be sure to focus on entering the right keystrokes.

#### Download and install BerryCam onto your Raspberry Pi

First of all make sure you're in the home directory for the user you are logged in as. This will normally be 'pi' and is located `/home/pi/`

You can double check this using the

```
pwd
```

command. If you find you are in a different directory simply use:

```
cd /home/pi
```

or

```
cd /home/<your-user-name>/
```

Next, we need to clone the BerryCam script into your home folder. Within the terminal, simply type:

```
git clone https://github.com/fotosyn/berrycam.git
```

After some activity, the `berryCam.py` file will be copied onto your Raspberry Pi. This will be in a folder named berrycam. If you receive an error at this point (command not found), you will need to install git which can be done by typing:

```
sudo apt-get update
sudo apt-get install git
```

(Once this is complete, you'll need to repeat the clone step again.)

To check the `berryCam.py` file has been downloaded and unpacked, or set up as a file issue the command:

```
ls
```

This will list files currently in home. You will notice the new folder named berrycam. Change to this directory with:

```
cd berrycam
```

You can now check the required script has been downloaded. In the command line, enter again:

```
ls
```

You will see a number of files and a folder. Amongst these there should be a Python **berryCam.py** file. This is needed to provide the link between the iOS device and the Raspberry Pi.

Check that you have Python 3.7.0 by issuing the command:

```
python3 --version
```

If you have a version lower than this, we recommend you check the [Troubleshooting](#troubleshooting) guide.

#### Check you have PiCamera installed

If you are using the Raspbian or RaspberryPi OS distro, you probably have picamera installed by default. You can find out simply by starting Python and trying to import picamera

```
python3 -c "import picamera"
```

If you get no error, you’ve already got picamera installed. If you don’t have picamera installed you’ll see a `Traceback` type error. You'll need to install PiCamera before continuing.

```
sudo apt-get install python-picamera python3-picamera
```

#### Running the BerryCam Python script on your Raspberry Pi

BerryCam needs to be run as a Python process to provide the necessary links to allow the BerryCam iOS app to trigger the camera, provide previews and save files. To run simply enter:

```
nohup python3 berryCam.py > berryCam.log & tail -f berryCam.log
```

The Python script will run in the background and you will see the following message:

```
B E R R Y C A M -- Listening on port 8000 
Please ensure your BerryCam App is installed and running on your iOS Device
```

You can close terminal and as long as the Raspberry Pi has power will continue to run BerryCam.

If you experience any problems at this point, please check [Troubleshooting](#troubleshooting)

#### Updating the BerryCam.py script

If you've already started using the BerryCam app with the BerryCam script (thankyou!) - it's recommended you update to the latest version in the repo. This is easy if you've cloned it from this repo by simply using the command

```
git pull origin master

```

The [GitHub desktop tool](https://desktop.github.com) is another easy to use option to keep things up to date without the need for the command line.

[Back to top](#top)

---

## Using the BerryCam app to capture images

> Make sure you have downloaded and installed the BerryCam app onto your iOS device, and that both devices are connected on the same local network (generally your cellular connection won't work. Wifi will be easiest).

[![Download on the App Store](https://raw.githubusercontent.com/fotosyn/berrycam/master/Assets/app-store-badge.png)](https://apps.apple.com/app/berrycam-take-images-with-a-raspberry-pi-camera/id687071023)

To set up the connection on the iOS App, tap the settings button (gear/cog icon), scroll down to **Raspberry Pi Settings**

1. Select the correct version of camera you are using with your Raspberry Pi (this will dictate the output size of the image to be captured)
2. Update your IP address with the one given when you issued the `ifconfig` comnmand
3. Make sure you're using the correct port number. In most cases this will be 8000 and is set to this as default.

Once complete, select **Done** and you will be returned to the main screen. After a brief pause, BerryCam will detect your Raspberry Pi and the capture button will change to green. If this does not happen, check all of the steps above and make sure the IP address entered is correct.

#### You can now start capturing images

Simply press the large green capture (camera) button. After a short pause, the image will then appear in your iOS device. You can experiment with various capture parameters by revisiting the settings panel and updating. There's no need to worry about losing any captures you make.

Images are saved locally to the Pi, and can be accessed from within BerryCam, either by tapping the IP address shown on larger display iOS devices and iPads, or by going back into settings (gear/cog menu) and selecting 'Review images on Raspberry Pi'

You can also access the address directly on a browser on a device on the same local network by entering the address in the format below:

```
http://YOUR_IP_ADDRESS:8000/berrycam/
```

You can also save or share the currently captured image directly from the iOS device using the share button.

BerryCam is a quick and easy way to unlock experimentation with the Raspberry Pi camera modules. Try combinations of image effects, exposure controls and white balance to create some striking photographs!

[Back to top](#top)

---

### Install as a service that runs at boot

#### Install the gpg key and then the repo

```bash
curl -s --compressed "https://jcksnvllxr80.github.io/aw_rpi_ppa_repo/KEY.gpg" | sudo apt-key add -
sudo curl -s --compressed -o /etc/apt/sources.list.d/my_list_file.list "https://jcksnvllxr80.github.io/aw_rpi_ppa_repo/my_list_file.list"
sudo apt update
```

#### Install the service

```bash
sudo apt install berrycam
```

#### Updating the berrycam service

```bash
apt install --only-upgrade berrycam
```

#### Uninstall the service

```bash
sudo apt remove berrycam -y
```

#### Starting, stopping, restarting, or getting the service status

##### start berryCam

```bash
sudo systemctl start berryCam
```

##### stop berryCam

```bash
sudo systemctl stop berryCam
```

##### restart berryCam

```bash
sudo systemctl restart berryCam
```

##### get berryCam service status

```bash
sudo systemctl status berryCam
```

# Troubleshooting

If you are running an earlier version of Python3, pre version 3.3 then you will encounter a problem with the flush=true parameter in the `berryCam.py` script.

### Check your version of Python3

To check the version supply the command

```
python3 --version
```

If this returns less than 3.7.0 then there are a few things you can do.

#### Things you can do

**1. Upgrade your Raspberry Pi OS**

You could install a newer version of Raspberry Pi OS which has newer versions of Python. This is definitely the most direct route to consider if you're blowing the dust off a trusty Pi that's been sitting in the cupboard.

See [Setting up your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up) to get the latest version of Raspberry Pi OS and flash to your SD card.

**2. Update Python3 to version 3.7.0**

Alternatively, you can update your version of Python3. Be aware that this will a fair amount of time and involves a number of steps that need to be followed in this specific order. To update to the newest version with the following commands:

**Download and extract the latest version of Python3 logged in as root**

```
sudo su
```

```
cd /usr/src
```

```
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
```

```
tar -xf Python-3.7.0.tgz
```

**Install dependencies**

```
apt-get update
```

```
apt-get install -y build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libffi-dev
```

**Configure and install Python 3 (this part may take some time)**

```
cd Python-3.7.0
```

```
./configure --enable-optimizations
```

```
make altinstall
```

**Update the link to the newly installed version of Python**

```
ln -s /usr/local/bin/python3.7 /usr/local/bin/python3
```

**Check the version of Python (should return 3.7.0)**

```
python3 --version
```

Thanks to [Samx18](https://samx18.io/blog/2018/09/05/python3_raspberrypi.html) for the original guide to this detail. **Perform a reboot of the Pi to be doubly sure that this has been applied.**

**You should then be able to launch BerryCam using the command:**

```
sudo nohup python3 berryCam.py > berryCam.log & tail -f berryCam.log
```

### Image Capture Failing with HQ Camera Module

In some cases, you may encounter an out of component error when using PiCamera and the HQ Camera Module.

```
mmal: mmal_vc_component_enable: failed to enable component: ENOSPC
```

This is easily fixed using the Raspberry Pi config tool which is accessed using the terminal. In an active terminal session with your Raspberry Pi, use the command:

```
sudo raspi-config
```

Navigate to item **7 Advanced Options** in the menu screen that appears using the cursor keys and press enter. Again, using the cursor keys navigate to **A3 Memory Split** and press enter. You will then see a screen as follows. Enter 256MB where it reads 128MB.

An updated version of [Raspberry Pi OS](https://downloads.raspberrypi.org/raspios_full_armhf/release_notes.txt) has the GPU memory setting within **4 Performance Options** and option **P2 GPU Memory**

![\[Adjust GPU Memory in raspi-config\]](https://raw.githubusercontent.com/fotosyn/berrycam/master/Assets/raspi-config-screen.png)

Select `<Ok>` to continue, and then reboot the Raspberry Pi. This should address any issues related to image capture.

[Back to top](#top)

---

# Other things to try

### Autostart BerryCam on boot

If you regularly use your Raspberry Pi, set up with BerryCam to capture images, you may wish to set up the BerryCam.py script to launch automatically when the Pi is booted. This is quite an easy process and is particularly useful if you want to be able to plug your Pi in to power headless, and have it run BerryCam without configuration or a manual start.

For more information on editing the `rc.local` file please refer to the [Raspberry Pi documentation](https://www.raspberrypi.org/documentation/linux/usage/rc-local.md)

On your Pi, edit the file /etc/rc.local using the editor of your choice. You must edit with root, for example:

```
sudo nano /etc/rc.local
```

In the line **before** `exit 0` add the following lines. Be sure to reference absolute filenames rather than relative to your `/home` folder replacing `<your_user_name>` with your own user name (in many cases this is `pi`):

```
cd /home/<your_user_name>/berrycam/
nohup python3 berryCam.py > berryCam.log & tail -f berryCam.log
```

Save this file `CTRL x` confirming with `Y` and `Enter` then restart your Raspberry Pi. If this has been set up correctly and your BerryCam app should connect when you supply the correct IP address in the app.

### Copying images from your Raspberry Pi using a shared directory

You can share your Raspberry Pi folders with other devices on your network. A popular choice for MacOS users is [Netatalk](http://netatalk.sourceforge.net), with [Samba](https://www.samba.org) recommended for Windows users.

There are a number of guides online to do this, for [MacOS](https://spellfoundry.com/docs/copying-files-to-and-from-raspberry-pi-and-mac/) users (Netatalk) and [Windows](http://coffeetime.solutions/share-folder-raspberry-pi-access-windows/) users (Samba)

Once completed, and if you connect with your Pi's login credentials, you will see the shared folders with your captured images within the `berrycam` folder.

[Back to top](#top)
