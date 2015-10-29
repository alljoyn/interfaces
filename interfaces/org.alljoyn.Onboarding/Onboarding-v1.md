# org.alljoyn.Onboarding version 1

## Theory of Operation

This documents the org.alljoyn.Onboarding interface.  This is a legacy interface
that predates the formation of the Interface Review Board and their guidelines,
thus this interface may violate one or more of the guidelines.

### Definition Overview

The Onboarding interface is implemented by an application on a target device,
referred to as an onboardee. A typical onboardee is an AllJoyn™ thin client
device. This interface allows the onboarder to send the Wi-Fi credentials to the
onboardee to allow it to join the personal access point.

![Onboarding architecture](onboarding-arch.png)

**Figure:** Onboarding service framework architecture within the AllJoyn
  framework

**NOTE:** All methods and signals are considered mandatory to support the AllSeen Alliance Compliance and Certification program.
Onboarding

### Call Flows

#### Onboarding call flow using an Android onboarder

The following figure illustrates a call flow for onboarding an onboardee using
an Android onboarder.

![Android Onboarding call flow](onboarding-android-onboarder.png)

**Figure:** Onboarding a device using an Android onboarder

#### Onboarding call flow using an iOS onboarder

The following figure illustrates a call flow for onboarding an onboardee using
an iOS onboarder.

![iOS Onboarding call flow](onboarding-ios-onboarder.png)

**Figure:** Onboarding a device using an iOS onboarder


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = off                                          |


### Properties

#### State

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | int16                                                                 |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |

The configuration state.  Possible values:

  * 0 --- Personal AP Not Configured
  * 1 --- Personal AP Configured/Not Validated
  * 2 --- Personal AP Configured/Validating
  * 3 --- Personal AP Configured/Validated
  * 4 --- Personal AP Configured/Error
  * 5 --- Personal AP Configured/Retry


#### LastError

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | LastErrorStruct                                                       |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |

The last error code and error message. Error_message is the error message
received from the underlying Wi-Fi layer.  Possible error codes:

  * 0 --- Validated
  * 1 --- Unreachable
  * 2 --- Unsupported protocol
  * 3 --- Unauthorized
  * 4 --- Error message



### Methods

#### ConfigWifi(SSID, passphrase, authType) -> status

Sends the personal AP information to the onboardee. When the authType is equal
to -1 (any), the onboardee must try out all the possible authentication types it
supports to connect to the personal AP.

Input arguments:

  * **SSID** --- string --- Access point SSID.
  * **passphrase** --- string --- Access point passphrase.
  * **authType** --- int16 --- Authentication type.  Possible values:
    * -3 --- WPA2 Auto
    * -2 --- WPA Auto
    * -1 --- Any
    *  0 --- Open
    *  1 --- WEP
    *  2 --- WPA TKIP
    *  3 --- WPA CCMP
    *  4 --- WPA2 TKIP
    *  5 --- WPA2 CCMP
    *  6 --- WPS

Output arguments:

  * **status** --- int16 --- Response status.  Possible values:
    * 1 --- Current SoftAP mode will be disabled upon receipt of Connect. In
      this case, the Onboarder application must wait for the device to connect
      on the personal AP and query the State and LastError properties.
    * 2 --- Concurrent step used to validate the personal AP connection. In this
      case, the Onboarder application must wait for the ConnectionResult signal
      to arrive over the AllJoyn session established over the SoftAP link.

Errors raised by this method:

  * org.alljoyn.Error.OutOfRange --- Returned in the AllJoyn method call reply
    if authType parameter is invalid.

#### Connect()

Tells the onboardee to connect to the personal AP. It is recommended that the
onboardee use the concurrency feature, if it is available.

Input arguments:

_None_

Output arguments:

_None_

Errors raised by this method:

_None_


#### Offboard()

Tells the onboardee to disconnect from the personal AP, clear the personal AP
configuration fields, and start the soft AP mode.

Input arguments:

_None_

Output arguments:

_None_

Errors raised by this method:

_None_


#### GetScanInfo() -> (age, scanList)

Scans all the Wi-Fi access points in the onboardee's proximity.

Input arguments:

_None_

Output arguments:

  * **age** --- uint16 --- Age of the scan information in minutes. Reflects how
    long ago the scan procedure was performed by the device.
  * **scanList** --- ScanListStruct[] --- Array of records containing the SSID
    and authType.


Errors raised by this method:

  * org.alljoyn.Error.FeatureNotAvailable --- Returned in the AllJoyn response
    if the device does not support this feature.
    

### Signals

#### ConnectionResult -> (resultCode, resultsMessage)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This signal is emitted when the connection attempt against the personal AP is
completed. The signal is sent over the AllJoyn session established over the
SoftAP link.

This signal will be received only if the concurrency feature is supported by the
onboardee.

Output arguments:

  * **resultCode** --- int16 --- Connection result code.  Possible values are:
    * 0 --- Validated
    * 1 --- Unreachable
    * 2 --- Unsupported protocol
    * 3 --- Unauthorized
    * 4 --- Error message
  * **resultMessage** --- string --- Text that describes the connection result.


### Named Types

#### struct LastErrorStruct

The last error code and error message. Error_message is the error message
received from the underlying Wi-Fi layer.  Fields are as follows:

  * **errorNumber** --- int16 --- Error number.
  * **errorString** --- string --- Error message string.

#### struct ScanListStruct

Access point SSID and Auth Type.

  * **ssid** --- string --- Access point SSID.
  * **authType** --- int16 ---  Access point auth type.  Possible values are:
    *  0 --- Open
    *  1 --- WEP
    *  2 --- WPA TKIP
    *  3 --- WPA CCMP
    *  4 --- WPA2 TKIP
    *  5 --- WPA2 CCMP
    *  6 --- WPS

### Interface Errors

| Error name                              | Error message                  |
|-----------------------------------------|--------------------------------|
| org.alljoyn.Error.OutOfRange            | authType parameter is invalid. |
| org.alljoyn.Error.FeatureNotAvailable   | Feature not supported.         |



## References

  * The XML definition of the [Onboarding interface](Onboarding-v1.xml)
