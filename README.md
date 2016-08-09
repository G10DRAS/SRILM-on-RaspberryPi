# SRILM-on-RaspberryPi
Compile SRILM on the RPi2B (Raspbian-Whizzy)

Compilation of SRILM in the Raspberry Pi:

Although it is in C++, the program compilation SRILM requires knowledge of the type of machine which is to define certain variables pointing to libraries necessary. To do this, it provides a script sbin/machine-type that returns the type of machine from the output of the command uname -a. The output of this script is a type of machine has an internal list that the program, which is then used to define some variables or other. As expected, the machine type "Raspberry Pi" was not listed, so this script gave an error. To solve this, it was necessary to modify the script sbin/machine-type making return a given string (in our case, "armv7l") when the machine out Raspberry Pi. To do this, simply include in the case `uname -m` in the following case:

armv7l) MACHINE_TYPE=armv7l
;; 

Once this is done, you need to create a file in common/Makefile.machine.armvl64 with the variables corresponding to the Raspberry Pi. The easiest way to do this is to copy the file common/Makefile.machine.i686

cd common 
cp Makefile.machine.i686 Makefile.machine.armv7l

And the new file to delete all the flags of the line GCC_FLAGS not begin with -W:

GCC_FLAGS=-Wall -Wno-unused-variable -Wno-uninitialized 

Once this is done, you need to define variables,MACHINE_TYPE, add to PATH directories $SRILM/bin/$MACHINE_TYPE and $SRILM/bin and add to MANPATH directory $SRILM/man.

sudo nano ~/.bashrc

export SRILM=/home/pi/srilm-1.7.1
export MACHINE_TYPE=armv7l
export PATH=$PATH:$SRILM/bin/$MACHINE_TYPE:$SRILM/bin

sudo apt-get install tcl-dev

Insert path to tcl.h (e.g. -I/usr/include/tcl8.5) to TCL_INCLUDE in common/Makefile.machine.armv7l

# Tcl support (standard in Linux)
TCL_INCLUDE = -I/usr/include/tcl8.5
TCL_LIBRARY = -ltcl

The command will install the program and verify that is installed correctly. 

make World 
make test 
