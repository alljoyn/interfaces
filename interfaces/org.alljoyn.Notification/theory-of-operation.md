# org.alljoyn.Notification

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply with the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/6353/).

## Theory of Operation


### Definition Overview

The AllJoyn Notification service framework is a software layer that enables
AllJoyn devices to send notifications to other AllJoyn devices. These devices
are categorized as producers and consumers. Producers produce and send
notifications, while consumers consume and display these notifications. An end
user's home (Wi-Fi) network can have multiple producers connected and generating
notification messages, as well as multiple consumers connected and consuming
these messages.

The Notification service framework design supports text notification payload as
well as rich notification media (icon and audio). For rich media, the
notification message payload can include URL links or AllJoyn object path
references to rich notification media. The consumer app receiving the
notification message will fetch the rich notification media from the object path
or the producer device.

The Notification service framework uses the AllJoyn framework sessionless signal
to deliver notification messages. The Notification service framework exposes the
Notification Service API for application developers to deliver and receive
notification messages. The device OEM uses the Notification service framework
Producer API to send notification messages. The Notification service framework
sends these notification messages over the AllJoyn sessionless signal transport
mechanism and makes them available to consumer devices listening for sessionless
signals. The consumer running the Notification service framework registers with
the AllJoyn framework to receive notification messages. The application
developer for the consumer device uses the Notification service framework
Consumer API to register and receive notifications from any producer that is
sending notification on the Wi-Fi network.

NOTE: All methods and signals are considered mandatory to support the AllSeen
Alliance Compliance and Certification program.

### Architecture

The Notification service framework implements the Notification interface which
is the over-the-wire interface to deliver messages from producers to
consumers. Application developers making use of the Notification service
framework implement against the Notification service framework APIs (producer
and consumer side). They do not implement the Notification interface.

The following figure illustrates the Notification service framework API and
Notification interface on producers and consumers.

![Notification architecture](notification-arch.png)

**Figure:** Notification service framework architecture within the AllJoyn
  framework

### Typical call flow

The following figure illustrates a typical Notification service framework call
flow with a single producer app generating a notification message. The message
is then acquired by two consumer apps on the AllJoyn network.

![Notification call flow](notification-typical-call-flow.png)

**Figure:** Typical Notification service framework call flow

The AllJoyn framework on the producer device does a sessionless signal broadcast
for the notification message. This is received by the AllJoyn framework on the
consumer devices. The AllJoyn framework then fetches the notification message
over unicast session from the producer AllJoyn core and delivers to the consumer
application.

### Specification

#### Notification messages

The notification message comprises a set of fields including message type and
message TTL. These notification fields are specified by the producer app when
sending notification message as part of Notification service framework Producer
API.

#### Message type and TTL fields

The message type defines the type of notification messages (emergency, warning
and information). Multiple types of notification messages can be sent at the
same time by a producer. The message TTL defines the validity period of the
notification message. Notification messages can be received by consumers that
connect during the defined message TTL value.

Messages with the same message type will overwrite each other on the producer,
so a consumer that connects to the network after the notification was sent will
receive only the last of each message type.

#### Notification message behavior

The following behavior is supported using the Notification service framework.

 * If another notification message of the same message type is sent by a
   producer app within the TTL period, the new message overwrites the existing
   message.
   
 * If a consumer connects to the network after the TTL period expires, that
   consumer will not receive the message. For example, when a consumer such as
   a mobile phone is on the home network and the end user leaves the home; the
   consumer is no longer on the home network. The mobile phone will not receive
   notification messages when it reacquires the home network and the TTL of
   those notifications have expired.

NOTE: The value is only used for message validity on the producer device. The
TTL field is not sent as part of the notification message payload data over the
end user's home network.

See Notification Service Framework Use Cases for use case scenarios related to
notification message behavior.

#### Dismissing a notification

The dismiss notification is an option for consumers that have received the
notification to let the producer know that this notification has been seen and
there is no need to continue sending. It also lets other consumers know that the
notification can be removed from the user display.

When a consumer attempts to dismiss a notification, the service framework
creates a session with the producer using the original sender field sent in the
notification.

Using the original sender field confirms that the notification is received by
the actual producer and not the super agent in case the consumer received the
notification from the super agent.

The producer will then send out a dismiss sessionless signal to notify the rest
of the consumers in the network that this notification has been dismissed.

If the producer is not reachable, the consumer will send out the dismiss
sessionless signal on its own.
