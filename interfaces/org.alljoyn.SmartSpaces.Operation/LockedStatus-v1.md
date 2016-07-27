# org.alljoyn.SmartSpaces.Operation.LockedStatus version 1

## Theory of Operation

This interface provides to the _consumer_ (i.e. a remote application) the
capability to monitor the status of the locking mechanism on the _producer_ (i.e. the appliance).

This interface provides to the _consumer_ the capability to only monitor the
status of the locking mechanism. The capability to control it (i.e. to remotely lock/unlock)
is implemented by the interfaces **org.alljoyn.SmartSpaces.Operation.LockControl** and
**org.alljoyn.SmartSpaces.Operation.LockControl**.

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

#### IsLocked

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | bool                                                                  |
| Access                | read                                                                  |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

It indicates the current status of the locking mechanism.

 * false --- **Unlocked** --- the mechanism is unlocked.
 * true --- **Locked** --- the mechanism is locked.


### Methods

No methods are exposed by this interface.

### Signals

No signals are emitted by this interface.

### Interface Errors

No error associated to this interface.

## References

 * The XML definition of the [LockedStatus interface](LockedStatus-v1.xml)
 * The theory of operation of the HAE service framework [Theory of Operation](/org.alljoyn.SmartSpaces/theory-of-operation-v2)
 * The definition of the [LockControl interface](LockControl-v1)
 * The definition of the [UnlockControl interface](UnlockControl-v1)
