# org.allseen.LSF.ControllerService.SceneElement version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5631/).


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, look up and apply Scene Elements.

A Scene Element consists of a list of Lamps and Lamp Groups and an associated Effect ID
of the effect that is applied to the Lamps and Lamp Groups in the SceneElement when the
Scene Element is applied. The effect is either a Transition Effect, Pulse Effect or Preset.

A Pulse Effect brightens and then dims a Lamp or Lamp Group set to a predefined hue, 
saturation, or color temperature at a defined duty cycle (ratio between pulse duration 
in milliseconds and period in milliseconds).

A Transition Effect transitions a Lamp or Lamp Group from its current state to a 
target state over a specified number of milliseconds.   For a Hue, Saturation, 
Color Temperature or Brightness, the transition is linear in the HSV color model.  

A Preset is a user-specified Lamp State that is stored in the Controller Service.

The Controller Service can store only a certian maximum number of Scene Elements or until the size of the 
persisted Scene Elements file reaches a certian specified value whichever occurs first. If the user attempts to create more 
than the allowed number of Scene Elements, "no slot for new entry" is returned as the responseCode or if the user tries to
create a new Scene Element after the size of the Scene Element file reaches the maximum allowed value, "Not enough resources" is
returned as the responseCode. These maximum values are dictated by the product that implements this interface.

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
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true	|

The interface version number.

### Methods

#### GetAllSceneElementIDs () -> (responseCode, sceneElementIDs)

This method allows the LSF Controller Clients to fetch all the Scene Element IDs. 

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

  * **sceneElementIDs** --- string[] --- the list of all Scene Element IDs.

#### GetSceneElementName (sceneElementID, language) -> (responseCode, sceneElementID, language, sceneElementName)

This method allows the LSF Controller Clients to fetch the name of a Scene Element specified by the sceneElementID. 

Input arguments:

  * **sceneElementID** --- string --- the Scene Element ID.
  * **language** --- string --- the language tag associated with the Scene Element Name.

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

  * **sceneElementID** --- string --- the Scene Element ID.
  * **language** --- string --- the language tag associated with the Scene Element Name.
  * **sceneElementName** --- string --- the name of the Scene Element. The max supported size is 256 bytes. 

#### SetSceneElementName (sceneElementID, sceneElementName, language) -> (responseCode, sceneElementID, language)

This method allows the LSF Controller Clients to set the name of a Scene Element specified by the sceneElementID. 

Input arguments:

  * **sceneElementID** --- string --- the Scene Element ID.
  * **sceneElementName** --- string --- the name of the Scene Element. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Scene Element Name.

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


  * **sceneElementID** --- string --- the Scene Element ID.
  * **language** --- string --- the language tag associated with the Scene Element Name. 

#### CreateSceneElement (sceneElement, sceneElementName, language) -> (responseCode, sceneElementID)

This method allows the LSF Controller Clients to create a Scene Element. 

Input arguments:

  * **sceneElement** --- struct(string[], string[], string) --- the Scene Element structure.
  * **sceneElementName** --- string --- the name of the Scene Element. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Scene Element Name.

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

  * **sceneElementID** --- string --- the ID of the created Scene Element.

#### UpdateSceneElement (sceneElementID, sceneElement) -> (responseCode, sceneElementID)

This method allows the LSF Controller Clients to update a Scene Element. 

Input arguments:

  * **sceneElementID** --- string --- the ID of the Scene Element.
  * **sceneElement** --- struct(string[], string[], string) --- the Scene Element structure.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer encountered internally.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 11    | The arguments were invalid                                        		|
| 13    | Not enough resources                                               		|
| 16    | The entity of interest was not found                   			|

  * **sceneElementID** --- string --- the ID of the Scene Element.

#### DeleteSceneElement (sceneElementID) -> (responseCode, sceneElementID)

This method allows the LSF Controller Clients to delete a Scene Element. 

Input arguments:

  * **sceneElementID** --- string --- the ID of the Scene Element.

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

  * **sceneElementID** --- string --- the ID of the Scene Element.

#### GetSceneElement (sceneElementID) -> (responseCode, sceneElementID, sceneElement)

This method allows the LSF Controller Clients to fetch a Scene Element. 

Input arguments:

  * **sceneElementID** --- string --- the ID of the Scene Element.

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

  * **sceneElementID** --- string --- the ID of the Scene Element.
  * **sceneElement** --- struct(string[], string[], string) --- the Scene Element structure.

#### ApplySceneElement (sceneElementID) -> (responseCode, sceneElementID)

This method allows the LSF Controller Clients to apply a Scene Element. 

Input arguments:

  * **sceneElementID** --- string --- the ID of the Scene Element.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer encountered internally.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|

  * **sceneElementID** --- string --- the ID of the Scene Element.

### Signals

#### SceneElementsNameChanged -> (sceneElementIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The SceneElementsNameChanged signal is used to notify the list of the Scene Element IDs whose
names have changed.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs.

#### SceneElementsCreated -> (sceneElementIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The SceneElementsCreated signal is used to notify the list of the Scene Element IDs that have 
been created.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been created.

#### SceneElementsUpdated -> (sceneElementIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The SceneElementsUpdated signal is used to notify the list of the Scene Element IDs that have 
been updated.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been updated.

#### SceneElementsDeleted -> (sceneElementIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The SceneElementsDeleted signal is used to notify the list of the Scene Element IDs that have 
been deleted.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been deleted.

#### SceneElementsApplied -> (sceneElementIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The SceneElementsApplied signal is used to notify the list of the Scene Element IDs that have 
been applied.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been applied.

## References

  * The XML definition of the [SceneElement interface](SceneElement-v1.xml)

