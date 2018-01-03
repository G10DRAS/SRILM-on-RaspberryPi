# SRI Language Modeling Toolkit (SRILM) on RaspberryPi
How to compile SRILM (srilm-1.7.1) on the RPi2B (Rasbian-Whizzy/Jessie)

Although ```SRILM Toolkit``` is in C++, the compilation of ```SRILM``` requires knowledge of the type of machine which is to define certain variables pointing to necessary libraries. To do this, it provides a script ```srilm-1.7.1/sbin/machine-type``` that returns the type of machine from the output of the command ```uname -a```. The script has an internal list of machine type, which is used to define some variables or other. As expected, the machine type ```armv7l``` is not listed, so this script gave an error. To solve this, it is necessary to modify the script ```srilm-1.7.1/sbin/machine-type``` so that it will return a correct machine type (in our case, ```armv7l```). To do this, simply include machine type ```armv7l``` in the `uname -m` case statement as follows:<br />

```case "`uname -m`" in```<br />
```armv7l) MACHINE_TYPE=armv7l```<br />


Once this is done, you need to create a file ```srilm-1.7.1/common/Makefile.machine.armv7l``` for new machine type ```armv7l```. The easiest way to do this is to copy the file ```srilm-1.7.1/common/Makefile.machine.i686```

```cd ~/srilm-1.7.1/common```<br />
```cp Makefile.machine.i686 Makefile.machine.armv7l```<br />

And delete all the flags of the line ```GCC_FLAGS``` not begin with ```-W```. After the changes, line will looks like as follows:

```GCC_FLAGS=-Wall -Wno-unused-variable -Wno-uninitialized```<br /> 

Once this is done, define variables,```MACHINE_TYPE and SRILM```, and add them to ```PATH and MANPATH``` variable as follows:<br />


```sudo nano ~/.bashrc```<br />
```export SRILM=~/srilm-1.7.1```<br />
```export MACHINE_TYPE=armv7l```<br />
```export PATH=$PATH:$SRILM/bin/$MACHINE_TYPE:$SRILM/bin```<br />
```export MANPATH=$MANPATH:$SRILM/man```<br />

If TCL is not installed then install it as follows:

```sudo apt-get install tcl-dev```<br />

Insert path to ```tcl.h``` (e.g. ```-I/usr/include/tcl8.5```) to ```TCL_INCLUDE``` in ```common/Makefile.machine.armv7l```<br />

```# Tcl support (standard in Linux)```<br />
```TCL_INCLUDE = -I/usr/include/tcl8.5```<br />
```TCL_LIBRARY = -ltcl```<br />

The following command will compile ```SRILM``` and verify the installation. <br />

```cd ~/srilm-1.7.1```<br />
```make World```<br />
```make test``` <br />

License: To install, you will also need a copy of the `SRILM Toolkit`(http://www.speech.sri.com/projects/srilm/), 
for which you will need a license from SRI.
