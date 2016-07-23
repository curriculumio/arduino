# Outlet Relay
## Requirements
### Knowledge
1. [Basic Cicuitry](http://curriculum.io/arduino/basic-circuitry)


### Hardware
1. Arduino Uno
2. Relay Module with Breakout Board
3. Extension cord (1 - 2 feet preferable)
4. 2 wire caps
5. Wire strippers (with built in clipper and crimper optional)
6. (Optional) Clipper
7. (Optional) Crimper
8. Additional project ideas may require more components

### Software
1. Arduino 1.6.9 or Higher

## The Build

A relay is a device that can control circuits with large loads and voltage with logic-level voltages. This allows a microcontroller like Arduino to control powerful circuits that would otherwise fry the Arduino. The idea of gain is important here as a relay allows a relatively small voltage to control an arbitrarily large circuit. Think of a relay as an logic controllable switch.

The first step is to hack the extension cord to insert the relay. Start by cutting the extension cable in half with the wire stripper's clipper section. The inner wires now need to be exposed. To do this make a small indentation about 3 inches into the extension cord's exposed end. Using a pulsing motion clamp the clippers to cut into the rubber or plastic making sure not to go deep enough to cut the inner wires. Rotate the clippers, continuing to make the pulsing cutting motion. This should allow the insulation to slide off the cord when pulled. Repeat this process for the other half of the extension cord.

Once the black, green, and white wires are exposed use the same technique described previously to strip each of the leads. This should expose a mesh of copper wire at the tip of each lead. The white wire is "neutral", the black wire is "live", and the green wire is "ground". This is a standard convention in AC current.

The green and white wires need to reattached with the wire caps. Start with the green wire. Split the copper ends into two bundles on each corresponding green wire. Twist together one of the two bunles on each of the green wires. Twist together the other two bundles. Take these two, connected, twisted bundles and twist them together. Use the crimper to twist the new single bundle into a "J" shape and then crimp this. Place a wire cap onto this lead and twist until it has no more give. Refer to the video to clarify this technique. Repeat the process for the white wire.

The next step is to feed in the leads of the black wires into 

