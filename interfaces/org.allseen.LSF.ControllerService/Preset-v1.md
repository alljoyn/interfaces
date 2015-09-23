# org.allseen.LSF.ControllerService.Preset version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5877/).

## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, look up and apply Presets on Lamps.

A Preset is a user-specified Lamp State that is stored in the Controller Service.

The interface is not secure.

## Specification

|              |       				|
|--------------|--------------------------------|
| Version      | 1     				|
| Annotation   | org.alljoyn.Bus.Secure = false |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The interface version number.

### Methods

#### GetDefaultLampState () -> (responseCode, lampState)

This method allows the LSF Controller Clients to fetch the default lamp state. 

Input arguments: None

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|


  * **lampState** --- key value pairs --- the default lamp state. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.

#### SetDefaultLampState (lampState) -> (responseCode)

This method allows the LSF Controller Clients to set the default lamp state. 

Input arguments:

  * **lampState** --- key value pairs --- the default lamp state. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 11    | The arguments were invalid                                        		|


#### GetAllPresetIDs () -> (responseCode, presetIDs)

This method allows the LSF Controller Clients to fetch all the Preset IDs. 

Input arguments: None

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|


  * **presetIDs** --- string[] --- the list of all Preset IDs.

#### GetPresetName (presetID, language) -> (responseCode, presetID, language, presetName)

This method allows the LSF Controller Clients to fetch the name of a Preset specified by the presetID. 

Input arguments:

  * **presetID** --- string --- the Preset ID.
  * **language** --- string --- the language tag associated with the Preset Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|


  * **presetID** --- string --- the Preset ID.
  * **language** --- string --- the language tag associated with the Preset Name.
  * **presetName** --- string --- the name of the Preset. The max supported size is 256 bytes. 

#### SetPresetName (presetID, presetName, language) -> (responseCode, presetID, language)

This method allows the LSF Controller Clients to set the name of a Preset specified by the presetID. 

Input arguments:

  * **presetID** --- string --- the Preset ID.
  * **presetName** --- string --- the name of the Preset. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Preset Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 12    | The name is empty                          					|
| 16    | The entity of interest was not found                   			|



  * **presetID** --- string --- the Preset ID.
  * **language** --- string --- the language tag associated with the Preset Name. 

#### CreatePreset (lampState, presetName, language) -> (responseCode, presetID)

This method allows the LSF Controller Clients to create a Preset. 

Input arguments:

  * **lampState** --- key value pairs --- the state of the Preset. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.
  * **presetName** --- string --- the name of the Preset. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Preset Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 17    | There is no slot for new entry                                              	|


  * **presetID** --- string --- the ID of the created Preset.

#### UpdatePreset (presetID, lampState) -> (responseCode, presetID)

This method allows the LSF Controller Clients to update a Preset. 

Input arguments:

  * **presetID** --- string --- the ID of the Preset.
  * **lampState** --- key value pairs --- the state of the Preset. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 11    | The arguments were invalid                                        		|
| 13    | Not enough resources                                               		|
| 16    | The entity of interest was not found                   			|


  * **presetID** --- string --- the ID of the Preset.

#### DeletePreset (presetID) -> (responseCode, presetID)

This method allows the LSF Controller Clients to delete a Preset. 

Input arguments:

  * **presetID** --- string --- the ID of the Preset.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|
| 18    | There is a dependency of the entity for which a delete request was received   |


  * **presetID** --- string --- the ID of the Preset.

#### GetPreset (presetID) -> (responseCode, presetID, lampState)

This method allows the LSF Controller Clients to fetch a Preset. 

Input arguments:

  * **presetID** --- string --- the ID of the Preset.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|


  * **presetID** --- string --- the ID of the Preset.
  * **lampState** --- key value pairs --- the state of the Preset. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.

### Signals

#### DefaultLampStateChanged -> ()

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The DefaultLampStateChanged signal is used to notify that the default lamp state has changed.

Output arguments: None

#### PresetsNameChanged -> (presetIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PresetsNameChanged signal is used to notify the list of the Preset IDs whose
names have changed.

Output arguments:

  * **presetIDs** --- string[] --- the list of Preset IDs.

#### PresetsCreated -> (presetIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PresetsCreated signal is used to notify the list of the Preset IDs that have 
been created.

Output arguments:

  * **presetIDs** --- string[] --- the list of Preset IDs that have been created.

#### PresetsUpdated -> (presetIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PresetsUpdated signal is used to notify the list of the Preset IDs that have 
been updated.

Output arguments:

  * **presetIDs** --- string[] --- the list of Preset IDs that have been updated.

#### PresetsDeleted -> (presetIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PresetsDeleted signal is used to notify the list of the Preset IDs that have 
been deleted.

Output arguments:

  * **presetIDs** --- string[] --- the list of Preset IDs that have been deleted.

## References

  * The XML definition of the [Preset interface](Preset-v1.xml)


