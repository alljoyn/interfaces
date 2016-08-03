# org.alljoyn.Notification version 1

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

The org.alljoyn.Notification interface is implemented by the producer to send
notifications.

## Specification

|            |                              |
|:-----------|:-----------------------------|
| Version    | 1                            |
| Annotation | org.alljoyn.Bus.Secure = off |

### Signals

#### notify -> (version, msgId, msgType, deviceId, deviceName, appId, appName, attributes, customAttributes, langText)

|             |             |
|:------------|:------------|
| Signal Type | sessionless |

This is the Notification signal.

Output arguments:

  * **version** --- `uint16` --- Service version.
  * **msgId** --- `int32` --- Notification ID. This ID is unique within the
    context of the producer.
  * **msgType** --- `uint16` --- Message type:
      * `0` --- Emergency
      * `1` --- Warning
      * `2` --- Information
  * **deviceId** --- `string` --- Unique device ID (as defined in [About][about]
      service) of device running the app that implements this interface.
  * **deviceName** --- `string` --- Name of the device running the app that
    implements this interface.&sup1;
  * **appId** --- `byte[]` --- Unique app ID (as defined in [About][about]
      service) of app that implements this interface.
  * **appName** --- `string` --- Name of app that implements this interface.
    &sup1;
  * **attributes** --- [`AttributesType`][attr_type] --- supplemental
    information.
  * **customAttributes** --- [`CustomAttributesType`][custom_type] ---
    vendor-defined supplemental information.
  * **langText** --- [`LangTextType[]`][text_type] --- dictionary of message
    text content in one or more languages.

&sup1; &ndash; The deviceName and appName values correspond to the first
available value from the following sources:
* DeviceName from Config service.
* DeviceName/AppName from About service using DefaultLanguage from Config
  service as key.
* DeviceName/AppName from About service using DefaultLanguage from About service
  as key.

### Named Types

#### dictionary AttributesType

  * **key** --- `int32` --- attribute type:
      * 0 --- Rich Notification Icon URL.
      * 1 --- Rich Notification Audio URL.
      * 2 --- Rich Notification Icon Object Path.
      * 3 --- Rich Notification Audio Object Path.
      * 4 --- Response Object Path (see description in value list).
      * 5 --- Original Sender.
  * **value** --- `variant`&sup1; --- attribute value (depends on key):
      * 0 --- `string` --- Icon URL.
      * 1 --- [`AudioUrlType`][audio_type] --- Dictionary of audio URLs.
      * 2 --- `object path` --- Object path to Icon information.
      * 3 --- `object path` --- Object path to Audio information.
      * 4 --- `object path` --- Object path to a Control Panel that can be
        presented to the user for immediate notification response.
      * 5 --- `string` --- Bus name of the original sender.

&sup1; &ndash; Use of `variant` type in interfaces violates IRB guidelines. Do
not use this as precedent.

#### dictionary CustomAttributesType

  * **key** --- `string` --- vendor-defined key.
  * **value** --- `string` --- vendor-defined string.

#### struct LangTextType

  * **language** --- `string` --- [RFC 5646][rfc_5646] tag declaring language
    used in text.
  * **text** --- `string` --- Message text.

#### dictionary AudioUrlType

  * **key** --- `string` --- [RFC 5646][rfc_5646] tag declaring language of
    content at URL in value.
  * **value** --- `string` --- URL to audio data.

## References

  * The Notification interface [XML Definition](Notification-v1.xml)
  * The Notification Service [Theory of Operation][too]

[attr_type]: #dictionary-attributestype
[custom_type]: #dictionary-customattributestype
[text_type]: #struct-langtexttype
[audio_type]: #dictionary-audiourltype
[too]: ../org.alljoyn.Notification/theory-of-operation
[about]: About-v1
[gerrit_change]: https://git.allseenalliance.org/gerrit/6353
[irb_wiki]: https://wiki.allseenalliance.org/interfacereviewboard
[rfc_5646]: https://tools.ietf.org/html/rfc5646
