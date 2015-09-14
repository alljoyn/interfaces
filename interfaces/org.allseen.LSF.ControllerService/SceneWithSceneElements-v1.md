# org.allseen.LSF.ControllerService.SceneWithSceneElements version 1

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
Clients to create, update and look up Scene with Scene with Scene Elements.

A Scene with Scene Elements consists of a list of Scene Elements that constitute the
Scene.

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

The Controller Service can store only a certian maximum number of Scenes or until the size of the 
persisted Scenes file reaches a certian specified value whichever occurs first. If the user attempts to create more 
than the allowed number of Scenes, "no slot for new entry" is returned as the responseCode or if the user tries to
create a new Scene after the size of the Scene file reaches the maximum allowed value, "Not enough resources" is
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

#### CreateSceneWithSceneElements (sceneElementIDs, sceneName, language) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to create a Scene with Scene Elements. 

Input arguments:

  * **sceneElementIDs** --- string[] --- list of Scene Element IDs.
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
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 17    | There is no slot for new entry                                              	|

  * **sceneID** --- string --- the ID of the created Scene.

#### UpdateSceneWithSceneElements (sceneID, sceneElementIDs) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to update a Scene with Scene Elements. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.
  * **sceneElementIDs** --- string[] --- list of Scene Element IDs.

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

  * **sceneID** --- string --- the ID of the Scene.

#### GetSceneWithSceneElements (sceneID) -> (responseCode, sceneID, sceneElementIDs)

This method allows the LSF Controller Clients to fetch a Scene with Scene Elements. 

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
  * **sceneElementIDs** --- string[] --- list of Scene Element IDs.

## References

  * The XML definition of the [SceneWithSceneElements interface](SceneWithSceneElements-v1.xml)

