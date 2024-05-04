# PulseCaptureMonolithic
Monolithic versions of PulseCapture that target a single purpose with minimal footprint.

## IrMega45
This captures NEC IR signals on pin 45 of an Arduino Mega.  It takes over Timer5,
to which pin 45 is hard wired for hardware pulse capture.  Capture takes place completely using Timer5's interrupts.

Calling ```begin()``` is optional.  IR detection is active upon the first call to ```begin()``` or ```read()```.

When IR detection is active, it happens completely within interrupt handlers.  Incoming IR messages are buffered.

Calling ```read()``` returns a 32 bit unsigned integer, returning and emptying the read buffer.  Return value is 0 if nothing received, 1 if the one-bit "repeat" code was received (indicating a button is being held down), or otherwise, this will return a 32-bit value containing the Address and Command of the button message received.

The 32-bit value will have the Command field as the least significant byte, and the Address field as the most significant 16 bits.  Only a single message is buffered at a time, and is overwritten if a new IR message arrives before the first message is read (however, the "repeat" code, if received, will not be allowed to overwrite the buffer, and will be ignored if there isn't room for it)

Calling ```stop()``` pauses all receiving and interrupt handling until either ```begin()``` or ```read()``` is called again.
