# org.alljoyn.About version 1

## Theory of Operation

This documents the org.alljoyn.About interface.  This is a legacy interface that
predates the formation of the Interface Review Board and their guidelines, thus
this interface may violate one or more of the guidelines.

Details on the About service can be found here:
[https://allseenalliance.org/framework/documentation/learn/core/about-announcement/interface]


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

### Methods

### GetAboutData(languageTag) -> (aboutData)

Method to retrieve About data.

Input arguments:

  * **languageTag** --- string --- The desired language.

Output arguments:

  * **aboutData** --- AboutDataType --- The About metadata.


### Signals

#### Announce -> (version, port, objectDescription, metaData)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessionless                       |

This is the About announcement signal.

Output arguments:

  * **version** --- uint16 --- Version of the Notification protocol.
  * **port** --- uint16 --- Session port number to connect to announcer.
  * **objectDescription** --- ObjectDescriptionType[] --- About object
    description information.
  * **metaData** --- AboutDataType --- About metadata dictionary.

### Named Types

### struct ObjectDescriptionType

  * **objectPath** --- object path --- Object path being announced.
  * **interfaces** --- string[] --- List of interfaces implemented by
    objectPath.

#### dictionary AboutDataType

  * **key** --- string --- Attribute. The table below lists the
    predefined attributes and their corresponding value types.
  * **value** --- variant --- Attribute value.

| Attribute          | Mandatory | Value Type | Value meaning                                               |
|--------------------|-----------|------------|-------------------------------------------------------------|
| AppId              | yes       | byte[]     | A 128-bit globally unique identifier for the application.   |
| DefaultLanguage    | yes       | string     | The default language supported by the device.               |
| DeviceName         | no        | string     | Name of the device set by platform-specific means.          |
| DeviceId           | yes       | string     | Device identifier set by platform-specific means.           |
| AppName            | yes       | string     | Application name assigned by the app manufacturer.          |
| Manufacturer       | yes       | string     | The manufacturer's name of the app.                         |
| ModelNumber        | yes       | string     | The app model number.                                       |
| SupportedLanguages | yes       | string[]   | List of supported languages.                                |
| Description        | yes       | string     | Detailed description expressed in language tags.            |
| DateOfManufacture  | no        | string     | Date of manufacture using format YYYY-MM-DD.                |
| SoftwareVersion    | yes       | string     | Software version of the app.                                |
| AJSoftwareVersion  | yes       | string     | Current version of the AllJoyn SDK used by the application. |
| HardwareVersion    | no        | string     | Hardware version of the device on which the app is running. |
| SupportUrl         | no        | string     | Support URL.                                                |


## References

  * The XML definition of the [About interface](About-v1.xml)
  * Detailed description of About on the AllSeen Alliance: [https://allseenalliance.org/framework/documentation/learn/core/about-announcement/interface]

