## 519lab2-Setup-Guide
This guide explains how to get QT Py board to send data over USB via the C SDK.
We can get from “zero” to “hello_usb.c”!

```
Yixuan Wang
LinkedIn: https://www.linkedin.com/in/yixuan-wang-21ba40251/
Tested on: ASUS TUF Gaming F15, Windows 11
```
We can follow the [instructions](https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf) here in general, but there are **some points we need to change or add** to ensure it goes smoothly.

Since I use Windows system, so I nevigate directly to 9.2.
<img width="749" alt="image" src="https://user-images.githubusercontent.com/114169032/195381917-fa9d000e-01bc-412e-b036-62d365485628.png">

## 1. Installing the Toolchain
To build you will need to install some extra tools.
- [Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)
- [CMake](https://cmake.org/download/)
- [Build Tools for Visual Studio 2022](https://visualstudio.microsoft.com/zh-hans/downloads/)
- [Python 3.10](https://www.python.org/downloads/windows/)
- [Git](https://git-scm.com/download/win)

### 1.1 Arm GNU Toolchain
!<img width="430" alt="image" src="https://user-images.githubusercontent.com/114169032/195403722-927ad401-7523-4248-9b4c-f52621831ff9.png">

During installation you should tick the box to register the **path to the Arm compiler** as an environment variable in the
Windows shell when prompted to do so.

### 1.2 CMake
!<img width="430" alt="image" src="https://user-images.githubusercontent.com/114169032/195404057-25b4e1ca-52a6-41f2-9a27-db04c2e53908.png">

During the installation **add CMake to the system PATH** for all users when prompted by the installer.

### 1.3 Build Tools for Visual Studio 2022
!<img width="430" alt="image" src="https://user-images.githubusercontent.com/114169032/195404633-8f601fa1-106c-4c64-bd54-cad1c1547588.png">

When prompted by the Build Tools for Visual Studio installer you need to **install the C++ build tools only**

### 1.4 Python 3.10.8
!<img width="430" alt="image" src="https://user-images.githubusercontent.com/114169032/195405331-086f7a11-4795-4f08-bbbc-8fd20ab92466.png">

During the installation, ensure that it’s installed **'for all users'** and add Python 3.10 to the system PATH when prompted by
the installer. You should additionally **disable the MAX_PATH length limit when prompted at the end of the Python
installation**.

### 1.5 Git
!<img width="430" alt="image" src="https://user-images.githubusercontent.com/114169032/195441385-c17248e0-4b34-48d6-bd56-9634b48ad6c0.png">
1. When installing Git you should ensure that you **change the default editor away from vim to Notepad**.
2. Ensure you tick the checkbox to **allow Git to be used from 3rd-party software**.
3. You should also check the box **"Checkout as is, commit as-is"**, select **"Use Windows'default console window"**, and **"Enable experimental support for pseudo consoles"** during the installation process.


## 2. Getting the SDK and examples
```
C:\Users\Sharon\pico\Downloads> git clone -b master https://github.com/raspberrypi/pico-sdk.git
C:\Users\Sharon\pico\Downloads> cd pico-sdk
C:\Users\Sharon\pico\Downloads\pico-sdk> git submodule update --init
C:\Users\Sharon\pico\Downloads\pico-sdk> cd ..
C:\Users\Sharon\pico\Downloads> git clone -b master https://github.com/raspberrypi/pico-examples.git
```

## 3. Building "Hello World" from the Command Line
Select Windows > Visual> Studio 2022 > Developer Command Prompt for VS 2022 from the menu.
Then set the path to the SDK as follows,
```
C:\Users\Sharon\pico\Downloads> setx PICO_SDK_PATH "C:\Users\Sharon\pico\Downloads\pico-sdk"
```
Then close the current Command Prompt window and open a second Developer Command Prompt window where this environment variable will now be set correctly before proceeding. Navigate into the pico-examples folder, and build the 'Hello World' example as follows,
```
C:\Users\Sharon\pico\Downloads> cd pico-examples
C:\Users\Sharon\pico\Downloads\pico-examples> mkdir build
C:\Users\Sharon\pico\Downloads\pico-examples> cd build
C:\Users\Sharon\pico\Downloads\pico-examples\build> cmake -G "NMake Makefiles" ..
C:\Users\Sharon\pico\Downloads\pico-examples\build> nmake
```
After Cmake, it shows like this.

<img width="700" alt="image" src="https://user-images.githubusercontent.com/114169032/195444329-ca591cd3-23c3-4a09-9fbe-80e932f48d02.png">

After nmake, it will take a while to load.

<img width="700" alt="image" src="https://user-images.githubusercontent.com/114169032/195444419-5f33c9fd-0932-4c2d-a3c8-376f77bd6aca.png">

when it's done, it looks like this.

<img width="700" alt="image" src="https://user-images.githubusercontent.com/114169032/195444479-fd96ae0c-269c-4aa4-bb14-a382e1ad417a.png">

This will produce elf, bin, and uf2 targets, you can find these in the hello_world/serial and hello_world/usb directories inside your build directory. The UF2 binaries can be dragged-and-dropped directly onto a RP2040 board attached to your computer using USB.

## 4.Flashing and Running "Hello World"
When connecting RP2040 to our computer, we need to reset it first. Keep pressing the BOOTSEL button to force it into USB Mass Storage Mode(release the button after you've plugged the Pico into the PC).
But then you may find that whenever you drag-and-drop a file into this RPI-RP2 volume, it looks like "disappearing".
The I find the [solutions](https://www.raspberrypi.com/documentation/microcontrollers/raspberry-pi-pico.html#resetting-flash-memory)  here.
It explains this way.

!<img width="1000" alt="image" src="https://user-images.githubusercontent.com/114169032/195448101-78326f2a-7113-4bcf-8325-6b13f00e6e86.png">

So we need to download the uf2 file I point out in the picture and drag it to RPI-RP2. Then it can work as usual.
And now we can drag and drop "hello_usb.uf2" into the drive.

!<img width="400" alt="image" src="https://user-images.githubusercontent.com/114169032/195448980-b38a838c-1c88-43eb-9028-2f1d7e3291ea.png">

## 5.Installing Putty(output)
[Download](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) here

!<img width="500" alt="image" src="https://user-images.githubusercontent.com/114169032/195449282-9edad292-72bb-4af1-88d2-f7b7db63a39b.png">

we can use Windows Device Manager to find out which port the board is using.

!<img width="500" alt="image" src="https://user-images.githubusercontent.com/114169032/195450168-c2e89f63-05ff-4186-aa5e-fb0ca021cb04.png">

open Putty and Set parameters like this.

!<img width="500" alt="image" src="https://user-images.githubusercontent.com/114169032/195450395-14a9ed0b-09e0-4fd4-bf18-df02771c165a.png">

Then it shows" hello, world!"!

!<img width="350" alt="image" src="https://user-images.githubusercontent.com/114169032/195450582-01fd94bb-170e-4604-bb31-ac07a4965b0f.png">



