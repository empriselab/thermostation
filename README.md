This is the repository that processes real-time sensor data on the Raspberry Pi Pico onboard the CLAMP device. 

## Setup
### Repo setup
This repo uses a git submodule for the RP2040 SD SPI library. When cloning the
repo, make sure to clone with `--recurse-submodules` like:
```shell
$ git clone --recurse-submodules -j8 git@github.com:lmlack/thermostation.git
```

or if you have already cloned and forgot about the submodules, just run
```shell
$ git submodule update --init --recursive
```

### Build
This project, like all rpi pico C sdk projects, uses cmake to build. To build
with cmake, create a directory for all the cmake outputs (usually called build)
and run cmake while passing it the directory containing the CMakeLists.txt.
Then make can be run on the generate makefile to build the project:
```shell
$ mkdir build && cd build
$ cmake ../
$ make
```

### Flash
To flash on the firmware, there are many options. There are plenty of rpi pico
flashing tutorials out there, but the simplest options are to reboot the pico
while holding the boot button to put it into bootloader mode, and then copy the
uf2 file created in build/ by the build process to the USB mass storage device
for the pico that appears in bootloader mode, or install and use picotool:
```shell
$ make && picotool load hp_test.uf2 -f
```

### Run python data streamer
The repo includes a python program that deserializes and plots the data in
real time, using multiple processes to help ensure no dropped samples.

This serves as a good example of how to read and deserialize the data stream.

To run the plotting program, you'll need to install the following python
package prerequisites:
* pyserial
* numpy
* matplotlib

Once that's done, run the program like any other python program, passing one
positional command line arg, the path to the serial port:
```shell
$ python3 log_data.py /dev/ttyACM0
```

