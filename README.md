wiringPi by Ansaya
==================

This fork of Gordon's wiringPi library is intended to add some missing features and some C++ support to enhance usability.

Upgrades
========

### wiringPiISR
This function is used to enable an ISR on a given pin. This behaviour is achieved enabling requested edge mode on given pin and starting a pthread which will poll (linux poll function is used) the pin file descriptor and trigger given callback when requested condition is detected (rising edge, falling...). This is ok, but there is no way to stop this thread and changing the function causes a new thread to be spawned.

My new implementation consist of a similar apporach, but configuring the thread in such a way that it can be stopped and joined when needed.

ISR setup remains the same as Gordon's, but now you can call wirigPiISR in two new flavours:
- wiringPiISR(pin, mode, NULL) - this will change edge mode only, keeping current callback active
- wiringPiISR(pin, INT_EDGE_NONE, NULL) - this will disable ISR on 'pin' if any (joins the thread and do the housekeeping as explained above)

Furthermore calling wiringPiISR(pin, mode, newFunc) when an ISR is already active will update mode and callback function (disabling previous ISR thread and callback).

### wiringPiSetup
In original Gordon's implementation four setup functions are present, now I've merged all of them in a single wiringPiSetup(int wpi_mode) function.

My new implementation will check for subsequent function calls and complain in case they requere different setup modes. Saying it will complain I mean it will print an error and exit your program, because using multiple modes isn't possible!

### Error codes
In current version of Gordon's library error codes have been disabled (thus they can be enabled again using environment variables), but in my opinion it is really important to care about error conditions, so I enabled error codes back. Check for them and teach your program how to handle them, this will lead you to a much better software.

Gordon Credits
===============
Above verison of wiringPi is an extension of the original one and will behave in different ways in some cases. 

Please contact me for any question and remember to check http://wiringpi.com/ for the official documentation and latest updates about the original library.

If you need the original wiringPi library by Gordon Henderson check https://git.drogon.net/

Thanks!