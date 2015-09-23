# org.allseen.LeaderElectionAndStateSync version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable multiple LSF Controller
Services to elect a leader amongst themselves and sync the lighting data.

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

#### GetChecksumAndModificationTimestamp () -> (checksumAndTimestamp)

This method allows the other LSF Controller Services to fetch the checksum and timestamp associated
with each lighting data type from a particular LSF Controller Service. 

Input arguments: None

Output arguments:

  * **checksumAndTimestamp** --- struct[uint16, unit16, unit64] --- Array of lighting data type and the corresponding checksum and timestamp of when it was last updated

#### GetBlob (blobType) -> (blobType, blob, checksum, timestamp)

This method allows the LSF Controller Services to fetch the lighting data of a particular type specified by blobType. 

Input arguments:

  * **blobType** --- uint16 --- the lighting data type. The list of valid
    values are:

| Value         | Description                                                       		|
|---------------|-------------------------------------------------------------------------------|
| 0             | Preset                                                   		        |
| 1             | Lamp Group                                          		                |
| 2             | Scene                          		                                |
| 3             | Master Scene                                               		        |
| 4             | Transition Effect                                                   		|
| 5             | Pulse Effect                                            		        |
| 6             | Preset Update                   		                                |
| 7             | Lamp Group Update                                          		        |
| 8             | Scene Update                                   		                |
| 9             | Master Scene Update                                         		        |
| 10            | Transition Effect Update                                                      |
| 11            | Pulse Effect Update                                        		        |
| 12            | Scene Element                          					|
| 13            | Scene v2                                               		        |
| 0xFFFFFFFF    | Last Value                                                                    |

Output arguments:

  * **blobType** --- uint16 --- the lighting data type. The list of valid
    values are:

| Value         | Description                                                       		|
|---------------|-------------------------------------------------------------------------------|
| 0             | Preset                                                   		        |
| 1             | Lamp Group                                          		                |
| 2             | Scene                          		                                |
| 3             | Master Scene                                               		        |
| 4             | Transition Effect                                                   		|
| 5             | Pulse Effect                                            		        |
| 6             | Preset Update                   		                                |
| 7             | Lamp Group Update                                          		        |
| 8             | Scene Update                                   		                |
| 9             | Master Scene Update                                         		        |
| 10            | Transition Effect Update                                                      |
| 11            | Pulse Effect Update                                        		        |
| 12            | Scene Element                          					|
| 13            | Scene v2                                               		        |
| 0xFFFFFFFF    | Last Value                                                                    |

  * **blob** --- string --- the lighting data.
  * **checksum** --- uint16 --- the checksum associated with the lighting data.
  * **timestamp** --- uint64 --- the timestamp of last update of the lighting data.

#### Overthrow () -> (success)

This method allows the LSF Controller Services to ovethrow the current leader. 

Input arguments: None

Output arguments:

  * **success** --- bool --- Status of the overthrow operation

### Signals

#### BlobChanged -> (blobType, blob, checksum, timestamp)

The BlobChanged signal is used to notify the updated contents of a particular lighting data type.

Output arguments:

  * **blobType** --- uint16 --- the lighting data type. The list of valid
    values are:

| Value         | Description                                                       		|
|---------------|-------------------------------------------------------------------------------|
| 0             | Preset                                                   		        |
| 1             | Lamp Group                                          		                |
| 2             | Scene                          		                                |
| 3             | Master Scene                                               		        |
| 4             | Transition Effect                                                   		|
| 5             | Pulse Effect                                            		        |
| 6             | Preset Update                   		                                |
| 7             | Lamp Group Update                                          		        |
| 8             | Scene Update                                   		                |
| 9             | Master Scene Update                                         		        |
| 10            | Transition Effect Update                                                      |
| 11            | Pulse Effect Update                                        		        |
| 12            | Scene Element                          					|
| 13            | Scene v2                                               		        |
| 0xFFFFFFFF    | Last Value                                                                    |

  * **blob** --- string --- the lighting data.
  * **checksum** --- uint16 --- the checksum associated with the lighting data.
  * **timestamp** --- uint64 --- the timestamp of last update of the lighting data.

## References

  * The XML definition of the [LeaderElectionAndStateSync interface](LeaderElectionAndStateSync-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

