# LX200 Driver build for Indi 

This is how I built the LX200 drivers for indi/Ekos/KStars.

You should get the source code from the official source as I took a copy (Dec 2025), in order to try and have a look at what was going on "inside the hood".

# Why ?

Because I can not get the simulator to work.

# What Simulator ?

I would like to use the ONSTEP LX200 Controller which has a simulator option. 
I can set the Simulator - but then nothing seems to happen.

By building it myself I can try and debug what is going wrong.

# Build Steps 

    mkdir build
    cd build 
    cmake ..  
    make -j4

It will compile the LX200 code as project_id, and create a symbolic link 

## Run via indiserver 

This is just (assuming you are in the SOURCE folder)

    indiserver indi_ccd_simulator build/indi_lx200_OnStep

Now run KStars in another terminal/pc and you can see the output.
