# org.allseen.LSF.LampState version 1


## Theory of Operation
This interface is provided by the LSF Lamp Service to enable the LSF Lamp
Clients to access and control the state of a Lamp.

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

The color temperature of the Lamp

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
  * **NewState** --- key-value pairs --- the new state to transition to.
  * **TransitionPeriod** --- uint16 --- the transition period.

Output arguments:

  * **LampResponseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The last Lamp response code                                         		|

#### ApplyPulseEffect (FromState, ToState, period, duration, numPulses, timestamp) -> (LampResponseCode)

This method allows the LSF Lamp Clients to pulse the state of a lamp. 

Input arguments: 

  * **FromState** --- key value pairs --- the fromState of the Pulse Effect.
  * **ToState** --- key value pairs --- the toState of the Pulse Effect.
  * **period** --- uint16 --- the Pulse period.
  * **duration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **timestamp** --- uint64 --- the timestamp to start the transition at.

Output arguments:

  * **LampResponseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The last Lamp response code                                         		|

### Signals

#### LampStateChanged -> (lampID)

The LampStateChanged signal is used to notify that the Lamp State has changed.

Output arguments:

  * **lampID** --- string --- the Lamp ID.

## References

  * The XML definition of the [LampState interface](LampState-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

