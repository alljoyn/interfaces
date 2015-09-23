# org.allseen.LSF.LampDetails version 1


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

The make the Lamp. The list of valid values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | MAKE_INVALID                                                  		|
| 1     | MAKE_LIFX                                                       		|
| 2     | MAKE_OEM1                                                       		|

#### Model

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The model of the Lamp. The list of valid values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | MODEL_INVALID                                                  		|
| 1     | MODEL_LED                                                       		|

#### Type

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The device type of the Lamp. The list of valid values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | TYPE_INVALID                                                  		|
| 1     | TYPE_LAMP                                                       		|
| 2     | TYPE_OUTLET                                                      		|
| 3     | TYPE_LUMINAIRE                                                  		|
| 4     | TYPE_SWITCH                                                       		|

#### LampType

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The LampType of the Lamp. The list of valid values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | LAMPTYPE_INVALID                                                  		|
| 1     | LAMPTYPE_A15                                                       		|
| 2     | LAMPTYPE_A17                                                      		|
| 3     | LAMPTYPE_A19                                                  		|
| 4     | LAMPTYPE_A20                                                       		|
| 5     | LAMPTYPE_A21                                                      		|
| 6     | LAMPTYPE_A23                                                  		|
| 7     | LAMPTYPE_AR70                                                       		|
| 8     | LAMPTYPE_AR111                                                      		|
| 9     | LAMPTYPE_B8                                                     		|
| 10    | LAMPTYPE_B10                                                       		|
| 11    | LAMPTYPE_B11                                                      		|
| 12    | LAMPTYPE_B13                                                       		|
| 13    | LAMPTYPE_BR25                                                      		|
| 14    | LAMPTYPE_B30                                                       		|
| 15    | LAMPTYPE_B38                                                      		|
| 16    | LAMPTYPE_B40                                                       		|
| 17    | LAMPTYPE_BT15                                                      		|
| 18    | LAMPTYPE_BT28                                                      		|
| 19    | LAMPTYPE_BT37                                                      		|
| 20    | LAMPTYPE_BT56                                                      		|
| 21    | LAMPTYPE_C6                                                       		|
| 22    | LAMPTYPE_C7                                                      		|
| 23    | LAMPTYPE_C9                                                       		|
| 24    | LAMPTYPE_C11                                                      		|
| 25    | LAMPTYPE_C15                                                       		|
| 26    | LAMPTYPE_CA5                                                      		|
| 27    | LAMPTYPE_CA7                                                       		|
| 28    | LAMPTYPE_CA8                                                      		|
| 29    | LAMPTYPE_CA10                                                      		|
| 30    | LAMPTYPE_CA11                                                      		|
| 31    | LAMPTYPE_E17                                                      		|
| 32    | LAMPTYPE_E18                                                      		|
| 33    | LAMPTYPE_E23                                                       		|
| 34    | LAMPTYPE_E37                                                      		|
| 35    | LAMPTYPE_ED17                                                      		|
| 36    | LAMPTYPE_ED18                                                      		|
| 37    | LAMPTYPE_ED23                                                      		|
| 38    | LAMPTYPE_ED28                                                      		|
| 39    | LAMPTYPE_ED37                                                      		|
| 40    | LAMPTYPE_F10                                                      		|
| 41    | LAMPTYPE_F15                                                      		|
| 42    | LAMPTYPE_F20                                                      		|
| 43    | LAMPTYPE_G9                                                      		|
| 44    | LAMPTYPE_G11                                                      		|
| 45    | LAMPTYPE_G12                                                      		|
| 46    | LAMPTYPE_G16                                                      		|
| 47    | LAMPTYPE_G19                                                      		|
| 48    | LAMPTYPE_G25                                                      		|
| 49    | LAMPTYPE_G30                                                      		|
| 50    | LAMPTYPE_G40                                                      		|
| 51    | LAMPTYPE_T2                                                      		|
| 52    | LAMPTYPE_T3                                                      		|
| 53    | LAMPTYPE_T4                                                      		|
| 54    | LAMPTYPE_T5                                                      		|
| 55    | LAMPTYPE_T6                                                      		|
| 56    | LAMPTYPE_T7                                                      		|
| 57    | LAMPTYPE_T8                                                      		|
| 58    | LAMPTYPE_T9                                                      		|
| 59    | LAMPTYPE_T10                                                      		|
| 60    | LAMPTYPE_T12                                                      		|
| 61    | LAMPTYPE_T14                                                      		|
| 62    | LAMPTYPE_T20                                                      		|
| 63    | LAMPTYPE_MR8                                                      		|
| 64    | LAMPTYPE_MR11                                                      		|
| 65    | LAMPTYPE_MR16                                                      		|
| 66    | LAMPTYPE_MR20                                                      		|
| 67    | LAMPTYPE_PAR14                                                      		|
| 68    | LAMPTYPE_PAR16                                                      		|
| 69    | LAMPTYPE_PAR20                                                      		|
| 70    | LAMPTYPE_PAR30                                                      		|
| 71    | LAMPTYPE_PAR36                                                      		|
| 72    | LAMPTYPE_PAR38                                                      		|
| 73    | LAMPTYPE_PAR46                                                      		|
| 74    | LAMPTYPE_PAR56                                                      		|
| 75    | LAMPTYPE_PAR64                                                      		|
| 76    | LAMPTYPE_PS25                                                      		|
| 77    | LAMPTYPE_PS35                                                      		|
| 78    | LAMPTYPE_R12                                                      		|
| 79    | LAMPTYPE_R14                                                      		|
| 80    | LAMPTYPE_R16                                                      		|
| 81    | LAMPTYPE_R20                                                      		|
| 82    | LAMPTYPE_R25                                                      		|
| 83    | LAMPTYPE_R30                                                      		|
| 84    | LAMPTYPE_R40                                                      		|
| 85    | LAMPTYPE_RP11                                                      		|
| 86    | LAMPTYPE_S6                                                      		|
| 87    | LAMPTYPE_S8                                                      		|
| 88    | LAMPTYPE_S11                                                      		|
| 89    | LAMPTYPE_S14                                                      		|
| 90    | LAMPTYPE_ST18                                                      		|

#### LampBaseType

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The base type of the Lamp. The list of valid values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | BASETYPE_INVALID                                                  		|
| 1     | BASETYPE_E5                                                       		|
| 2     | BASETYPE_E10                                                      		|
| 3     | BASETYPE_E11                                                  		|
| 4     | BASETYPE_E12                                                      		|
| 5     | BASETYPE_E14                                                      		|
| 6     | BASETYPE_E17                                                  		|
| 7     | BASETYPE_E26                                                       		|
| 8     | BASETYPE_E27                                                      		|
| 9     | BASETYPE_E29                                                     		|
| 10    | BASETYPE_E39                                                       		|
| 11    | BASETYPE_B22                                                      		|
| 12    | BASETYPE_GU10                                                       		|

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

Indicates if the lamp supports effects.

#### MinVoltage

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the minimum voltage supported by the lamp.

#### MaxVoltage

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the maximum voltage supported by the lamp.

#### Wattage

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the wattage of the lamp.

#### IncandescentEquivalent

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the incandescent equivalent of the lamp in watts.

#### MaxLumens

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the max lumens of the lamp.

#### MinTemperature

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the minimum temperature supported by the lamp in Kelvin.

#### MaxTemperature

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the maximum temperature supported by the lamp in Kelvin.

#### ColorRenderingIndex

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Indicates the color rendering index of the lamp.

#### LampID

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | string                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The unique ID of the lamp.

## References

  * The XML definition of the [LampDetails interface](LampDetails-v1.xml)


