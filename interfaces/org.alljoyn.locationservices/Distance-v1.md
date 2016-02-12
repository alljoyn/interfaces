# org.alljoyn.LocationServices.Distance version 1
## Specification
This interface contains methods for applications to access the data of the
distance service
The distance service provides data on the relative distance between two entities.
Users of this service can query for the distance between a reference entity and a
set of entities specified by a distance filter.
Users of this service can subscribe to notifications when an entity comes within
a specified distance of another entity, or any member of a group.
or get a notification if an entity moves a specified distance in regards to
another entity.
Distances between entities may not be available for all entities in the Distance
service. 
If entity A and entity B are 4 meters apart and entity B and entity C are 2
meters apart, it is not possible to know the distance between entity C and enity
A. 
The distance service does not calculate the distance between entities, it relies
on the distance contributing device to provide all distance information.

|                       |                                                       |
|-----------------------|-------------------------------------------------------|
| Version               | 1                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                         |


#### Version

|            |                                                                  |
| ---------- | ---------------------------------------------------------------- |
| Type       | uint16                                                           |
| Access     | read-only                                                        |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = const         |

The interface version.  


#### Subscribe(filter, delta) -> (tracker)



Subscribe for events that occur when distance information is available between 
firstEntity and secondEntity.
Events are only sent again if the distance change is  more than delta
(measured in cm)
This method returns a DistanceTracker. 
If a tracker exists that matches the filter it is returned,
otherwise a new tracker object is created.
Trackers have a lifetime and will be reaped after the lifetime is exceeded.

Input arguments:

  * **filter** --- DistanceFilter --- A filter to match entities
  * **delta** --- double --- change in distance threshold for notification
(measured in cm)
  

Output arguments:

  * **tracker** --- object path --- a bus object that implements the
DistanceTracker interface

Errors raised by this method:

#### QueryDistance(filter) -> (entities)

Query for distance information based on the supplied filter

Input Arguments:

* **filter** --- DistanceFilter --- The filter to be used to select the entities to return 

Output Arguments:

* **entites** --- a[DistanceEntity] --- An array of distance entities that match the distanceFilter

### Named Types

#### struct Entity

Entity is a generic representation of a device
Entities are objects that are tracked by the location services.
An entity may or may not be a member of the AllJoyn bus. 
An entity may be known or anonymous. 
A known entity is registered with the service by an application. 
Entities that are discovered, and not pre-registered are designated as unknown entities.
An entity is comprised of two strings, a unique identifier and a media specific identifier. 
The unique identifier is a human readable string.
The media specific identifier is a unique descriptive string or it can be the MAC address if available. 
Location Services uses both the unique identifier and the media specific identifier when matching 
entities. Anonymous entities will have the entityId of 00000000-0000-0000-0000-000000000000
and a media specific identifier.

  * **entityDescriptor** --- string --- A human readable description of the entity
  * **entityId** --- string --- A unique ID associated with an entity (usually a UUID or MAC address)
 
#### struct DistanceEntity

Distance services representation of device information
It contains an entity (a human readable description and a unique id of the
device), a distance (an integer
representing the distance of the device from a reference entity and a
multiplier), and a representation of accuracy (an integer
representing the accuracy of the measurement and a multiplier)

  * **entity** -- Entity -- The entity for the device
  * **distance** --- double --- the distance of the device from a reference entity (in cm.) 
  * **accuracy** --- double --- the accuracy of the measurment. Accuracy is 70 percent confindence 
that the distance is within plus or minus the accuracy value.

#### struct DistanceFilter

Service settings along with selection criteria for specifying entities of interest
The filter contains a regular expression to match the field to match with entities in the distance service.

  * **referenceEntity** -- Entity -- The reference entity is the entity from which distances to other entites are measured
  * **entityList** --- a[Entity] --- An array of regular expressions of entities


## References

  * The xml definition of the [Distance service] (Distance-v1.xml)
  * The [Theory of Operations] (theory-of-operations)
  * The [DistanceTracker] (DistanceTracker-v1)
  * the [DistanceContributor] (DistanceContributor)
