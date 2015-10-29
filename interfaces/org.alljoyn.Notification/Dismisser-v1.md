# org.alljoyn.Notification.Dismisser version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply with the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/6353/).

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
  * The Notification [Theory of Operation](theory-of-operation)
