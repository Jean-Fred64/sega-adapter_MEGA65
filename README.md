# Sega Mega Drive (Genesis) controller to MEGA65

Adapt a Sega Mega Drive controller to the 9-pin joystick standard
using a Microchip PIC 16F1847 microcontroller.

![MEGA65 version](img/Sega%20Adapter%20_%20MEGA65%20_%20M30.jpg)
This adapter is compatible with [most
computers](#computer-compatibility) that use the Atari 9-pin joystick
connector, supporting most of the features of [Sega Mega Drive
controllers](#controller-compatibility), which use the same connector
with a slightly different pinout.  The Mega Drive was known as Genesis
in the US.

Read all you need to?  [Here's how to build the adapter](#build).


## Features of this adapter

The adapter uses a microcontroller to talk to the Mega Drive controller
and map its four buttons to the three inputs available on an 8-bit
Atari.

The B button on the controller is mapped to the normal joystick button.
Buttons A and C are connected to pins 5 and 9 on the joystick port;
these can be read as extra joystick buttons by the computer.  Most games
that only support one extra button use the C button (pin 9).

### "A is Up" toggle

The start button has no natural mapping since the Atari can only read
three buttons, but a common complaint in platform games on the Atari
(and later the Amiga) was that they often used joystick Up to jump
because of the lack of extra joystick buttons.

A dedicated Up button would solve this problem, but the Start button is
not usually in a good location to be used as a jump button.  The adapter
therefore supports this feature indirectly: holding the Start button
down for a second maps the A button to Up.

Since the joystick button often starts games, a short press of the Start
button is mapped to joystick button 1.  This allows Start to both serve
its normal function (in supported games) and switch the A button between
modes.

### SNACK enhanced mode

[SNACK](https://abbuc.de/produkt/snack-adapter/) is an Atari adapter for
SNES controllers.  It has an enhanced mode that allows all of the
buttons to be read by the Atari, and this adapter supports that mode if
you have a six-button controller.

To switch to SNACK mode on a Sega controller, press Mode, Start, and C
(the right shoulder button if you're using an 8BitDo M30 controller) at
the same time.  Mode+Start+Z (left shoulder on the M30) goes back to
normal mode.

If you're using an 8BitDo Retro Receiver with an Xbox or PlayStation
controller, the button layout is different.  Press Mode+Start+X (left
shoulder) to switch to SNACK mode on these controllers, and Mode+Start+Z
(right shoulder) to go back to normal mode.

The SNES controller has four main buttons: A, B, X, and Y; left and
right shoulder buttons; and START and SELECT.

The main buttons have the same labels and approximately the same diamond
configuration as the A, B, X, and Y buttons on a Mega Drive controller,
but the AB and XY buttons are swapped.

The Mega Drive controller does not have shoulder buttons, so the Z and C
buttons are used instead.  As noted above, the 8BitDo Retro Receiver
maps Z and C to the shoulder buttons on the M30 controller.

Finally, the Mode button stands in for SELECT.

#### Notes

- SNACK mode uses the joystick ports in a completely different way to a
  normal joystick and will produce unexpected results in software that
  is not written to detect it.
- The buttons are not completely independent.  Only one of START, SELECT
  (Mode), X, and Y can be detected at any time, and any of those buttons
  will override A and B.  However, A and B are independent of each
  other, and the shoulder buttons are entirely independent.
- Centering the joystick relies on fairly precise timing to charge the
  Atari's pot capacitors to a specific range, so different computers and
  even cable lengths may make the joystick appear to be intermittently
  leaning in one direction.
- This feature is still experimental.

### Autodetection (Atari 8-bit machines only)

Because of the way the paddle circuits are used to read the second and
third button inputs, 8-bit Ataris read an unconnected second or third
button input as if the button is being held down.

This allows\* games running on the computer to detect the presence of a
multibutton controller by assuming that a button does not exist until it
has been seen to be released.  The adapter supports this autodetection
by leaving its button outputs open unless a Mega Drive controller is
detected or the corresponding button input is pulled low.

To activate the second button when a Mega Drive controller is not
connected, pin 9 on the controller input must be pulled low before the
adapter starts driving the second button output.  This means that the
second button must be pressed once before it can be detected by the
computer.

The third button output is open unless a Mega Drive controller is
detected since there is no other source for a third button input.

\* The astute reader will note that games are _required_ to ignore
button inputs until the button is released, otherwise a nonexistent
button will appear to be continually held down.


## Computer compatibility
### 8-bit Atari computers

The adapter has mostly been tested on 8-bit Ataris using
[Joy2B+](https://github.com/ascrnet/Joy2Bplus) enhanced games.

### 8-bit Commodore computers and MEGA65

The C64, C128, and VIC-20 can also read the second and third buttons.
The extra buttons on these machines are active high, which is opposite
to Atari and Sega.

Pulling the /C64 pin (number 4) on the PIC low inverts the outputs for
the extra buttons, allowing them to be read correctly by the 8-bit
Commodores.  The simplest way to do this is to install a jumper in the
appropriate location on the circuit board.

Because the joystick inputs of the 8-bit Commodores are somewhat
sensitive, all of the pins are normally open (floating).  The extra
button pins are pulled high when active, all other signals are pulled
low when active.

### Amiga

Unlike the 8-bit Commodores the extra buttons on the Amiga are active
low, and unlike all the 8-bit computers they are digital inputs instead
of repurposed paddle circuits.  The Atari mode supports both Atari and
Amiga by driving the pins for buttons two and three both high and low.

### Atari ST (untested)

Button two registers as the second mouse button / port 1 joystick button
if the adapter is plugged into port 0.  The ST does not support
additional joystick buttons on port 1.


## Controller compatibility
### Sega controllers

The adapter is compatible with three- and six-button Mega Drive
controllers. On six-button controllers, buttons X, Y, and Z act as
autofire versions of A, B, and C, while the Mode button is only used for
[SNACK enhanced mode](#snack-enhanced-mode).

Sega Master System controllers are theoretically supported, including
the second button, but this is untested.

### 8BitDo Retro Receiver

The adapter is extensively tested with the [8BitDo Retro Receiver for
SEGA](https://www.8bitdo.com/retro-receiver-genesis-mega-drive/).  It's
a nifty way to use wireless controllers on old hardware.

### Other joysticks or controllers

A controller or joystick with a second button that is not normally
recognised by 8-bit Atari and Commodore computers _may_ work through the
adapter.

**CAUTION:** the second button must be on pin 9 and _not_ pin 5.  Pin 5
is the power pin, and pulling it low may damage your computer or the
adapter.

Single-button joysticks will work but derive no benefit from using the
adapter.

### Atari 7800 controller, Commodore multi-button joysticks, paddles, steering wheels, etc.

These devices will not work and should not be plugged in to the adapter.


## Build

![PCB MEGA 65](img/Sega%20Adapter%20_%20PCB%20MEGA65%20version.jpg)
The schematics and parts list for the adapter are in the [kicad
directory](KiCad).  If you don't already have a preferred way to
manufacture a PCB, you can order a set of three [original PCB](https://github.com/eyvind/sega-adapter) from OSH
Park.  You also need a
way to [program the microcontroller](#program).


## Program

[JOY-2-PIC](https://ataribits.weebly.com/joy2pic.html) can program a PIC
using an Atari 8-bit computer's joystick port.  Boot from the .atr image
attached to each release to start programming.

If you have a USB PIC programmer connected to a PC, use the .hex file instead.

![T48](img/T48%20program%20PIC16F1847.jpg)
For XGecu T48 programmer (TL866 3rd generation) select PIC16F1827 uncheck "Check ID"
![T48 PIC16F1827](img/Program%20PIC16F1847%20setup%20PIC16F1827%20on%20T48.jpg)

## Modify

If you want to modify the firmware, you need the [xc8
compiler](https://www.microchipdeveloper.com/xc8:installation) from
Microchip to build it.  Run `rake` or `xc8-cc -mcpu=16f1847 -o
sega-adapter.hex *.c` to create a .hex file suitable for a PIC
programmer.


## Rationale

All right, it's time for some history.

- In 1977, Atari released the 2600 game console.
- In 1981, Commodore released the VIC-20 home computer.
- In 1984, Sega released the SG-1000 II console with one main
  improvement from the SG-1000: detachable controllers.

These machines all used the same physical connector with an almost
identical pinout for their controllers, and their manufacturers stuck
with the standard through later generations, tempting people to swap
controllers and joysticks between the systems.

Most of the time, this more or less worked.  Buttons were not always
completely compatible, but most Atari and Commodore games only supported
one button so it didn't really matter as long as one button worked.  One
important caveat: Sega chose to put power on pin 5 instead of pin 7, so
mixing and matching had the potential to cause damage to both machines
and controllers.

Initially each signal was carried by an individual pin: four pins for
directions, one for the button, two pins reserved for power and ground,
leaving two pins free for other functions.

### Mega Drive (Genesis)

For the Mega Drive, released in 1988, Sega wanted to have more buttons
(four) than there were available pins on the controller port (three).
They solved this problem by using one pin (7) as an output from the
console to the controller, allowing two pins (6 and 9) to multiplex the
A, B, C, and Start buttons.

### Joy2B+

Fast forward 20 years, to January 2019.  A poster on an [Atari 8-bit
forum](https://atariage.com/forums/topic/278884-2-button-joystick/?do=findComment&comment=4206381)
introduced a design for a three-button joystick for the 8-bit Ataris,
and -- crucially -- included patched versions of existing games that
supported the extra buttons.  This was the
[Joy2B+](https://github.com/ascrnet/Joy2Bplus) project.

Suddenly, there was a reason to connect a joystick with more than one
button to an Atari.

### 8BitDo Retro Receiver for SEGA

Finally, a month later, a Hong Kong-based company called 8BitDo released
a device that allowed Bluetooth game controllers to be used with Sega
consoles, the [8BitDo Retro Receiver for
SEGA](https://www.8bitdo.com/retro-receiver-genesis-mega-drive/)...
tantalisingly close to being compatible with Atari joystick ports.


## Pinout

```
                      PIC 16F1847
                     +---------_---------+
                     |                   |
            Atari 5 -|  1  RA2   RA1  18 |- Atari 1
                     |                   |
            Atari 6 -|  2  RA3   RA0  17 |- Atari 2
                     |                   |
            Atari 9 -|  3  RA4   RA7  16 |- Atari 3
                ___  |                   |
                C64 -|  4  RA5   RA6  15 |- Atari 4
                     |                   |
    Atari 8  SEGA 8 -|  5  GND   +5v  14 |- Atari 7  SEGA 5
                     |                   |
             SEGA 1 -|  6  RB0   RB7  13 |- SEGA 6
                     |                   |
             SEGA 2 -|  7  RB1   RB6  12 |- SEGA 7
                     |                   |
             SEGA 3 -|  8  RB2   RB5  11 |- SEGA 9
                     |                   |
             SEGA 4 -|  9  RB3   RB4  10 |- X
                     |                   |
                     +-------------------+
```


## Acknowledgements

Information on reading Sega controllers came from
https://github.com/jonthysell/SegaController/wiki/How-To-Read-Sega-Controllers
and
https://www.raspberryfield.life/2019/03/25/sega-mega-drive-genesis-6-button-xyz-controller/
