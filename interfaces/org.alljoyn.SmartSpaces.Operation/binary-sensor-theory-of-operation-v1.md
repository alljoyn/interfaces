## Binary Sensor Theory of Operation version 1

This theory of operation explains the interaction between interfaces that are common to sensing devices.

### Overview

A sensor is defined as a device that detects a change in its environment and responds to it. At a minimum, a sensor must implement TriggerSensor. This 
is so it can effect a minimal response by emitting a signal when changing its internal state.

### A Basic Sensor

#### org.alljoyn.SmartSpaces.Operation.TriggerSensor

This interface exposes the capability to monitor whether the sensor has detected the change that it is responding to.

### More complex sensors

#### A sensor with an alarm mechanism

An alarm mechanism, in this case, is a physical method for drawing attention, e.g., an audible alarm, flashing lights etc.  

The _TriggerSensor_ should be separate from the alarm mechanism. This means that hushing the alarm, e.g., via a physical button, does not stop the
sensor reporting the state of its environment.

#### org.alljoyn.SmartSpaces.Operation.Alerts

This interface can be used to raise alerts generated by the sensor. The _Alerts_ interface can be used to emit a _warning_ or an _alarm_ when 
the state of _TriggerSensor_ changes. This interface can also be used to emit a _fault_ alert for any detected hardware failures.

#### Interaction between a physical alarm mechanism and org.alljoyn.SmartSpaces.Operation.Alerts

Once the sensor has triggered and the alarm mechanism is active, an _alarm_ alert is emitted by the _Alerts_ interface.
The emitted _alarm_ alert can then be acknowledged as a mechanism to hush the physical alarm. Hushing the alarm via the emitted alert has the same 
behavior as hushing the alarm via a physical method.

If the alarm is hushed via a physical method, then a _warning_ should be emitted via the _Alerts_ interface that the alarm has been hushed.

#### A Battery powered sensor

Please refer to the [BatteryPowered](batter-powered-theory-of-operation-v1) theory of operation.

#### org.alljoyn.SmartSpaces.Operation.RemoteControllability

This interface provides the capability for a sensor to be have its remote controllability enabled or disabled. 
When RemoteControllability is disabled, all remote functionality that modifies or controls the sensor will return an error.