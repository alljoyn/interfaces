# org.allseen.LeaderElectionAndStateSync version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5877/).

## Theory of Operation
This interface is provided by the LSF Controller Service to enable multiple LSF Controller
Services to elect a leader amongst themselves and sync the lighting data.

In a Lighting Service Framework network, there can be scenarios wherein multiple LSF Controller 
Services are present in a proximal network. As per the LSF architecture, User Applications control 
Lamps through the LSF Controller Service; and Lamp Groups, Scenes, Presets, Effects and Master Scenes 
defined by user are persisted by the LSF Controller Service. In a scenario where-in there are multiple 
LSF Controller Services in a proximal network:
* Multiple LSF Controller Services need to elect a Leader amongst themselves, and this one leader must 
advertise itself as the single service through which all the LSF User Applications can control the Lamps;
* Once an LSF Controller Service is elected as a Leader, it lazily synchronizes its own persisted state 
with that of the with the persisted state in the set of Followers; 
* The LSF Controller Service elected as the Master informs its Followers of any updates to the system 
persistent state as long as it is the Leader allowing Followers to optimize future persistent state 
synchronization processes;
* If the LSF Controller Service elected as Master becomes unreachable (is disconnected from its Followers), 
the Followers collectively re-elect a new Leader;
* In the case of (temporary) network partitions, multiple Leaders may be present in this case, once the 
partition(s) is resolved, the multiple Leaders will elect a new single Leader and any former Leaders will 
'resign' and become Followers (releasing any associated Lamps so that they may re-associate with the new Leader.


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

  * **checksumAndTimestamp** --- array(struct[uint32, uint32, uint64]) --- Array of structure containing lighting data type and the corresponding checksum and timestamp in milliseconds (representing NTP time) of when it was last updated

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

  * **blob** --- string --- the lighting data.
  * **checksum** --- uint16 --- the checksum associated with the lighting data.
  * **timestamp** --- uint64 --- the timestamp in milliseconds (representing NTP time) of last update of the lighting data.

#### Overthrow () -> (success)

This method allows any entity to overthrow the current leader. 

Input arguments: None

Output arguments:

  * **success** --- bool --- Status of the overthrow operation

### Signals

#### BlobChanged -> (blobType, blob, checksum, timestamp)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The BlobChanged signal is used to notify the receivers about the updated contents of a particular lighting data type.

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

  * **blob** --- string --- the lighting data.
  * **checksum** --- uint16 --- the checksum associated with the lighting data.
  * **timestamp** --- uint64 --- the timestamp in milliseconds (representing NTP time) of last update of the lighting data.

## References

  * The XML definition of the [LeaderElectionAndStateSync interface](LeaderElectionAndStateSync-v1.xml)


