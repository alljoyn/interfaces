# org.alljoyn.Notification.Dismisser version 1

## Theory of Operation

This documents the org.alljoyn.Notification.Dismisser interface.  This is a
legacy interface that predates the formation of the Interface Review Board and
their guidelines, thus this interface may violate one or more of the guidelines.

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = off                                          |


### Signals

#### Dismiss -> (msgId, appId)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessionless                       |

This informs other devices that a Notification has been dismissed.

Output arguments:

  * **msgId** --- int32 --- Unique ID of the message that was dismissed.
  * **appId** --- byte[] --- ID of the application whose Notification message
    was dismissed.

## References

  * The XML definition of the [Notification Dismisser interface](Dismisser-v1.xml)
  * The Notification [Theory of Operation](theory-of-operation.md)
