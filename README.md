# MicroManagerArduino
Arduino Nano Every, pin-layout and firmware for use with MicroManager.

See also: https://micro-manager.org/wiki/Arduino and https://store.arduino.cc/arduino-nano-every 

----


So, this took a bit more work than I expected. Turns out the port mapping on a Nano Every is very different from the regular Arduino, specifically because it has many more registers with fewer pins per register.

This meant that I could not simply pick a new register to use and let that be it, since you’d get at most three digital outputs instead of six (with the exception for the D register, but that happens to be the only analog input register and I did not want to sacrifice it). The B register (with three digital output pins available) overlaps with the few PWM-controlled analog output pins, and I did not want to sacrifice those either, and the E register overlaps with the build-in LED. It’s a right mess.

Instead, I mapped outputs via a function to several registers in sequence. I expect this will slow down execution somewhat, though since the ATMEGA4809 is a good deal faster than the chip used in the original Arduino, maybe not by much. I cannot measure it here, and at least for my application it does not matter.

On the upside, since I was able to pick a new pin layout, I’ve simply ordered the digital pins in sequence as they appear on the board, so it’s a lot easier to remember where each subsequent pin goes.

For my purposes, I wanted to use the PWM-outs to control an analog device directly (well, through a simple RF circuit) so the Analog-out for DAC1 and DAC2 are PWM-controlled in this version, instead of controlling a TLV5618.

From what I can test, the digital outs and the PWM-outs work as expected. I don’t know whether anyone uses the input trigger or analog inputs, they should work, but I cannot test them so YMMV.
