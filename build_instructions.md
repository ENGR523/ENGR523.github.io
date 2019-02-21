# Instructions to build applications locally

Version: 2019.2  

Particle provides the flexibility of building user applications using
a number of ways. For example the web IDE was already shown in the
class. The particle CLI tool can also be used to build applications
online and then download the binaries.

This documentation lists out a process to build user applications
locally against the complete device firmware source, which is open
sourced and available on [Github.](https://github.com/particle-iot/device-os) This provides the additional
flexibility of modifying the firmware code. It uses the ARM GCC
tool-chain for building the entire source code tree.


<a id="orgc331188"></a>

# Setup

The following instructions assume you are running on the ENGR523 Virtual Machine.  If you are not running here, your paths may vary. 

Let's start by blowing away any old firmware in our workspace directory.  In a terminal, enter the following:

    cd ~/workspace/
    rm -rf device-os

# Clone Firmware

Now let's pull a new version of the Particle firmware:

    git clone https://github.com/particle-iot/device-os.git
    cd device-os
    git checkout v0.9.0
    git submodule update --init

# Code our new application

In this example, we're going to create a project named timer.

    cd ~/workspace/device-os/user/applications
    mkdir timer
    cd timer

# Starter Code

We recommend you use the following starter code to get your project up and running quickly: 

    // A necessary header inclusion
    #include "application.h" 

    // Defining 2 int variables which references the D0 and D7 pins.
    int ledD7 = D7;

    // This will run the device in semi-automatic mode, and not connect to the
    // wifi, Particle cloud by default.
    SYSTEM_MODE(SEMI_AUTOMATIC);

    // setup() runs once, when the device is first turned on.
    void setup() {
        // Put initialization like pinMode and begin functions here.
        pinMode(ledD7, OUTPUT);
    }

    // loop() runs over and over again, as quickly as it can execute.
    void loop() {

        // To blink the LED, first we'll turn it on...
        digitalWrite(ledD7, HIGH);

        // We'll leave it on for 1 second...
        delay(1000);

        // Then we'll turn it off...
        digitalWrite(ledD7, LOW);

        // Wait 1 second...
        delay(500);
        // And repeat!
    }

# Building your project

This will build your project, and add the particle firmware.  Notice that we are adding a few extra flags to the make process.  

    cd ~/workspace/device-os/main
    make all PLATFORM-argon APP=timer PARTICLE_DEVELOP=1 USE_SWD_JTAG=y MODULAR=n

# Flash your project

Now you have a few options for flashing your application to the Argon, we
recommend the following.  This will flash the built image and the application
binary through DFU. Prior to executing this please ensure the **device is set
to DFU mode(blinking yellow led)**.

    make program-dfu PLATFORM=argon APP=timer 

# Cleaning your project

If you ever want to remove all of your build files, try the following:

    cd ~/workspace/device-os/main
    make clean all PLATFORM-argon APPDIR=timer

# Notes

Please email/contact Subhojit(susom@iu.edu) in case:

-   You are facing any issues with the setup/building
-   You feel this document is all gibberish and does not make any sense at all!
-   This documentation is missing something or all this could be better arranged

If you don't want to limit yourself to the VM that is shared, please
feel free to browse through Particle's [documentation](https://docs.particle.io/support/particle-tools-faq/local-build/) for setting up
a local build environment and you will get to know where I did get
ALL this from!


<a id="org434c676"></a>

# References

-   Particle [documentation](https://docs.particle.io/support/particle-tools-faq/local-build/) for setting up a local build environment
-   [github](https://github.com/particle-iot/device-os/blob/v0.8.0-rc.27/docs/build.md#quick-start) build doc page for particle firmware
-   Particle community [post](https://community.particle.io/t/locally-building-firmware-for-the-argon-and-xenon-boards/46765).

