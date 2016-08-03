# org.alljoyn.Notification.Dismisser version 1

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

The org.alljoyn.Dismisser interface is implemented by the producer to signal
dismissal of a notification message.

It is also possible for a consumer to implement this interface. If the producer
is no longer available on the network, a consumer may choose to implement this
interface and notify other consumers that a notification should be dismissed.

## Specification

|            |                              |
|:-----------|:-----------------------------|
| Version    | 1                            |
| Annotation | org.alljoyn.Bus.Secure = off |

### Signals

#### Dismiss -> (msgId, appId)

|             |             |
|:------------|:------------|
| Signal Type | sessionless |

Inform consumers that a notification has been dismissed. Upon receiving this
signal, consumers should stop presenting the identified notification.

Output arguments:

  * **msgId** --- `int32` --- Notification ID. This ID is unique within the
    context of the producer.
  * **appId** --- `byte[]` --- The AppId of the producer as specified in the
    [About interface description][about].

## References

  * The Notification Dismisser interface [XML Definition](Dismisser-v1.xml)
  * The Notification Service [Theory of Operation][too]

[about]: ../org.alljoyn/About-v1
[too]: theory-of-operation
[gerrit_change]: https://git.allseenalliance.org/gerrit/6353
[irb_wiki]: https://wiki.allseenalliance.org/interfacereviewboard
