# org.alljoyn.About version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/6353/).

## Theory of Operation

This documents the org.alljoyn.About interface.  This is a legacy interface that
predates the formation of the Interface Review Board and their guidelines, thus
this interface may violate one or more of the guidelines.


### Definition Overview

The About interface is to be implemented by an application on a target
device. This interface allows the app to advertise itself so other apps can
discover it. The following figure illustrates the relationship between a client
app and a service app.

![About Architecture](about-arch.png)

**Figure:** About feature architecture within the AllJoyn framework

NOTE: All methods and signals are considered mandatory to support the AllSeen
Alliance Compliance and Certification program.

### Discovery

A client can discover the app via an announcement which is a sessionless signal
containing the basic app information like app name, device name, manufacturer,
and model number. The announcement also contains the list of object paths and
service framework interfaces to allow the client to determine whether the app
provides functionality of interest.

In addition to the sessionless announcement, the About interface also provides
the on-demand method calls to retrieve all the available metadata about the app
that are not published in the announcement.

### Discovery Call Flows

#### Typical discovery flow

The following figure illustrates a typical call flow for a client to discover a
service app. The client merely relies on the sessionless announcement to decide
whether to connect to the service app to use its service framework offering.

![About typical discovery flow](about-typical-discovery.png)

**Figure:** Typical discovery flow (client discovers a service app)

#### Nontypical discovery flow

The following figure illustrates a call flow for a client to discover a service
app and make a request for more detailed information.

![About non-typical discovery flow](about-nontypical-discovery.png)

**Figure:** Nontypical discovery call flow


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = off                                          |

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
