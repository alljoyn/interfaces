# org.allseen.LSF.ControllerService.Scene version 1

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
Clients to create, update, delete, look up and apply Scenes on Lamps.

Scenes are preferences saved by the End User to set a particular mood or to simply store 
a setting for convenience and future recall.  Scenes are implemented in the Controller Service 
and are made up of a Preset and/or Effects applied to a set of Lamps and/or LampGroups.

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

#### GetAllSceneIDs () -> (responseCode, sceneIDs)

This method allows the LSF Controller Clients to fetch all the Scene IDs. 

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


  * **sceneIDs** --- string[] --- the list of all Scene IDs.

#### GetSceneName (sceneID, language) -> (responseCode, sceneID, language, sceneName)

This method allows the LSF Controller Clients to fetch the name of a Scene specified by the sceneID. 

Input arguments:

  * **sceneID** --- string --- the Scene ID.
  * **language** --- string --- the language tag associated with the Scene Name.

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


  * **sceneID** --- string --- the Scene ID.
  * **language** --- string --- the language tag associated with the Scene Name.
  * **sceneName** --- string --- the name of the Scene. The max supported size is 256 bytes.

#### SetSceneName (sceneID, sceneName, language) -> (responseCode, sceneID, language)

This method allows the LSF Controller Clients to set the name of a Scene specified by the sceneID. 

Input arguments:

  * **sceneID** --- string --- the Scene ID.
  * **sceneName** --- string --- the name of the Scene. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Scene Name.

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



  * **sceneID** --- string --- the Scene ID.
  * **language** --- string --- the language tag associated with the Scene Name. 

#### CreateScene (transitionlampsLampGroupsToState, transitionlampsLampGroupsToPreset, pulselampsLampGroupsWithState, pulselampsLampGroupsWithPreset, sceneName, language) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to create a Scene. 

Input arguments:

  * **transitionlampsLampGroupsToState** --- array of struct[string[], string[], key-value pairs, uint16] --- Transition to State component.
  * **transitionlampsLampGroupsToPreset** --- array of struct[string[], string[], string, uint16] --- Transition to Preset component.
  * **pulselampsLampGroupsWithState** --- array of struct[string[], string[], key-value pairs, key-value pairs, uint16, uint16, uint16] --- Pulse with State component.
  * **pulselampsLampGroupsWithPreset** --- array of struct[string[], string[], string, string, uint16, uint16, uint16] --- Pulse with Preset component.
  * **sceneName** --- string --- the Scene Name. The max supported size is 256 bytes. 
  * **language** --- string --- the language tag associated with the Scene Name.

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


  * **sceneID** --- string --- the ID of the created Scene.

#### UpdateScene (sceneID, transitionlampsLampGroupsToState, transitionlampsLampGroupsToPreset, pulselampsLampGroupsWithState, pulselampsLampGroupsWithPreset) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to update a Scene. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.
  * **transitionlampsLampGroupsToState** --- array of struct[string[], string[], key-value pairs, uint16] --- Transition to State component.
  * **transitionlampsLampGroupsToPreset** --- array of struct[string[], string[], string, uint16] --- Transition to Preset component.
  * **pulselampsLampGroupsWithState** --- array of struct[string[], string[], key-value pairs, key-value pairs, uint16, uint16, uint16] --- Pulse with State component.
  * **pulselampsLampGroupsWithPreset** --- array of struct[string[], string[], string, string, uint16, uint16, uint16] --- Pulse with Preset component.

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


  * **sceneID** --- string --- the ID of the Scene.

#### DeleteScene (sceneID) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to delete a Scene. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.

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


  * **sceneID** --- string --- the ID of the Scene.

#### GetScene (sceneID) -> (responseCode, sceneID, transitionlampsLampGroupsToState, transitionlampsLampGroupsToPreset, pulselampsLampGroupsWithState, pulselampsLampGroupsWithPreset)

This method allows the LSF Controller Clients to fetch a Scene. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.

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


  * **sceneID** --- string --- the ID of the Scene.
  * **transitionlampsLampGroupsToState** --- array of struct[string[], string[], key-value pairs, uint16] --- Transition to State component.
  * **transitionlampsLampGroupsToPreset** --- array of struct[string[], string[], string, uint16] --- Transition to Preset component.
  * **pulselampsLampGroupsWithState** --- array of struct[string[], string[], key-value pairs, key-value pairs, uint16, uint16, uint16] --- Pulse with State component.
  * **pulselampsLampGroupsWithPreset** --- array of struct[string[], string[], string, string, uint16, uint16, uint16] --- Pulse with Preset component.

#### ApplyScene (sceneID) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to apply a Scene.

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.

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
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|


  * **sceneID** --- string --- the ID of the Scene.

### Signals

#### ScenesNameChanged -> (sceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The ScenesNameChanged signal is used to notify the list of the Scene IDs whose
names have changed.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs.

#### ScenesCreated -> (sceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The ScenesCreated signal is used to notify the list of the Scene IDs that have 
been created.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been created.

#### ScenesUpdated -> (sceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The ScenesUpdated signal is used to notify the list of the Scene IDs that have 
been updated.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been updated.

#### ScenesDeleted -> (sceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The ScenesDeleted signal is used to notify the list of the Scene IDs that have 
been deleted.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been deleted.

#### ScenesApplied -> (sceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The ScenesApplied signal is used to notify the list of the Scene IDs that have 
been applied.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been applied.

## References

  * The XML definition of the [Scene interface](Scene-v1.xml)


