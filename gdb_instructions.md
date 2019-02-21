# Quick Introduction to Debugging with GDB + OpenOCD
Version: 2019.1  


# Setup

The following instructions assume you are running on the ENGR523 Virtual
Machine.  If you are not running here, your paths may vary. 

Let's start by making sure we have everything we need to run OpenOCD installed.
In a terminal, enter the following:
    
    sudo apt-get install openocd 
    cd ~/workspace/
    wget https://docs.particle.io/assets/files/nrf52-var.cfg 

The password for the engr523 account is posted on Piazza. 

# Build your application

Please follow the steps illustrated [here](build_instructions.md) to build and
program your project onto the argon.  

# Debugging your application

Debugging your project requires two applications working simultaneously, OpenOCD
and GDB.  For that, you will need two terminals.  

## Terminal 1

This terminal will launch OpenOCD, which will connect to the Argon for you.  

    cd ~/workspace
    openocd -f interface/cmsis-dap.cfg -f ./nrf52-var.cfg -c "gdb_port 3333"
    
## Terminal 2

This terminal will lauch GDB itself. Here, we're assuming your application name
is 'timer'.  If you choose a different name, please update the second command. 
    
    cd ~/workspace/device-os/main
    arm-none-eabi-gdb ../build/target/main/platform-12/timer.elf

This will launch GDB, and tell it where to find the binary file of your
application.  Next, we need to type the following commands inside GDB itself.
These will connect to OpenOCD, tell the application to stop at the `setup`
function, and then restart the application.  

    target remote localhost:3333
    break setup
    monitor reset halt
    continue

From here, we recommend the following command to enable the "Text User
Interface":
    
    tui enable


# Debugging

From here, you can use GDB to debug your application.   GDB has many commands.
Here is a list of a few, and what they do: 

- continue: Continue executing until next break point/watchpoint.
- step: Step to next line of code. Will step into a function.
- next: Step to next line of code. Will step into a function.
- finish: Continue to end of function.
- break:  Creates a breakpoint at a specified line, address or function.
- clear:  Deletes a breakpoint at a specified location.
- info breakpoints:  List of the current breakpoints

For more documentation on GDB, we recommend going trying these links: 
- <http://www.yolinux.com/TUTORIALS/GDB-Commands.html#GDB_COMMANDS>
- <http://visualgdb.com/gdbreference/commands/>
