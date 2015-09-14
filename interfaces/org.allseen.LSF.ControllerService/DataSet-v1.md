# org.allseen.LSF.ControllerService.DataSet version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to fetch all the Lamp metadata using a single method call.

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

#### GetLampDataSet (lampID, language) -> (responseCode, lampID, language, lampName, lampDetails, lampState, lampParameters)

This method allows the LSF Controller Clients to fetch all the Lamp metadata. 

Input arguments:

  * **lampID** --- string --- the unique identifier of the Lamp.
  * **language** --- string --- the language tag associated with the Lamp Name.

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

  * **lampID** --- string --- the unique identifier of the Lamp.
  * **language** --- string --- the language tag associated with the Lamp Name.
  * **lampName** --- string --- the name of the Lamp. 
  * **lampDetails** --- key value pairs --- the details associated with the Lamp.
  * **lampState** --- key value pairs --- the state of the Lamp. 
  * **lampParameters** --- key value pairs --- the parameters associated with the Lamp.

## References

  * The XML definition of the [DataSet interface](DataSet-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

