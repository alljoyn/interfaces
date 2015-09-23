# org.allseen.LSF.LampParameters version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5877/).

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


