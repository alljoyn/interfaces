# org.alljoyn.SmartSpaces.Operation.PlugInUnits version 1

## Theory of Operation

This interface is designed to be used in combo devices which have some sub-units
that can be plugged in and out or disabled/enabled on the fly.

The interface gives information about:
* the sub-units that can be hot plugged in and out, specifying their bus objects
* their current plug in status.

When a sub unit is not plugged in, the related bus object should not be used by
the _consumer_ because the related interfaces are not working.
This interface lets the _consumer_ get in real time the sub-unit status by
emitting related properties change signals and so the _consumer_ is aware of the
possibility to use the related bus object.
If a sub unit is plugged or unplugged, the corresponding bus objects doesn't
have to be appear and disappear; i.e. the bus object structure doesn't change in
real time and always includes all the bus objects corresponding to the plug in
units supported by the device. In this way:
* the messages of the About mechanism at discovery phase don't change depending
  on the sub units status
* the PlugInUnits interface doesn't have any reference to not existing
  bus-object in its properties.

## Specification

|            |                               |
| ---------- | ----------------------------- |
| Version    | 1                             |
| Annotation | org.alljoyn.Bus.Secure = true |

### Properties

#### Version

|            |                                                         |
| ---------- | ------------------------------------------------------- |
| Type       | uint16                                                  |
| Access     | read-only                                               |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true |

Interface version.
The EmitsChangedSignal of this property should be modified to const once that
feature is available.

#### PlugInUnits

|            |                                                         |
| ---------- | ------------------------------------------------------- |
| Type       | PlugInInfo[]                                            |
| Access     | read-only                                               |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true |

The lists of all the possible hot pluggable devices and their associated status
which indicates which are plugged in and which are not.

### Methods

No methods are implemented by this interface.

### Signals

No signals are emitted by this interface.

### Named Types

No Named Types are required by this interface.

### Named Types

#### struct PlugInInfo

The **PlugInInfo** is used to describe the characteristics and the status
of a plug-in unit. Its elements are:

  * **objectPath** --- object_path --- object path of the unit
  * **deviceId** --- uint32 --- device type id of the unit as defined in the HAE
    service framework [Theory of Operation](/org.alljoyn.SmartSpaces/theory-of-operation-v1)
  * **pluggedIn** --- boolean --- current status of the unit
    * **false** --- not active, not present or not inserted
    * **true** --- active, present or inserted

### Interface Errors

No unique errors are associated with this interface.

## Examples

A typical example of usage of this interface is an oven device with a plug in
meat probe.
The bus objects of the units are:
  * /appliance  --- for the root
  * /appliance/oven --- for the oven cavity
  * /appliance/oven/probe -- for the meat probe

The **PlugInUnits** interfaces should be implemented in the **/appliance/oven**
object path and with an element in its list which describes the meat probe as a
**FoodProbe** device.

## References

  * The XML definition of the [PlugInUnits interface](PlugInUnits-v1.xml)
  * The theory of operation of the HAE service framework [Theory of Operation](/org.alljoyn.SmartSpaces/theory-of-operation-v1)

