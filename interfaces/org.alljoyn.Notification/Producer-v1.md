# org.alljoyn.Notification.Producer version 1

## Important Note

This interface has been defined prior to the creation of the interface design
guidelines for AllSeen. Many design decisions in this interface do not comply
with the guidelines and constitute bad precedent. Do not use these interfaces as
a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki][irb_wiki].

For a detailed annotation of the interface design guideline violations in this
interface, please visit the [Gerrit submission page][gerrit_change].

## Theory of Operation

The Notification Service declares the roles of _producer_ apps that send
notifications and _consumer_ apps that receive them. For more detailed
information, refer to the Notification Service [Theory of Operation][too].

The org.alljoyn.Notification.Producer interface is implemented by a producer. A
consumer can use this interface to request that the producer stop broadcasting a
notification. To inform consumers that a notification was dismissed, implement
the org.alljoyn.Notification.Dismisser interface as well.

## Specification

|            |                              |
|:-----------|:-----------------------------|
| Version    | 1                            |
| Annotation | org.alljoyn.Bus.Secure = off |

### Methods

#### Dismiss(msgId)

Request the producer stop broadcasting a specified notification.

Input arguments:

  * **msgId** --- `int32` --- Notification ID. This ID is unique within the
    context of the producer.

Output arguments:

_None_

## References

  * The Notification Producer interface [XML Definition](Producer-v1.xml)
  * The Notification Service [Theory of Operation][too]

[too]: theory-of-operation
[gerrit_change]: https://git.allseenalliance.org/gerrit/6353
[irb_wiki]: https://wiki.allseenalliance.org/interfacereviewboard
