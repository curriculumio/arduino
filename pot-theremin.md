# Potentiometer Theremin
## Requirements
### Knowledge
1. [Basic breadboard and circuits](curriculum.io/arduino/basic-circuitry)

### Hardware
1. Arduino
2. Programming cable
3. 1k$\Omega$ resistor
4. 10k$\Omega$ potentiometer
5. 8$\Omega$ speaker
6. Jumper cables

### Software
1. Arduino 1.6.9 or higher

## The Build
Start by wiring `5V` to the power rail on the breadboard and `GND` to the ground rail. Consider this the basic skeleton of most of the projects outlined. 

Break out a line from power to one of the rows on the breadboard. Take a jumper and connect `A0` from the bottom left header on the Arduino to 2 rows below the row used for power. Break out a line from ground to the row 2 below that. Wire the positive terminal of the potentiometer to the power row. Wire the output signal of the potentiometer to the `pin A0` row. Finally, wire the negative terminal of the potentiometer to the ground row. The setup should match the following diagram.
![potentiometer connections](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/pot-theremin/potentiometer_to_arduino.png?raw=true)
The next step is to wire the speaker to a pin. Break out a line from ground to a new row on the breadboard. Use a jumper to connect `pin 13` to a nearby row. Wire a resistor going from this row into a new row. Finally, connect the negative terminal of the speaker to the new ground row and the positive terminal to the `pin 13` row. Since this resistor is used as a volume control, experiment with a few resistor values to find the best volume. Alternatively, use a potentiometer as a variable resistor to have an adjustable volume. Both diagrams are provided below.

Fixed Volume:
![fixed volume](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/pot-theremin/potentiometer_with_speaker.png?raw=true)

Adjustable Volume:
![adjustable value](https://github.com/curriculumio/curriculumio.github.io/blob/master/image/arduino/pot-theremin/pot_volume_control.png?raw=true)


## Making the Sketch
Start by making the basic skeleton `void setup()` and `void loop()`. This sketch will use the native `tone()` function to produce tones of a certain frequency. It will also use the native `analogRead()` function to read the voltage coming off the potentiometer. The difference between analog and digital will be further detailed in the lab.

## Defining Variables
Define an `int` constant `SPEAKER` to store which pin the speaker is connected to. Define another constant `POTENTIOMETER` to store the analog-in pin connected to the potentiometer.
```c
#define SPEAKER 13
#define POTENTIOMETER A0
```

## Defining Functions
In `void setup()`, set `SPEAKER` as an output. A0 is already defined as an input by default, so that doesn't have to be changed. Again, begin `Serial` at a baud-rate of `9600` for debug purposes.
```c
void setup() {
	Serial.begin(9600);
	pinMode(SPEAKER, OUTPUT);
}
```

The `void loop()` function makes use of `analogRead()`, so it is important to have a clear understanding of the difference between analog and digital. `digitalRead()` reads two voltage levels `HIGH` and `LOW` with no in-between values. There is some wiggle room, but in accordance with static discipline these should be small. If voltage drops, but not enough to be in a `LOW` state, digital devices will not work properly. `analogRead()`, however, makes full use of the range of voltages present between `HIGH` and `LOW`.

The potentiometer is a variable resistor. Recalling that a resistor drops the current in a circuit, a potentiometer set to be at full resistance would drop the voltage more than the same potentiometer set to be at minimal resistance. Reading the voltage with `analogRead()` provides a way to detect what position the potentiometer is in. This information can be used to drive an action with the Arduino. In the case of this project, the action will be generating a tone from a speaker.

The native `tone()` function generates a tone on a pin designated in an `int` argument at the frequency designated in another `int` argument. A higher frequency indicates a higher pitch.

`loop()` will repeatedly read values from the `POTENTIOMETER` pin and generate tones based on those values. Use the native `map()` function to map the potentiometer readings to a more listenable musical interval.  `map(int freq, int oldLow, int oldHigh, int newLow, int newHigh)` takes values in a range (`oldLow` to `oldHigh`) and scales them to a new range (`newLow` to `newHigh`). A small delay is also needed to ensure the accuracy of the readings.
```c
void loop() {
	int freq = analogRead(POTENTIOMETER);
	freq = map(freq, 0, 1023, 880, 110);
	tone(SPEAKER, freq);
	delay(10);
}
```
All together the sketch should match the following.
```c
#define SPEAKER 13
#define POTENTIOMETER A0

void setup() {
	Serial.begin(9600);
	pinMode(SPEAKER, OUTPUT);
}

void loop() {
	int freq = analogRead(POTENTIOMETER);
	freq = map(freq, 0, 1023, 880, 110);
	tone(SPEAKER, freq);
	delay(10);
}
```
