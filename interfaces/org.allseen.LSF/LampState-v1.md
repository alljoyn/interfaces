# org.allseen.LSF.LampState version 1


## Theory of Operation
This interface is provided by the LSF Lamp Service to enable the LSF Lamp
Clients to access and control the state of a Lamp.

Lamp State is the state information that the Lamp persists through reset or power 
cycle that includes End-User set state attributes such as Hue, Saturation, Color Temperature, 
and Brightness. We need to persist the state to ensure that the Lamp comes back ON at the
same state that it was it before the power cycle/reset.

The interface is not secure.

## Specification

|              |       				|
|--------------|--------------------------------|
| Version      | 1     				|
| Annotation   | org.alljoyn.Bus.Secure = false |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The interface version number.

#### OnOff

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | bool                                                     |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates whether the lamp is on/off.

#### Hue

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The hue of the Lamp

#### Saturation

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The saturation of the Lamp

#### ColorTemp

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The color temperature of the Lamp in Kelvin

#### Brightness

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The brightness of the Lamp

### Methods

#### TransitionLampState (Timestamp, NewState, TransitionPeriod) -> (LampResponseCode)

This method allows the LSF Lamp Clients to transition the state of a lamp. 

Input arguments: 

  * **Timestamp** --- uint64 --- the timestamp to start the transition at.
  * **NewState** --- key-value pairs --- the new state to transition to. The keys are the property names in the [LampState interface](LampState-v1.xml) interface.
  * **TransitionPeriod** --- uint16 --- the transition period in milliseconds.

Output arguments:

  * **LampResponseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 11    | The arguments were invalid                                        		|

#### ApplyPulseEffect (FromState, ToState, period, duration, numPulses, timestamp) -> (LampResponseCode)

This method allows the LSF Lamp Clients to pulse the state of a lamp. 

Input arguments: 

  * **FromState** --- key value pairs --- the fromState of the Pulse Effect. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.
  * **ToState** --- key value pairs --- the toState of the Pulse Effect. The keys are the property names 
                                        in the [LampState interface](LampState-v1.xml) interface.
  * **period** --- uint16 --- the Pulse period in milliseconds.
  * **duration** --- uint16 --- the Pulse duration in milliseconds.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **timestamp** --- uint64 --- the NTP timestamp to start the transition at.

Output arguments:

  * **LampResponseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|

### Signals

#### LampStateChanged -> (lampID)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampStateChanged signal is used to notify that the Lamp State has changed.

Output arguments:

  * **lampID** --- string --- the Lamp ID.

## References

  * The XML definition of the [LampState interface](LampState-v1.xml)


