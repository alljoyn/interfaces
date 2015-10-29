# org.alljoyn.Config version 1

## Theory of Operation

This documents the org.alljoyn.Config interface.  This is a legacy interface
that predates the formation of the Interface Review Board and their guidelines,
thus this interface may violate one or more of the guidelines.

Details on the Config service can be found here:
[https://allseenalliance.org/framework/documentation/learn/base-services/configuration/interface]


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

### Methods

#### Restart

This method restarts or poser cycles the device.

Input arguments:

_None_

Output arguments:

_None_

Errors raise by this method:

  * **org.alljoyn.Error.FeatureNotAvailable** --- Restart is not supported.


#### FactoryReset

Directs the device to disconnect from the personal AP, clear all previously
configured data, and start the softAP mode.

Input arguments:
_None_

Output arguments:
_None_

Errors raised by this method:

  * **org.alljoyn.Error.FeatureNotAvailable** --- FactoryReset is not supported.


#### SetPasscode(daemonRealm, passcode)

Updates the passcode to be used for the org.alljoyn.Config interface which is
secure. The default passcode is 000000 until it is overwritten by SetPasscode.

Input arguments:

  * **daemonRealm** --- string --- Identifies the daemon's identity for secure
    access. This parameter is currently ignored by the Configuration service
    framework.
  * **passcode** --- byte[] --- Passphrase that will be utilized for the secure
    Config interface.

Output arguments:
_None_

Errors raised by this method:

_None_


#### GetConfigurations(languageTag) -> (configData)

Returns all the configurable fields specified within the scope of the Config
interface. If language tag is not specified (i.e., ""), configuration fields
based on the device's default language are returned.

Input arguments:

  * **languageTag** --- string --- Language tag used to retrieve Config fields.

Output aruguments:

  * **configData** --- ConfigDictionaryType --- Returns configuration fields in
    the form of dictionary. See Configuration map fields for the default set of
    Configuration map fields.

Errors raised by this method:

  * **org.alljoyn.Error.LanguageNotSupported** --- Returned if a language tag is
    not supported by the device.


#### UpdateConfigurations(languageTag, configMap)

Provides a mechanism to update the configuration fields.

Input arguments:

  * **languageTag** --- string --- Identifies the language tag.
  * **configMap** --- ConfigDictionaryType --- Set of configuration fields being
    updated.

Output arguments:

_None_

Errors raised by this method:

  * **org.alljoyn.Error.InvalidValue** --- Returned whenever there is an error
    in updating the value for a specific field in the configMap. The error
    message will contain the field name of the invalid field.
  * **org.alljoyn.Error.LanguageNotSupported** --- Returned if a language tag is
    not supported by the device.


#### ResetConfigurations(languageTag, fieldList)

Provides a mechanism to reset (i.e., value is restored to factory default but
the field itself is retained) values of configuration fields.

Input arguments:

  * **languageTag** --- string --- Identifies the language tag.
  * **fieldList** --- string[] --- List of fields or configuration items that
    are being reset.

Output arguments:

_None_

Errors raised by this method:

  * **org.alljoyn.Error.InvalidValue** --- Returned whenever there is an error
    in updating the value for a specific field in the configMap. The error
    message will contain the field name of the invalid field.
  * **org.alljoyn.Error.LanguageNotSupported** --- Returned if a language tag is
    not supported by the device.



### Named Types

#### dictionary ConfigDictionaryType

  * **key** --- string --- Config field.
  * **value** --- variant --- Config value.
  
The following table lists the known configuration fields that are part of the
configMap parameter fields. The OEM or application developer can add additional
fields.

| Config Field    | Config Value                                                                                            |
|-----------------|---------------------------------------------------------------------------------------------------------|
| DefaultLanguage | Default language supported by the device.                                                               |
| DeviceName      | Device name assigned by the user. The device name appears on the UI as the friendly name of the device. |


### Interface Errors

The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                             | Error message                              |
|----------------------------------------|--------------------------------------------|
| org.alljoyn.Error.LanguageNotSupported | The language tag is not supported.         |
| org.alljoyn.Error.InvalidValue         | Parameter data contained an invalid value. |
| org.alljoyn.Error.FeatureNotAvailable  | Feature is not supported.                  |


## References

  * The XML definition of the [Config interface](Config-v1.xml)
  * Detailed description of Config on the AllSeen Alliance: [https://allseenalliance.org/framework/documentation/learn/base-services/configuration/interface]

