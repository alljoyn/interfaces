# org.allseen.LSF.ControllerService.MasterScene version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, lookup and apply MasterScenes on Lamps.

A Master Scene consists of a list of Scenes. Scenes are preferences saved by the End User 
to set a particular mood or to simply store a setting for convenience and future recall.  
Scenes are implemented in the Controller Service and are made up of a Preset and/or Effects 
applied to a set of Lamps and/or LampGroups.

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

#### GetAllMasterSceneIDs () -> (responseCode, masterSceneIDs)

This method allows the LSF Controller Clients to fetch all the MasterScene IDs. 

Input arguments: None

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|


  * **masterSceneIDs** --- string[] --- the list of all MasterScene IDs.

#### GetMasterSceneName (masterSceneID, language) -> (responseCode, masterSceneID, language, masterSceneName)

This method allows the LSF Controller Clients to fetch the name of a MasterScene specified by the masterSceneID. 

Input arguments:

  * **masterSceneID** --- string --- the MasterScene ID.
  * **language** --- string --- the language tag associated with the MasterScene Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|


  * **masterSceneID** --- string --- the MasterScene ID.
  * **language** --- string --- the language tag associated with the MasterScene Name.
  * **masterSceneName** --- string --- the name of the MasterScene. The max supported size is 256 bytes.

#### SetMasterSceneName (masterSceneID, masterSceneName, language) -> (responseCode, masterSceneID, language)

This method allows the LSF Controller Clients to set the name of a MasterScene specified by the masterSceneID. 

Input arguments:

  * **masterSceneID** --- string --- the MasterScene ID.
  * **masterSceneName** --- string --- the name of the MasterScene. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the MasterScene Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 16    | The entity of interest was not found                   			|



  * **masterSceneID** --- string --- the MasterScene ID.
  * **language** --- string --- the language tag associated with the MasterScene Name. 

#### CreateMasterScene (scenes, masterSceneName, language) -> (responseCode, masterSceneID)

This method allows the LSF Controller Clients to create a MasterScene. 

Input arguments:

  * **scenes** --- string[] --- Array of Scene IDs.
  * **masterSceneName** --- string --- the MasterScene Name.
  * **language** --- string --- the language tag associated with the MasterScene Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 17    | There is no slot for new entry                                              	|


  * **masterSceneID** --- string --- the ID of the created MasterScene.

#### UpdateMasterScene (masterSceneID, scenes) -> (responseCode, masterSceneID)

This method allows the LSF Controller Clients to update a MasterScene. 

Input arguments:

  * **masterSceneID** --- string --- the ID of the MasterScene.
  * **scenes** --- string[] --- Array of Scene IDs.

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


  * **masterSceneID** --- string --- the ID of the MasterScene.

#### DeleteMasterScene (masterSceneID) -> (responseCode, masterSceneID)

This method allows the LSF Controller Clients to delete a MasterScene. 

Input arguments:

  * **masterSceneID** --- string --- the ID of the MasterScene.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|
| 18    | There is a dependency of the entity for which a delete request was received   |


  * **masterSceneID** --- string --- the ID of the MasterScene.

#### GetMasterScene (masterSceneID) -> (responseCode, masterSceneID, scenes)

This method allows the LSF Controller Clients to fetch a MasterScene. 

Input arguments:

  * **masterSceneID** --- string --- the ID of the MasterScene.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|


  * **masterSceneID** --- string --- the ID of the MasterScene.
  * **scenes** --- string[] --- Array of Scene IDs.

#### ApplyMasterScene (masterSceneID) -> (responseCode, masterSceneID)

This method allows the LSF Controller Clients to apply a MasterScene.

Input arguments:

  * **masterSceneID** --- string --- the ID of the MasterScene.

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
| 16    | The entity of interest was not found                   			|


  * **masterSceneID** --- string --- the ID of the MasterScene.

### Signals

#### MasterScenesNameChanged -> (masterSceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The MasterScenesNameChanged signal is used to notify the list of the MasterScene IDs whose
names have changed.

Output arguments:

  * **masterSceneIDs** --- string[] --- the list of MasterScene IDs.

#### MasterScenesCreated -> (masterSceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The MasterScenesCreated signal is used to notify the list of the MasterScene IDs that have 
been created.

Output arguments:

  * **masterSceneIDs** --- string[] --- the list of MasterScene IDs that have been created.

#### MasterScenesUpdated -> (masterSceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The MasterScenesUpdated signal is used to notify the list of the MasterScene IDs that have 
been updated.

Output arguments:

  * **masterSceneIDs** --- string[] --- the list of MasterScene IDs that have been updated.

#### MasterScenesDeleted -> (masterSceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The MasterScenesDeleted signal is used to notify the list of the MasterScene IDs that have 
been deleted.

Output arguments:

  * **masterSceneIDs** --- string[] --- the list of MasterScene IDs that have been deleted.

#### MasterScenesApplied -> (masterSceneIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The MasterScenesApplied signal is used to notify the list of the MasterScene IDs that have 
been applied.

Output arguments:

  * **masterSceneIDs** --- string[] --- the list of MasterScene IDs that have been applied.

## References

  * The XML definition of the [MasterScene interface](MasterScene-v1.xml)


