# org.alljoyn.Notification version 1

## Theory of Operation

This documents the org.alljoyn.Notification interface.  This is a legacy
interface that predates the formation of the Interface Review Board and their
guidelines, thus this interface may violate one or more of the guidelines.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = off                                          |


### Signals

#### notify -> (version, msgId, msgType, deviceId, deviceName, appId, appName, attributes, customAttributes, langText)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessionless                       |

This is the Notification signal.

Output arguments:

  * **version** --- uint16 --- Version of the Notification protocol
  * **msgId** --- int32 --- Unique ID for the message.
  * **msgType** --- uint16 --- Indicates the type of message.  Possible values
    are defined below:
      * 0 --- Emergency
      * 1 --- Warning
      * 2 --- Information
  * **deviceId** --- string --- Device ID as defined for About.
  * **deviceName** --- string --- Device name as defined for About.
  * **appId** --- byte[] --- Application ID as defined for About.
  * **appName** --- string --- Application name as defined for About.
  * **attributes** --- AttributesType --- Supplimental information for the Notification.
  * **customAttributes** --- CustomeAttributesType --- OEM defined attributes.
  * **langText** --- LangTextType[] --- Text message in one or more languages.


### Named Types

#### dictionary AttributesType

  * **key** --- int32 --- Attribute type.  Possible values are:
      * 0 --- Rich Notification URL.
      * 1 --- Rich Notification Audio URL.
      * 2 --- Rich Notification Icon Object Path.
      * 3 --- Rich Notification Audio Object Path.
      * 4 --- Response Object Path.
      * 5 --- Original Sender.
  * **value** --- variant --- Value type depends on the attribute type specified
    in the key:
      * 0 --- string --- Icon URL.
      * 1 --- AudioUrlType --- Map of languages to audio URLs.
      * 2 --- object path --- Object path to Icon information.
      * 3 --- object path --- Object path to Audio information.
      * 4 --- object path --- Response object path.
      * 5 --- string --- Bus name of the original sender.

#### dictionary CustomAttributesType

  * **key** --- string --- OEM defined key.
  * **value** --- string --- OEM defined string.

#### struct LangTextType

  * **language** --- string --- Language specifier.
  * **text** --- string --- Message text in the associated language.

#### dictionary AudioUrlType

  * **key** --- string --- Language specifier.
  * **value** --- string --- Audio URL.


## References

  * The XML definition of the [Notification interface](Notification-v1.xml)
  * The Notification [Theory of Operation](theory-of-operation.md)
