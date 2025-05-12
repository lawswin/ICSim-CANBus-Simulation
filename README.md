# ICSim-CANBus-Simulation
CAN Bus simulation using ICSim for educational and cybersecurity research
------------------------------------------


------------------------------------------



Compiling
---------
You will need:
* SDL2
* SDL2_Image
* can-utils

You can get can-utils from github or on Ubuntu you may run the following

```
  sudo apt-get install libsdl2-dev libsdl2-image-dev can-utils  
```



```
  
  
```

Testing on a virtual CAN interface
----------------------------------
You can run the following commands to setup a virtual can interface

```
  sudo modprobe can
  sudo modprobe vcan
  sudo ip link add dev vcan0 type vcan
  sudo ip link set up vcan0
```

If you type ip a you should see a vcan0 interface. A setup_vcan.sh file has also been provided with this
repo.

Usage
-----
Default operations:

Follow these steps to run the ICSim CAN bus simulation with default settings

Open a terminal and run
```
  ./icsim vcan0
```
This launches the graphical instrument cluster interface, simulating a vehicle dashboard connected to a virtual CAN network (vcan0).



Launch the Controls Interface

In a second terminal, execute
```
  ./controls vcan0
```
This opens the control panel for sending input (like turn signals or speed changes) over the CAN bus

Important Notes
The IC simulator (icsim) and the controls interface (controls) are synchronized by default.

Pressing buttons in the control panel generates CAN packets.

The IC simulator monitors these packets and updates the dashboard display accordingly.

It is recommended to use a game controller (such as an Xbox controller) for smooth interaction, though keyboard input also works.

Troubleshooting
---------------
* If you get an error about canplayer then you may not have can-utils properly installed and in your path.
* If the controller does not seem to be responding make sure the controls window is selected and active

## lib.o not linking
If lib.o doesn't link it's probably because it's the wrong arch for your platform.  To fix this you will
want to compile can-utils and copy the newly compiled lib.o to the icsim directory.  You can get can-utils
from: https://github.com/linux-can/can-utils

## read: Bad address
When running `./icsim vcan0` you end up getting a `read: Bad Address` message,
this is typically a result of needing to recompile with updated SDL libraries.
Make sure you have the recommended latest SDL2 libraries.  Some users have
reported fixing this problem by creating symlinks to the SDL.h files manually
or you could edit the Makefile and change the CFLAGS to point to wherever your
distro installs the SDL.h header, ie: /usr/include/x86_64-linux-gnu/SDL2

There was also a report that on Arch linux needed sdl2_gfx library.

CAN Hacking Training Usage
--------------------------
To *safely* train on CAN hacking you can play back a sample recording included in this repo of generic CAN traffic.  This will
create something similar to normal CAN "noise".  Then start the IC Sim with the -r (randomize) switch.

```
  ./icsim -r vcan0
  Using CAN interface vcan0
  Seed: 1401717026
```

Now copy the seed number and paste it as the -s (seed) option for the controls.

```
  ./controls -s 1401717026 vcan0
```

This will randomize what CAN packets the IC needs and by passing the seed to the controls they will sync.  Randomizing
changes the arbitration IDs as well as the byte position of the packets used.  This will give you experience in hunting down
different types of CAN packets on the CAN Bus.

For the most realistic training you can change the difficulty levels.  Set the difficulty to 2 with the controls:

```
  ./controls -s 1401717026 -l 2 vcan0
```

This will add additional randomization to the target packets, simulating other data stored in the same arbitration id.
