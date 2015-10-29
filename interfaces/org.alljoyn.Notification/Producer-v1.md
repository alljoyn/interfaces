# org.alljoyn.Notification.Producer version 1


## Theory of Operation

This documents the org.alljoyn.Notification.Dismisser interface.  This is a
legacy interface that predates the formation of the Interface Review Board and
their guidelines, thus this interface may violate one or more of the guidelines.

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = off                                          |


### Methods

#### Dismiss(msgId)

This dismisses a Notification on a producer.

Input arguments:

  * **msgId** --- int32 --- Unique ID of the message to dismiss.

Output arguments:
_None_

## References

  * The XML definition of the [Notification Producer interface](Producer-v1.xml)
  * The Notification [Theory of Operation](theory-of-operation.md)
