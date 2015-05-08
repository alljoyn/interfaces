# org.allseen.LocationServices version 1

## Theory of Operation
Location Services encompasses four types of service, presence, proximity, location and containment. 
Each of these services attempts to locate an entity in the proximal network.  
An entity is the respresentation of a device. The device may or may not be connected to the proximal 
network. Examples of entities are mobile phones, lights, beacons, smart watches, the location service, etc...
Entities can be knwon or anonymous. A known enity has registered with the location service. An anonymous 
entity has not.
The presence service announces the entry or exit of an entity onto the proximal network.
The presence service can queried to determine if an entity is on the proximal network.
Users of the presence service can subscribe to changes in presence of known, anonymous or all entities.

The proximity service provides relative distance between two entities.
Users of this service can query for the distance between two entities, or all entities within a specified distance.
Users of this service can subscribe to notifications when an entity comes within a specified distance of another 
or get a notification if an emtity moves a specified distance.

The location service provides the location of an entity.

The containment services provides information about which entities reside within a container and which container 
and entity is within.

Provide information that helps explain the interface. Typically, this includes

  * a summary of the interaction roles (normally, _producer_ and _consumer_)
  * a list of common use cases
  * common interaction flows (discovery, what conditions trigger signal emission, ...)
    Keep this bit very brief - if longer walkthroughs are necessary, add them at the
    end of the document in an Examples section.
  * relation with other Interfaces (e.g., there is a clear relationship between the
    Lighting Service Framework's Lamp Interface and the Lighting Controller Interface)

##Location Services Specification

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |


### Properties

#### version

|-----------------------|----------------------------------------------------------------------|
| Type                  | q                                                                    |
| Access                | read-only                                                            |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = true       |
| Introduced in version | n (only add this line for n > 1)                                     |

There is room for a long, tedious and incredibly detailed explanation of the property
here, in multiple paragraphs and with lists if needed.



### Methods

#### getAllGroupIds() -> (groupIds)


This method returns an array of strings. Each string is the name of a groupId.

Input arguments:

  * **none**

Output arguments:

  * **groupIds** --- string[] --- an array of strings


#### createGroup(groupId)


This method creates a group named by the passed groupId string

Input ArgumentsL
* **groupId** --- string --- the name of the group to be created.

Errors raised by this method:
* org.alljoyn.locationservices.Error.GroupExists --- the group already exists


#### destroyGroup(groupId)

This methods destroys a group.

Input Arguments:
* **groupId** --- string --- the name of the group to destroy.

Errors raised by this method:
* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.

#### joinGroup(groupId, entity)

This method adds an entity to a group.

Input Arguments
* **groupId** --- string --- the name of the group
* **entity** --- entity --- the entity to add to the group

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.

#### leaveGroup(groupId, entity)

This method removes an entity from a group

Input Arguments
* **groupId** --- string --- the name of the group
* **entity** --- entity --- the entity to add to the group

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.

#### getGroupEntities(groupId) -> (entites)

This method returns all of the entities in a group.

Input Arguments:
* **groupId** --- string --- the name of the group

Output Arguments:
* **entities** --- entity[] --- a list of enetities that are members of the group.

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.


#### setEntityDefaultPrivacyPolicy(entity, policy)

This method applies a default policy to an entity.

Input Arguments:
* **entity** --- entity --- the entity
* **policy** --- defaultPrivacyPolicy -- the policy to apply to the entity.

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

#### setGrouoDefaultPrivacyPolicy(groupId, policy)

This method applies a default policy to an entity.

Input Arguments:
* **groupId** --- entity --- the group
* **policy** --- defaultPrivacyPolicy -- the policy to apply to the group.

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the entity does not exist.

#### entityWhitelistAdd(entity, whitelist)

This method adds application IDs to an entities whitelist.

Input Arguments:
* **entity** --- entity --- the entity
* **whitelist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

#### entityBlacklistAdd(entity, whitelist)

This method adds application IDs to an entities blacklist.

Input Arguments:
* **entity** --- entity --- the entity
* **blacklist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

#### groupWhitelistAdd(groupId, whitelist)

This method adds application IDs to a groups whitelist.

Input Arguments:
* **group** --- string --- the group
* **whitelist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.

#### groupBlacklistAdd(groupId, whitelist)

This method adds application IDs to a groups blacklist.

Input Arguments:
* **group** --- group --- the group
* **blacklist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.

#### entityWhitelistDelete(entity, whitelist)

This method deletes application IDs from an entities whitelist.

Input Arguments:
* **entity** --- entity --- the entity
* **whitelist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

#### entityBlacklistDelete(entity, blacklist)

This method deletes application IDs from an entities blacklist.

Input Arguments:
* **entity** --- entity --- the entity
* **blacklist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

#### groupWhitelistDelete(group, whitelist)

This method deletes application IDs from a groups whitelist.

Input Arguments:
* **group** --- group --- the group
* **whitelist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.GroupDoesNotExist -- the group does not exist.

#### groupBlacklistDelete(group, blacklist)

This method deletes application IDs from a groups blacklist.

Input Arguments:
* **group** --- group --- the group
* **blacklist** --- string[] --- a list of application IDs.

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

### Structures
#### entity

|-----------------------|-----------------------------------------------------------------------|
| field Name            | entityId      | string | The assigned identifier for the entity       |
| field Name            | entityMac     | string | The physical address of the entity (MAC)     |

Entities are objects that are tracked by the location services. An entity may or may not be a member of the AllJoyn bus. An entity may be known or anonymous. A known entity is registered with the service by an application. Entities that are discovered, and not pre-registered are designated as unknown entities.
An entity is comprised of two strings, a unique identifier and a media specific identifier. The media specific identifier will normally be the MAC address. Location Services uses both the unique identifier and the media specific identifier when matching entities. Anonymous entities will have a unique identifier of B5872451-E0C2-473F-B7C7-BB96D5A749A0 and a media specific identifier of their MAC address. 

#### defaultPrivacyPolicy
|---------------|-------------------|-------|---------------------------------------------------|
| Field Name    | presencePolicy    | int32 | enum of ALLOW, DISALLOW or CALLBACK               |
| Field Name    | proximityPolicy   | int32 | enum of ALLOW, DISALLOW or CALLBACK               |
| Field Name    | locationPolicy    | int32 | enum of ALLOW, DISALLOW or CALLBACK               |
| Field Name    | containmentPolicy | int32 | enum of ALLOW, DISALLOW or CALLBACK               |

Location Services uses privacy policies to protect the privacy of entities. The policy controls the disbursement of presence, proximity, location or containment information. Each entity has a default policy, a white list and a black list for each of the four services. The default policy can be ALLOW, DISALLOW or CALLBACK. In the case of a CALLBACK, a callback is made to the creator of the entity to determine if the entities information can be disbursed. The white list and black list contain application names. Queries and subscriptions from applications on the entities white list will always be fulfilled; queries and subscriptions from applications on the entities black list will be fail. Queries and subscriptions from applications not on either list will follow the entities default policy. If policy has not been applied to an entity, the default policy will be ALLOW. Privacy policies can also be applied to groups, the only difference is that the default policy for a group cannot be CALLBACK. If there is a disparity between a group’s policy and the policy of an entity, the entities policy takes precedence.
Note that CALLBACK does cannot be applied to privacy policy on an entity group.

### Interface Errors



The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                                            | Error message                             |
|-------------------------------------------------------|-------------------------------------------|
| org.alljoyn.locationservices.Error.EntityDoesNotExist | The entity was not found in the service   |
| org.alljoyn.locationservices.Error.GroupDoesNotExist  | The group was not found in the service    |
| org.alljoyn.locationservices.Error.GroupExists        | The group already exists                  |

# org.allseen.LocationServices.Presence version 1
## Presence Service

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |


### Properties

#### version

|-----------------------|----------------------------------------------------------------------|
| Type                  | q                                                                    |
| Access                | read-only                                                            |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = true       |

### Methods

#### subscribeForEntity(entity)

Receive notifications if an entity enters or exits the proximal network.

Input Arguments:

* **entity** --- entity --- the entity

#### subscribeForEntityGroup(groupId)

Recieve notifications if any entity in the group enters or exits the proximal network.

Input Arguments:

* **groupId** --- string --- the name of the group

#### subscribeForEntityList(entities)

Receive notifications if any entity on the list enters or exits the proximal network.

Input Arguments:

* **entities** --- entity[] --- the list of entities

#### subscribeAll()

Recieve notifications if any entity enters or leaves the proximal network.

#### unsubscribeForEntity(entity)

Remove an entity subscription

Input Arguments:
* **entity** --- entity --- Stop recieving notifications for this entity

#### unsubscribeForEntityGroup(groupId)

Remove a subscription for an entity group

Input Arguments:
* **groupId** --- string --- Stop recieving notifications for entities in this group.

#### unsubscribeAll()

Stop recieving notifications of entities entering and exiting the proximal network.

#### setPresence(entity, isPresent)

Set the presence of an entity

Input Arguments:
* **entity** --- entity --- the entity
* **isPresent --- boolean --- True for present, false for not present

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

#### queryPresence(entity) -> (present)

Query for the presence of an entity.

Input Arguments:
* **entity** --- entity --- the entity.

Output Arguments:
* **present** --- boolean --- true for present false for not present

Errors raised by this method:

* org.alljoyn.locationservices.Error.EntityDoesNotExist -- the entity does not exist.

### Signals

#### present -> (entity, present)

|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This signal is sent to notify the entrance or exit of an entity on the proximal network.



Output arguments:

  * **entity** --- entity --- the entity that entered or exited the proximal network
  * **present** --- boolean --- true for present, false for not present.


## References

In this section, list references for the Interface definition (standards, RFCs, related interfaces, etc.).
Make sure to include at least the following references:

  * locationservices.xml  - the xml description of these interfaces.
  * 