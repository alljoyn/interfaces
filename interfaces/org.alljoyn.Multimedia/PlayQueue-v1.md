# org.alljoyn.Multimedia.PlayQueue version 1

## Theory of Operation

This interface defines a play queue that gets implemented on a sink.  A play
queue is distinct from a play list in that a play queue is ephemeral and only
contains what is currently scheduled to be played whereas a play list is a
collection of media that gets played as a group and is saved for replaying as a
group at a later time.  Often, all the media in a play list will be added to a
play queue in one shot.

Due to the message size limit for AllJoyn, the play queue cannot be represented
as a simple property.  Thus, it is necessary to provide a more sophisticated
interface to allow for a play queue that is capable of queuing more than a
couple dozen entries.

Operations on the play queue must be transactional.  To accomplish that, a
sequence number (PlayQueueSequence) will be used to identify the current state
of the play queue for operations that query and modify the play queue.  This
ensures that when a media controller attempts to modify the play queue, it will
only be able to modify a play queue in a known state.

A controller may provide the function of skipping between media through
manipulation of the play queue.  For example, to skip forward would be a matter
of removing entries from the head of the queue while skipping backward would be
a matter of adding media then moving that media to the head of the queue.

It is important to note that the play queue does not keep track of what was
previously played.  This means that it is incumbent upon controllers that wish
to provide a skip back operation to keep track of what was previously
played/skipped.  This does introduce the possibility of data race conditions
between multiple controllers if multiple controllers are actively modifying the
play queue.  This, however, should not be viewed as a critical problem.  The
worst that can happen is that one controller will have media in its history that
could be skipped back to that another controller does not.

The alternative, however, would be to make the sink keep track of historical
entries in the play queue which could be both resource intensive and highly
complex.  Ultimately, the ability to skip forward and backward is a UI issue.  A
sink only needs to know what it should be playing now and where to get that
media.

When media gets added to the play queue, a controller provides the bus name of
the source supplying the media and a URI that source understands.  The
implementor of the PlayQueue interface will add a salt value to the added entry
to create a uniquely identifying tuple for the queue entry.  This will allow for
the same media to be added multiple times.  The salt value will remain static
for a given entry so that it can be tracked as it moves around in the play queue.

While much of the description above assumes that this interface would be
implemented by a sink, that is not strictly required.  For example, it should be
possible for a non-sink device to implement this interface and somehow control
actual sinks to stream media from sources.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

#### PlayQueueSequence

This indicates the current state of the play queue in terms of items on the
queue.  Every time media is added, removed, relocated, this property gets
incremented.  When a sink transitions from playing the current media to the next
in the queue due to the current media reaching the end, that will be considered
equivalent to a removal since the head of the queue gets removed so that the
next in line becomes the head.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint32                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |


#### QueueSize

This indicates the number of entries on the play queue.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint32                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |



### Methods

#### GetQueueEntries(startIndex, count) -> (playQueueSequence, queueData)

This retrieves a segment of the play queue.  This will provide enough
information to a controller to look up the media's metadata on the source.  The
information entry information is usable for specific entry identification for
the Remove and Move operations.  The size of queueData may not exceed the number
of entries specified by count.  The size of queueData may contain fewer entries
than was requested by count.

Input arguments:

  * **startIndex** --- uint32 --- Position withing the play queue to get play
    queue data.
  * **count** --- uint32 --- Maximum number of entries to retrieve.

Output arguments:

  * **playQueueSequence** --- uint32 --- The current play queue sequence number
    for which the play queue data applies.
  * **queueData** --- QueueEntry[] --- The play queue entries starting at the
    requested position.

Errors raised by this method:

  * **org.alljoyn.Multimedia.Error.OutOfRange** --- This indicates that the
    startIndex is beyond the end of the queue.


#### Add(playQueueSequence, mediaReferences, position) -> (oldPlayQueueSequence)

This adds media entries to the play queue at the specified position.

Input arguments:

  * **playQueueSequence** --- uint32 --- State of the play queue this addition
    applies to.
  * **mediaReferences** --- MediaReferences[] --- Media information to add to
    the play queue.
  * **position** --- uint32 --- Position in the play queue to insert the media
    entries.  If position is equal to or greater than the QueueSize property,
    the media entries are simply appended to the queue.  If the position is less
    than QueueSize then the media will be added starting at the specified
    position and all existing entries will be moved to after the last entry in
    mediaReferences.

Output arguments:

  * **oldPlayQueueSequence** --- uint32 --- The play queue sequence number at
    the time the Add method was received.  If this equals the passed in value
    for playQueueSequence, then the media was successfully added to the play
    queue.  If oldPlayQueueSequence is different from the passed in value for
    playQueueSequence, then the media was _not_ successfully added.

Errors raised by this method:

  * **org.alljoyn.Multimedia.Error.QueueFull** --- This indicates that the
    requested media could not be added because there is not enough room for all
    the entries.


#### Remove(playQueueSequence, entries) -> (oldPlayQueueSequence)

This removes media entries from the play queue.

Input arguments:

  * **playQueueSequence** --- uint32 --- State of the play queue this removal
    applies to.
  * **entries** --- QueueEntry[] --- Media to remove from the play queue.

Output arguments:

  * **oldPlayQueueSequence** --- uint32 --- The play queue sequence number at
    the time the Remove method was received.  If this equals the passed in value
    for playQueueSequence, then the media was successfully removed from the play
    queue.  If oldPlayQueueSequence is different from the passed in value for
    playQueueSequence, then the media was _not_ successfully removed.


#### Move(playQueueSequence, entries, position) -> (oldPlayQueueSequence)

This removes media entries from the play queue.

Input arguments:

  * **playQueueSequence** --- uint32 --- State of the play queue this move
    operation applies to.
  * **entries** --- QueueEntry[] --- Media to move within the play queue.
  * **position** --- uint32 --- Position in the play queue to move the media
    entries to.  If position is equal to or greater than the QueueSize property,
    the media entries are simply moved to the end of the queue.  If the position
    is less than QueueSize then the media will be moved to starting at the
    specified position and all existing entries will be moved to after the last
    entry in mediaReferences.

Output arguments:

  * **oldPlayQueueSequence** --- uint32 --- The play queue sequence number at
    the time the Move method was received.  If this equals the passed in value
    for playQueueSequence, then the media was successfully moved within the play
    queue.  If oldPlayQueueSequence is different from the passed in value for
    playQueueSequence, then the media was _not_ successfully moved.


### Signals

_None_

### Named Structures

#### struct QueueEntry

  * **sourceBusName** --- string --- Bus name of the source supplying the media.
  * **mediaUri** --- string --- URI of the media on the named source.
  * **salt** --- uint32 --- Unique number assigned to this entry by the
    implementor of this interface.

#### struct MediaReference

  * **sourceBusName** --- string --- Bus name of the source supplying the media.
  * **mediaUri** --- string --- URI of the media on the named source.


### Interface Errors

| Error name                              | Error message                   |
|-----------------------------------------|---------------------------------|
| org.alljoyn.Multimedia.Error.OutOfRange | Value out of range.             |
| org.alljoyn.Multimedia.Error.QueueFull  | No more room in the play queue. |

## References

  * The XML definition of the [PlayQueue interface](PlayQueue-v1.xml)
  * The Multimedia [Theory of operation](theory-of-operation)
  * The description of the [SourceSelection interface](SourceSelection-v1)
  * The description of the [SourceDiscovery interface](SourceDiscovery-v1)
  * The description of the [Multimedia Metadata Schema](Metadata-v1)
