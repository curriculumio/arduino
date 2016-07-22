# Keyboard Instrument
# Requirements
## Knowledge
1. [Basic Circuitry](http://curriculum.io/arduino/basic-circuitry)
2. [LED Lab](http://curriculum.io/arduino/binary-counter)

## Hardware
1. Arduino Uno
2. Programming cable
2. Full-size breadboard
3. (Optional) mini breadboard
2. 5-8 momentary tact switches
3. 5-8 1k $\Omega$ resistors
4. 22-24 gauge red, black, yellow, and green wire
5. Jumper cables
6. 8 $\Omega$ speaker

## Software
1. Arduino 1.6.9

# The Build
## Button
The first step to building a keyboard is to wire a button on the breadboard. The first implementation will be with jumper cables, and the second implementation will be with flat, clean wiring so that the button is easily accessible when played.

Start by wiring power and ground from the Arduino to the breadboard. This is achieved by wiring a jumper from the left side header on the Arduino `5V` pin to the red rail on the breadboard, and wiring the `GND` pin from the same header to the blue rail of the breadboard.

The momentary tact switch used may have 2 to 4 pins depending on style, but for this tutorial a 4-pin tact switch will be assumed. Install the button across the center divide of the breadboard with two pins on either side. At this point your board should match the following diagram.

![power, ground, and button](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/simple-button/button.png?raw=true)

Wire a jumper from the power rail to the lower left pin on the button. When the button is pressed, it will connect this pin to one of the two pins on the opposite side. To test which pin it connects to, connect a jumper cable to one of the opposite pins. Wire the other end of the jumper to the power side of an LED on your breadboard, and insert the ground side of the LED in the ground rail. The setup should look as follows.

![testing pins](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/simple-button/button_with_LED_test.png?raw=true)

Use the LED jumper to find the pin on the button that turns the LED on when the button is depressed and turns off otherwise. This is usually diagonally across from the button pin that power is wired to.

Now the button is functional for simple electric applications like turning an LED on and off. However, in order to use it as a digital input to the Arduino for program control, it needs a static reference to ground.

The first knee-jerk reaction to getting a reference to ground would be to wire a jumper from the connecting pin to ground. This gives a reference to ground, but it has a bigger problem of short circuiting the board when the button is depressed. This is because it gives the electricity direct access to ground without any resistance.

To address this problem, instead wire a resistor from the connecting pin to ground. Now take a jumper from the connecting pin and wire it to a digital pin on the Arduino. This gives a static reference to ground when the button is not depressed, but will prevent a short circuit when the button is depressed. 


Wire the jumper coming from the connected side to `pin 13` on the right side header of the Arduino. The setup should match the following diagram.

![completed button](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/simple-button/button_with_resistor.png?raw=true)

Use this sample program to test the button.
```c
#define BUTTON 13;

void setup() {
	Serial.begin(9600);
	pinMode(BUTTON, OUTPUT);
}

void loop() {
	int readValue = digitalRead(BUTTON);
	if (readValue == HIGH) {
		Serial.println("ON");
	} else {
		Serial.println("OFF");
	}
	delay(10);
}
```
After uploading this code to the board, open the Serial by clicking the magnifying glass in the top right corner of the Arduino software. When the button is depressed, the `Serial` should read "ON". When the button is not depressed, the `Serial` should read "OFF".

## Flat-wiring

In order to make the button easily accessible for playing, flat-wiring is necessary to keep the project from turning into a tangle of wires. Power and ground should also be connected to the unconnected power and ground rails on the other side of the breadboard to allow access from both sides. 

Connecting the rails is the first introduction to the flat-wiring technique. Unspool a length of 22 to 24 gauge red wire without cutting it from the spool. Lay it flat against the board from the connected power rail to the unconnected power rail. Leave some overhang in the wire on either side of the rails and carefully mark the part closer to the spool where it will be cut.

After the wire is cut to length, lay it on the breadboard and use the hole spacing to measure three holes in from either side of the wire and mark. Strip the wire from these marks so you have short leads on either side of the wire. Bend these at $90^o$ angles in the same direction. The wire should resemble a long staple. Insert these ends into the power rails on either side of the breadboard, and push the whole thing in at the same time. Repeat for the ground rail with black wire. (See the first image in the next section for visual reference.)

## Flat-wiring a Button

Remove the existing jumpers, other than the main jumpers from power and ground, from the board if not already done. Use the flat-wiring technique to create the shortest possible connection from power to the lower left hand pin of the button using red wire. Cut the legs from a 1k$\Omega$ resistor so that it lies flush with the board when inserted. Place one end in the right side ground rail and the other end into the connected pin of the button found earlier.

Instead of using a jumper to connect the button straight to the Arduino, flat-wire a connection from the pin with the resistor on it to the very top of the breadboard. The top three rows of the breadboard will be used as a launching point for jumper cables going to the Arduino. This way, all of the messy wiring will be restricted to a small area. Wire a jumper from the flat-wired button extension at the top of the board to `pin 13` on the Arduino. The setup should now match the following diagram.

![single button flat-wired](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/button-keyboard/single_button_with_resistor.png?raw=true)

Repeat the steps for the remaining 4 buttons, connecting them to pins `12` through `9` on the Arduino. The following diagram will be helpful to organize the placement of the wires.

![all buttons flat-wired](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/button-keyboard/all_buttons.png?raw=true)

The wiring pattern was chosen to minimize the number of overlapping wires. Notice that the last two buttons are wired in the opposite direction as the first three, with the resistor to ground on the left side of the button and the power supply on the right side.

## Wiring the Speaker

Install the 8$\Omega$ speaker with the positive lead in one row and the negative lead in another row at the bottom of the board or in a small, separate breadboard. Take a jumper from `pin 8` and wire it to the left side of the top of your breadboard, one row down from the other jumpers on the left. Measure out a flat wire to jump this row to the bottom of your breadboard. Then, wire it either directly to the positive lead of the speaker on the breadboard, or use a jumper to connect it to the lead on the small breadboard.

The setup should match the following diagram if wired to a small breadboard, but should be electrically equivalent if wired on the same board.

![completed keyboard](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/button-keyboard/button_keyboard_complete.png?raw=true)

# Making the Sketch

Start by creating the basic skeleton of the sketch (`void loop()` and `void setup()`). This sketch will make use of the native Arduino `tone()` function to drive the speaker, and the `digitalRead()` function to get values from the buttons. `tone()` takes two parameters: the first is an `int` argument defining which pin to output the tone onto, and the second is another `int` argument defining the frequency of the tone being played. A higher frequency results in a higher pitch. The sketch will also use the mathematical formula for turning musical intervals into frequencies, which will be expanded further in the lesson.

## Defining Variables
Define a constant `int` `SPEAKER` to store the pin that the speaker is connected to. Define another `int` constant that stores the starting frequency of the keyboard (use `220`, which translates to an "A2"). Create an `int` array `buttons` to store the pins connected to the buttons.
```c
#define SPEAKER 8
#define START_TONE 220
int buttons[5] = {9,10,11,12,13};
```
## Defining Functions

In `void setup()`, use `pinMode()` to set `SPEAKER` to `OUTPUT`. Use a for-loop to set the pins in `buttons` to `INPUT`. Also begin the `Serial` at the baud-rate of `9600` for debug purposes.
```c
void setup() {
	Serial.begin(9600);
	pinMode(SPEAKER, OUTPUT);
	for (int i = 0; i < 5; i++) {
		pinMode(buttons[i], INPUT);
	}
}
```
Before actually playing a tone, make sure your buttons can read values correctly. Inside `void loop()`, use `Serial` to print the value of the `getKey()` function. `getKey()` does not yet exist, but it will eventually return an `int` which represents the first position in the `buttons` array whose button is depressed.
```c
void loop() {
	Serial.println(getKey());
}
```
To implement the `getKey()`function, use the native function `digitalRead()` to check the state of each pin in `buttons`. Check each pin, and if one is `HIGH`, return the index of that pin. If no pin in `buttons` is high, return a $-1$ to indicate that no button is currently pressed.
```c
int getKey() {
	for (int i = 0; i < 5; i++) {
		int readValue = digitalRead(buttons[i]);
		if (readValue == HIGH) {
			return i;
		}
	}
	return -1;
}
```
Upload the sketch to the board. The complete sketch as it is now is listed below.
```c
#define SPEAKER 8
#define START_TONE 220
int buttons[5] = {9,10,11,12,13};

void setup() {
	Serial.begin(9600);
	pinMode(SPEAKER, OUTPUT);
	for (int i = 0; i < 5; i++) {
		pinMode(buttons[i], INPUT);
	}
}

void loop() {
	Serial.println(getKey());
}

int getKey() {
	for (int i = 0; i < 5; i++) {
		int readValue = digitalRead(buttons[i]);
		if (readValue == HIGH) {
			return i;
		}
	}
	return -1;
}
```
Test to make sure that the values are printing properly. It should print a $-1$ if no buttons are depressed, or $0$ through $4$ depending on the button being pressed. Once satisfied, continue with developing the sketch.

Comment out the `Serial.println(getKey());` using `//` and in its place write `play(getKey())` to play the given key instead of printing it. The next step is to implement the `play()` function.

The mathematical formula for getting frequencies based on a start tone and musical interval is called equal-tempered tuning. It uses the fact that a note one octave (or 12 half-steps) above has a frequency that is exactly double the frequency of the note an octave below it. All the notes in between are therefore equally tempered on an exponential scale. The formula described above is $T*2^{H/{12}}$, where $T$ is the starting tone (in this program, $T$ is `220`) and $H$ is the number of half-steps (or semitones) away from the starting tone.

Programmatically, raising $2$ to some power uses the native `pow()` function with the argument $2$ and another argument representing the power to which $2$ is raised. For example, the call `pow(2, 3)` yields the result $8$.

The `void play()` function should take an `int key` as an argument, and should play a tone based on the value of `key`. For now, assume that each key (represented by the buttons) is a half-step away from its adjacent keys before devising a way to assign each key to, say, a major scale position. `play()` should check whether `key` is $-1$ and should call `noTone(SPEAKER)` if it is. Otherwise, determine an `int note` to store the frequency of the note calculated using the equal-tempered tuning formula. Then call `tone(SPEAKER, note)` and add a 10ms delay.
```c
void play(int key) {
	if (key == -1) {
		noTone(SPEAKER);
	} else {
		int note = START_TONE * pow(2, key / 12.0);
		tone(SPEAKER, note);
		delay(10);
	}
}
```
A keyboard that only plays 5 half-steps is rather dull. Instead, create a function `getStep()` that takes an argument `int key` and returns a scale position in half-steps from the start tone. Use switch-case to implement this. The implementation listed below uses the pentatonic minor scale, but any scale (major, minor, diminished, Dorian, whatever you want!) can be implemented.
```c
int getStep(int key) {
	int pos;
	switch(key) {
		case 0: 
			pos = 0; 
			break;
		case 1: 
			pos = 3;
			break;
		case 2: 
			pos = 5;
			break;
		case 3:
			pos = 7;
			break;
		case 4:
			pos = 10;
			break;
	}
	return pos;
}
```

The only change left to be made is to replace `key` in the line `int note = START_TONE * pow(2, key / 12.0);` (the even-tempered calculation) with `getStep(key)`. The final sketch should match the following.
```c
#define SPEAKER 8
#define START_TONE 220

int buttons[5] = {9,10,11,12,13};

void setup() {
	Serial.begin(9600);
	pinMode(SPEAKER, OUTPUT);
	for (int i = 0; i < 5; i++) {
		pinMode(buttons[i], INPUT);
	}
}

void loop() {
	//Serial.println(getKey());
	play(getKey());
}

int getKey() {
	for (int i = 0; i < 5; i++) {
		int readValue = digitalRead(buttons[i]);
		if (readValue == HIGH) {
			return i;
		}
	}
	return -1;
}

void play(int key) {
	if (key == -1) {
		noTone(SPEAKER);
	} else {
		int note = START_TONE * pow(2, getStep(key) / 12.0);
		tone(SPEAKER, note);
		delay(10);
	}
}

int getStep(int key) {
	int pos;
	switch(key) {
		case 0: 
			pos = 0; 
			break;
		case 1: 
			pos = 3;
			break;
		case 2: 
			pos = 5;
			break;
		case 3:
			pos = 7;
			break;
		case 4:
			pos = 10;
			break;
	}
	return pos;
}
```
