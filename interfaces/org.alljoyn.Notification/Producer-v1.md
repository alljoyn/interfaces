# org.alljoyn.Notification.Producer version 1


## Theory of Operation

This documents the org.alljoyn.Notification.Dismisser interface.  This is a
legacy interface that predates the formation of the Interface Review Board and
their guidelines, thus this interface may violate one or more of the guidelines.

Details on the Notification service can be found here:
[https://allseenalliance.org/framework/documentation/learn/base-services/notification/interface]

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |


### Methods

#### Dismiss(msgId)

This dismisses a Notification on a producer.

Input arguments:

  * **msgId** --- int32 --- Unique ID of the message to dismiss.

Output arguments:
_None_

## References

  * The XML definition of the [Notification Producer interface](Producer-v1.xml)
  * Detailed description of Notification on the AllSeen Alliance: [https://allseenalliance.org/framework/documentation/learn/base-services/notification/interface]

