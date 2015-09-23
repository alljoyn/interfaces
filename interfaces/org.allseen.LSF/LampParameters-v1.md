# org.allseen.LSF.LampParameters version 1


## Theory of Operation
This interface is provided by the LSF Lamp Service to enable the LSF Lamp
Clients to access the parameters of a Lamp.

Lamp Parameters are read-only volatile parameters that are read from the Lamp hardware.

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

#### Energy_Usage_Milliwatts

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The energy usage of the Lamp in milliwatts.

#### Brightness_Lumens

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16[]                                                 |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The brightness of the Lamp in Lumens

## References

  * The XML definition of the [LampParameters interface](LampParameters-v1.xml)


