# org.alljoyn.SmartSpaces.Operation.Brightness version 1

## Theory of Operation

This interface provides to the _consumer_ (i.e. a remote application) the
capability to directly set the brightness value of the _producer_ (i.e. the appliance).

This interface is intended to be implemented by devices that emit light,
e.g., lamps, screens etc. or become the brightness/value component of an HSV color model
when combined with the **org.alljoyn.SmartSpaces.Operation.Color** interface.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |


### Properties

#### Version

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint16                                                                |
| Access                | read                                                                  |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = const              |

The interface version.

#### Brightness

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | double                                                                |
| Access                | readwrite                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |
| Annotation            | org.alljoyn.Bus.Type.Min = 0.0                                        |
| Annotation            | org.alljoyn.Bus.Type.Max = 1.0                                        |

Holds the current brightness value of the device.

 * The minimum value of 0.0 indicates that zero light is emitted but the device is
still functionally on, e.g., a television that has had its brightness set to 0.0
will still emit sound.

 * The maximum value of 1.0 indicates the maximum amount of light is emitted by that
device.


### Methods

No methods are exposed by this interface.

### Signals

No signals are emitted by this interface.

### Interface Errors

No errors associated to this interface.

## References

  * The XML definition of the [Brightness interface](Brightness-v1.xml)
  * The theory of operation of the HAE service framework [Theory of Operation](/org.alljoyn.SmartSpaces/theory-of-operation-v2)
  * The definition of the [Color interface](Color-v1)
