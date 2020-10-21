# FPGA-Candy-Crush
"Candy Crush" matching game implemented in C on the DE1 SoC FPGA Board.

![FPGA Trumpet](./demo.png)

Made in cooperation with Cecilio Carelo.

## Set Up
- This was made on ARMv7 DE1-SoC (version 16.1) of CPUlator.
A link has been provided your convenience. Copy
and paste this file and compile in C and press continue to run.
https:cpulator.01xz.net/?sys=arm-de1soc&d_audio=48000
- In the bottom left settings tab, scroll down to the bottom of the window.
Under "Settings that require reloading" change "Preallocated memory"
to 16 MB or greater or else it will not run properly. 
- To use a mouse on CPUlator, go onto the right and scroll down to
"PS/2 keyboard or mouse .... IRQ 79 ff200100" and click the arrow pointing
downwards to open the drop down menu. Select mouse. After running the program,
you can click on the capture button to allow the game to track your mouse
movements
- For optimal experience, it is recommended to enable “show in a separate box” for the seven segment
displays, VGA pixel buffer and the PS/2 mouse and keyboard mentioned above.

## What it does
- Set the amount of moves you want at the very top #define MOVES. It is 15 by default.
- The hex displays your moves on the left and score on the right. The max
score is 999.
- Left click is used to select a tile.
- Unlike regular match 3 games, where you can only swap with adjacent tiles,
Our version allows you to swap one tile with any other tile in a row
or column through adjacent swaps.
- Each time you match something with 3 or greater size, you will get points
equal to tiles matched.
-Has sound effects for popping and dropping of tiles.


## How we built it
- The first step was to get the sprites drawing properly, so we created some miminmalistic sprites to test out drawing to the VGA pixel buffer
- After this was complete we continued on the game logic and made sure that the visuals matched the logic that was happening in the game using the simulated PS2 keyboard.
- Then we started to implement other devices on the DE1-SOC board to add additional features: PS2 Mouse to control cursor, Audio Buffer to create SFX for tile movement, Seven Segment Display to keep track of score
- Lastly we polished our sprites by making better ones and implemented pseudo animation to make the game more fluid (tiles swapping and tile dropping)

## Challenges
- The most difficult part was the audio buffer implementation. The audio buffer is small relative to the sound effects, and since we wanted sound effects to match the tiles moving, we were writing audio data to the buffer as frequently as tiles moved which would overflow it and cause mismatched audio. The solution to this was to trim our sound effects along with speeding them up (compressing them) and experiment if this would cause any audio problems with the audio buffer overflowing, which in the end did not.

## Notes
- You will get this warning during the first compile of the code:
"Device-specific warning: VGA buffer swap was requested, but both buffers
are the same (c8000000). If you are using double-buffering, make sure buffer
pointers are initialized properly. Simulator requested a breakpoint."
Press continue, as this has no impact on functionality of the code.
- Do not move your mouse too much after clicking while game is running. If you do
this may cause a FIFO overflow, as the CPUlator mouse always outputs to FIFO buffer
even when sent the 0xF5 (Disable Data Reporting) code. If it does overflow,
press "continue / f3" again, as the overflow has no impact on the actual
functionality of the code.
- There may be delays in audio. This is a CPUlator issue. In the CPUlator documentation:
"The long FIFOs result in high latency. In the simulator,
sounds will typically be delayed by about 300-600 milliseconds due to buffering,
while the delay is just a few milliseconds in hardware. Large buffers help
mitigate the high jitter when running in a web browser."

## Citations
SFX
- xtrgamr. Tones of Victory. June 26th, 2015. Available from: https:freesound.org/people/xtrgamr/sounds/277441/
- Fupicat/. Videogame Menu Select. May 22nd, 2019. Available from: https:freesound.org/people/Fupicat/sounds/471937/
- luke_harris_01. Swap SFX-1. March 6th, 2019. Available from: https:freesound.org/people/luke_harris_01/sounds/462643/
- InspectorJ. Pop, Low, A (H1). November 23rd, 2017. Available from: https:freesound.org/people/InspectorJ/sounds/411639/

Converted using WAVToCode:
-Colin J.S. A WAV File to C Code Converter. February 28th, 2011. Available from: http:colinjs.com/wavtocodeV1_0/wavtocode.htm

Pixel art text/numbers:
-Pixilart, Pixilart - Free online pixel art drawing tool. 2020. Available from: https:www.pixilart.com/

Converted using:
-digole.com, Picture to C Hex converter. Available from: https:www.digole.com/tools/PicturetoC_Hex_converter.php

Code was used from Appendix 9.5 (audio) and Appendix 9.7 PS/2 Controller
Intel Corporation - FPGA University Program, DE1-SoC Computer System with ARM Cortex A9 for Quartus® Prime 17.1. [PDF]. November 2017.
