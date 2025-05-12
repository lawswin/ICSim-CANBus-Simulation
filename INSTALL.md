ICSim + CAN-utils Installation

Install Required Dependencies:

First, make sure your system is up-to-date and install required packages.

```sudo apt-get update```

```sudo apt-get install can-utils libsdl2-dev libsdl2-image-dev```

```can-utils```: Tools for working with the CAN bus.
 ```libsdl2-dev, libsdl2-image-dev```: Libraries required for compiling ICSim graphical interface



Clone the ICSim Repository:

 ```gitclone https://github.com/lawswin/ICSim-CANBus-Simulation```

```cd ICSim```

This will download the official ICSim simulator source code and move you into the project directory.

Build the ICSim Application:
 
 Run the following:

```make all```

If you see a build error:

```sudo apt-get upgrade```

    
Then try again

```make all```

Set Up a Virtual CAN Interface (No Hardware Needed)

If you don’t have a physical CAN interface, create a virtual one


` sudo modprobe can`
   `sudo modprobe vcan`
   `sudo ip link add dev vcan0 type vcan`
   `sudo ip link set up vcan0`

Then verify it


```ip a | grep vcan0```
You should see something like vcan0: <NOARP,UP,...> — this means your virtual CAN is ready.

 Run the Simulator and Controls:
 
Now that everything is set up, split your terminal into 3 sections. Run each of the following in a different terminal tab or pane.

Terminal 1 — Launch the ICSim Dashboard:

```./icsim vcan0```

Terminal 2 — Launch the Control Panel:

```./controls vcan0```

Terminal 3 — (Optional) Monitor CAN traffic:

```cansniffer vcan0```

 
 If You Encounter This Specific Error:


```/usr/bin/ld: lib.o: error adding symbols: file in wrong format```

This likely means you’re mixing 32-bit and 64-bit object files. Here’s how to rebuild SDL manually:


# Go one level above ICSim if needed
cd ..

# Clone SDL manually
git clone https://github.com/libsdl-org/SDL.git
cd SDL
mkdir build
cd build

# Build SDL
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --config Release --parallel
Now compile ICSim using the local SDL build:


cd ../../ICSim
gcc -c -o lib.o lib.c -I../SDL/include
```make``

 You’re Done!
You now have:

A running ICSim dashboard and control panel

Virtual CAN traffic flowing

Optional sniffer monitoring traffic
