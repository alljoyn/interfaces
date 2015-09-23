# org.allseen.LSF.LampDetails version 1

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
Clients to access the details of a Lamp.

Lamp details are the LSF-specific metadata that the lamp exposes such that information 
about the lamp can be introspected via a Controller Service.  Lamp Details are read-only 
and set at the time of manufacturing.  Certain Lamp Details fields are mandatory, others 
are optional.

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

#### Make

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The make the Lamp. This is same as the About Manufacturer field. The list of valid values are:

| Value | Description                                                       	|
|-------|-----------------------------------------------------------------------|
| 0     | INVALID                                                  		|
| 1     | LIFX                                                       		|
| 2     | OEM1                                                       		|

#### Model

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The model of the Lamp. The list of valid values are:

| Value | Description                                                       	|
|-------|-----------------------------------------------------------------------|
| 0     | INVALID                                                  		|
| 1     | LED                                                       		|

#### Type

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The device type of the Lamp. The list of valid values are:

| Value | Description                                                       	|
|-------|-----------------------------------------------------------------------|
| 0     | INVALID                                                  		|
| 1     | LAMP                                                       		|
| 2     | OUTLET                                                      		|
| 3     | LUMINAIRE (Virtual Lamp)                                     		|
| 4     | SWITCH                                                       		|

#### LampType

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The LampType of the Lamp. The list of valid values are:

| Value | Description                                                       	|
|-------|-----------------------------------------------------------------------|
| 0     | INVALID                                                  		|
| 1     | A15                                                       		|
| 2     | A17                                                      		|
| 3     | A19                                                  		        |
| 4     | A20                                                       		|
| 5     | A21                                                      		|
| 6     | A23                                                  		        |
| 7     | AR70                                                       		|
| 8     | AR111                                                      		|
| 9     | B8                                                     		|
| 10    | B10                                                       		|
| 11    | B11                                                      		|
| 12    | B13                                                       		|
| 13    | BR25                                                      		|
| 14    | B30                                                       		|
| 15    | B38                                                      		|
| 16    | B40                                                       		|
| 17    | BT15                                                      		|
| 18    | BT28                                                      		|
| 19    | BT37                                                      		|
| 20    | BT56                                                      		|
| 21    | C6                                                       		|
| 22    | C7                                                      		|
| 23    | C9                                                       		|
| 24    | C11                                                      		|
| 25    | C15                                                       		|
| 26    | CA5                                                      		|
| 27    | CA7                                                       		|
| 28    | CA8                                                      		|
| 29    | CA10                                                      		|
| 30    | CA11                                                      		|
| 31    | E17                                                      		|
| 32    | E18                                                      		|
| 33    | E23                                                       		|
| 34    | E37                                                      		|
| 35    | ED17                                                      		|
| 36    | ED18                                                      		|
| 37    | ED23                                                      		|
| 38    | ED28                                                      		|
| 39    | ED37                                                      		|
| 40    | F10                                                      		|
| 41    | F15                                                      		|
| 42    | F20                                                      		|
| 43    | G9                                                      		|
| 44    | G11                                                      		|
| 45    | G12                                                      		|
| 46    | G16                                                      		|
| 47    | G19                                                      		|
| 48    | G25                                                      		|
| 49    | G30                                                      		|
| 50    | G40                                                      		|
| 51    | T2                                                      		|
| 52    | T3                                                      		|
| 53    | T4                                                      		|
| 54    | T5                                                      		|
| 55    | T6                                                      		|
| 56    | T7                                                      		|
| 57    | T8                                                      		|
| 58    | T9                                                      		|
| 59    | T10                                                      		|
| 60    | T12                                                      		|
| 61    | T14                                                      		|
| 62    | T20                                                      		|
| 63    | MR8                                                      		|
| 64    | MR11                                                      		|
| 65    | MR16                                                      		|
| 66    | MR20                                                      		|
| 67    | PAR14                                                      		|
| 68    | PAR16                                                      		|
| 69    | PAR20                                                      		|
| 70    | PAR30                                                      		|
| 71    | PAR36                                                      		|
| 72    | PAR38                                                      		|
| 73    | PAR46                                                      		|
| 74    | PAR56                                                      		|
| 75    | PAR64                                                      		|
| 76    | PS25                                                      		|
| 77    | PS35                                                      		|
| 78    | R12                                                      		|
| 79    | R14                                                      		|
| 80    | R16                                                      		|
| 81    | R20                                                      		|
| 82    | R25                                                      		|
| 83    | R30                                                      		|
| 84    | R40                                                      		|
| 85    | RP11                                                      		|
| 86    | S6                                                      		|
| 87    | S8                                                      		|
| 88    | S11                                                      		|
| 89    | S14                                                      		|
| 90    | ST18                                                      		|

#### LampBaseType

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The base type of the Lamp. The list of valid values are:

| Value | Description                                                       	|
|-------|-----------------------------------------------------------------------|
| 0     | INVALID                                                  		|
| 1     | E5                                                       		|
| 2     | E10                                                      		|
| 3     | E11                                                  		        |
| 4     | E12                                                      		|
| 5     | E14                                                      		|
| 6     | E17                                                  		        |
| 7     | E26                                                       		|
| 8     | E27                                                      		|
| 9     | E29                                                     		|
| 10    | E39                                                       		|
| 11    | B22                                                      		|
| 12    | GU10                                                       		|

#### LampBeamAngle

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The beam angle of the Lamp.

#### Dimmable

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | bool                                                     |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates if the lamp is dimmable.

#### Color

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | bool                                                     |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates if the lamp supports color.

#### VariableColorTemp

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | bool                                                     |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates if the lamp supports variable color temperature.

#### HasEffects

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | bool                                                     |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates if the lamp supports effects. Transition and Pulse Effects are examples of supported effects.

#### MinVoltage

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the minimum voltage supported by the lamp. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### MaxVoltage

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the maximum voltage supported by the lamp. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### Wattage

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the wattage of the lamp. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### IncandescentEquivalent

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the incandescent equivalent of the lamp in watts. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### MaxLumens

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the max lumens of the lamp. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### MinTemperature

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the minimum color temperature supported by the lamp in Kelvin. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### MaxTemperature

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the maximum color temperature supported by the lamp in Kelvin. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### ColorRenderingIndex

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the color rendering index of the lamp. This value is dictated by the Lamp hardware and can be referenced from the Lamp DataSheet.

#### LampID

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | string                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The unique ID of the lamp. This is same as the About DeviceID.

## References

  * The XML definition of the [LampDetails interface](LampDetails-v1.xml)


