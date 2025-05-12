We can install icsim and can-utils using the following commands;

'git clone https://github.com/zombieCraig/ICSim.git'

sudo apt-get install can-utils

sudo apt-get install libsdl2-dev libsdl2-image-dev

sudo apt-get update — fix-missing

If you can’t or won’t purchase hardware devices, you can always set up a virtual CAN network. To set up a virtual CAN network;

sudo modprobe can

sudo modprobe vcan

sudo ip link add dev vcan0 type vcan

sudo ip link set up vcan0

Ip a // We can now see vcan0 in the output

cd ICSim

make all

If an error is received in the “make all” command;

sudo apt-get upgrade

If the same error persists;

sudo apt-get install libsdl2-dev libsdl2-image-dev

if this is the error -> (/usr/bin/ld: lib.o: error adding symbols: file in wrong format)

git clone https://github.com/libsdl-org/SDL

cd SDL

mkdir build

cd build

cmake .. -DCMAKE_BUILD_TYPE=Release

cmake — build . — config Release — parallel

cd ICSim

gcc -c -o lib.o lib.c -I/home/naoumine/SDL/include

make

We have finished the installation phase!

I suggest you to divide the terminal into 3 parts for ease of use.

Let’s run the following commands separately in the relevant terminals.

./icsim vcan0

./controls vcan0
