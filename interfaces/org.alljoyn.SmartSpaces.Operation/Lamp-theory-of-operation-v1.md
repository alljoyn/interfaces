## Lamp Theory of Operation version 1

This theory of operation explains the interaction of various interfaces to
assemble a lamp with varying functionality.


### Overview

The definition of a lamp is any type of single lighting device such as a light globe.
The Common Device Model service framework defines a common set of standard interfaces that work
across multiple device types. This document illustrates how to combine a subset
of those interfaces to monitor and control different types of lighting functionality.


### A Simple System

#### org.alljoyn.SmartSpaces.Operation.OnOffStatus

This interface provides the capability to monitor the on/off power status of
the lamp. The use of only this interface could coincide with the presence of a
physical switch that controls the lamp's power so only monitoring functionality is exposed.

#### OnOffStatus Combined with the OnControl and OffControl interfaces

The interfaces ***org.alljoyn.SmartSpaces.Operation.OnControl*** and
***org.alljoyn.SmartSpaces.Operation.OffControl*** provide functionality to fully
control the power operation of the lamp. A lamp with or without a physical switch
may want to provide this combination as a means to remotely turn lights on or off
or alter the power status driven by some other event.


### Some More Complex Systems

#### org.alljoyn.SmartSpaces.Operation.Brightness

Some lamps are made with the ability to have multiple brightness levels. This interface
exposes functionality that directly sets a lamp's brightness. A non colored but
dimmable lamp may want to use this interface.


#### org.alljoyn.SmartSpaces.Operation.Color

Some lamps are made with the ability to alter their color. This interface exposes
functionality to directly set both the hue and the saturation for the lamp. A use
case where only the Color interface is used is for a lamp that is acting as part
of a monitoring service. Depending on the state of what is being monitored, the
lamp might shift its hue from green to red or some other user defined color.

#### Combining Brightness and Color to create a complete HSV (hue, saturation and value) color model

The combination of both ***org.alljoyn.SmartSpaces.Operation.Color*** and
***org.alljoyn.SmartSpaces.Operation.Brightness*** provide the means to express
the color of the lamp using the HSV color model. This color model provides an intuitive 
representation of color in that the hue value represents the actual color and the saturation 
is amount of that color relative to the brightness. The documentation for ***org.alljoyn.SmartSpaces.Operation.Color***
provides a more in depth explanation of how the Color and Brightness interfaces combine to form the HSV model.
