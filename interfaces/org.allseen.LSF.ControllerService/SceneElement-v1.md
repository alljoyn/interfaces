# org.allseen.LSF.ControllerService.SceneElement version 1


## Theory of Operation
The interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, lookup and apply Scene Elements on Lamps.

The interface is not secure.

## Specification

|                       |       |
|-----------------------|-------|
| Version               | 1     |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |

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
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **sceneElementIDs** --- string[] --- the list of all Scene Element IDs.

#### GetSceneElementName (sceneElementID, language) -> (responseCode, sceneElementID, language, sceneElementName)

This method allows the LSF Controller Clients to fetch the name of a Scene Element specified by the sceneElementID. 

Input arguments:

  * **sceneElementID** --- string --- the Scene Element ID.
  * **language** --- string --- the language associated with the Scene Element Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **sceneElementID** --- string --- the Scene Element ID.
  * **language** --- string --- the language associated with the Scene Element Name.
  * **sceneElementName** --- string --- the name of the Scene Element. 

#### SetSceneElementName (sceneElementID, sceneElementName, language) -> (responseCode, sceneElementID, language)

This method allows the LSF Controller Clients to set the name of a Scene Element specified by the sceneElementID. 

Input arguments:

  * **sceneElementID** --- string --- the Scene Element ID.
  * **sceneElementName** --- string --- the name of the Scene Element.
  * **language** --- string --- the language associated with the Scene Element Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|


  * **sceneElementID** --- string --- the Scene Element ID.
  * **language** --- string --- the language associated with the Scene Element Name. 

#### CreateSceneElement (sceneElement, sceneElementName, language) -> (responseCode, sceneElementID)

This method allows the LSF Controller Clients to create a Scene Element. 

Input arguments:

  * **sceneElement** --- struct(string[], string[], string) --- the Scene Element structure.
  * **sceneElementName** --- string --- the name of the Scene Element.
  * **language** --- string --- the language associated with the Scene Element Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

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
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

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
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

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
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **sceneElementID** --- string --- the ID of the Scene Element.
  * **sceneElement** --- struct(string[], string[], string) --- the Scene Element structure.

#### ApplySceneElement (sceneElementID) -> (responseCode, sceneElementID)

This method allows the LSF Controller Clients to update a Scene Element. 

Input arguments:

  * **sceneElementID** --- string --- the ID of the Scene Element.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **sceneElementID** --- string --- the ID of the Scene Element.

### Signals

#### SceneElementsNameChanged -> (sceneElementIDs)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | default                          |

The SceneElementsNameChanged signal is used to notify the list of the Scene Element IDs whose
names have changed.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs.

#### SceneElementsCreated -> (sceneElementIDs)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | default                          |

The SceneElementsCreated signal is used to notify the list of the Scene Element IDs that have 
been created.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been created.

#### SceneElementsUpdated -> (sceneElementIDs)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | default                          |

The SceneElementsUpdated signal is used to notify the list of the Scene Element IDs that have 
been updated.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been updated.

#### SceneElementsDeleted -> (sceneElementIDs)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | default                          |

The SceneElementsDeleted signal is used to notify the list of the Scene Element IDs that have 
been deleted.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been deleted.

#### SceneElementsApplied -> (sceneElementIDs)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | default                          |

The SceneElementsApplied signal is used to notify the list of the Scene Element IDs that have 
been applied.

Output arguments:

  * **sceneElementIDs** --- string[] --- the list of Scene Element IDs that have been applied.

## References

  * The XML definition of the [SceneElement interface](SceneElement-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

