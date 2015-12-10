# org.alljoyn.Multimedia

## Theory of Operation

This set of interfaces are intended to enable the discovery and delivery of
multimedia related data.  At the very highest level, there are 3 major roles:
Source, Sink, and Controller.  The design of this set of interfaces is to
support any number of sources, sinks, and controllers.

AllJoyn devices may implement any combination of Source, Sink, and/or
Controller.  A file server may implement only the Source role, while a TV or
receiver may implement the Source and Sink roles.  A phone could implement all
three.

Another major concept to this design is the "Play Queue".  The Play Queue
differs from an ordinary play list in that it is ephemeral in nature unlike a
play list which is a collection of media that is kept and reused many times over
long periods of time.  In essence, the Play Queue contains what is currently
playing and what will automatically be played next once the current media
completes.  For short media files, like songs, the Play Queue will typically
contain many entries.  For long media files, like movies, or unbounded media,
like live streams, the Play Queue will typically only contain the currently
playing media.

### Sources

A source is any AllJoyn bus node capable of supplying multimedia data.  It can
be a library of files, a relay for an internet service, or even devices like
baby monitors or home security web cams.

If a source has enough processing capability, it my provide transcoding of its
media to work with the sink it is delivering media to.

Often times the source will need to understand formats of the media it delivers
in order to extract the relevant metadata to allow the controller to perform
search operations.

### Sinks

A sink is any AllJoyn bus node capable of rendering media supplied by a Source.
Some sinks may only render audio, while others may render both video and audio.

All sinks must implement a play queue that controllers can manipulate.

### Controllers

A controller is any AllJoyn bus node capable of discovering media on a source
and queuing up that media for rendering on a sink.  Generally speaking,
Controllers can only be implemented on devices that offer a user interface that
is capable of conveying sufficient information for a person to select media for
playback and to manipulate the current play queue.

Controllers will be responsible for discovering both Sources and Sinks.

### Media Data Transfer

As discussed in the Bulk Data Transfer theory of operation document, there are 2
different kinds of bulk data transfer: bounded and unbounded.  Because each has
their benefits and problems, both will actually be utilized for delivery of
media.  Which one gets used will depend on various conditions.

#### Bounded Media Delivery

This will only work for delivery of media stored as files in a library.  This is
preferred when there is only one sink rendering the selected media.  This cannot
be used when delivering live streaming media.

The reason it is preferred to streaming is that it allows the sink better
control over when it receives the next block of data to render.  This makes the
sink faster to respond to seeking both forward and reverse as well as jumping to
a specific spot in the media.

Another benefit is this will eliminate perceived delays and gaps caused by the
need to do initial buffering.

((QUESTION: Should we drop bounded media delivery in favor of always using
unbounded media delivery?  This would simplify Sink implementations as well as
avoid the need to decide which transfer mechanism to use.))

#### Unbounded Media Delivery

This is the only option when delivering live media or relaying from streaming
internet services.  Streaming is also the only viable option for delivering
media to multiple sinks simultaneously.

### Play Queue Details

The Play Queue is the central structure around which AllJoyn multimedia playback
is centered.  The Play Queue will have a serial number associated with it that
will change every time the contents of the queue changes.  This include the
addition, removal, and rearrangement of entries in the queue.  The automatic
removal of the currently playing media when it reaches the end is a removal
operation and so the serial number will still be updated for that event.

This serial number allows Controllers to monitor when changes to the Play Queue
happen without the need for the Sink to broadcast an updated play queue to
everyone.

Play Queues will only keep track of the currently playing media and any media
that has not been played yet.  It will not keep track of previously played
media.  Implementations of Controllers are free to do so on their own if they
wish to present that information to their users.  This simplifies the
implementation of Sinks and reduces the amount of information Sinks need to
maintain.  Most Controllers will be complex by the fact that they will need to
implement UIs and so having Controllers keep track of history keeps complexity
where the complexity already exists.

When a media selection is added to a Play Queue, the Sink will assign that
addition a queue ID number that will remain static for the lifetime that entry
remains on the queue.  The queue ID along with the Source and Source's media ID
will uniquely identify an entry on the queue for future reference.  How the
queue ID is generated is implementation dependent, however, implementations
should use a mechanism that avoids assigning the same queue ID number for the
same Source and Source's media ID so that the same media can be queued up
multiple times and each entry can be uniquely identified.

### Common Operations

#### Add Media to Play Queue

For this operation, a Controller will retrieve a static media identifier from a
Source along with relevant metadata and append that information to the Play
Queue of a Sink.  The Sink will assign a queue ID for future reference.  The
issuance of a queue ID allows for a given piece of media to be queued multiple
times while maintaining a unique reference to each queued entry.

#### Remove Media from a Play Queue

For this operation, a Controller will simply send the method call to remove the
desired media from the Play Queue according to the assigned queue ID.

#### Move Queued Media to a New Position

For this operation, a Controller will simply send the method call to move the
desired media to a new position by informing the Sink which media to move
according to its queue ID and the destination position.  Destination positions
past the end will simply move the media to the end of the queue.

#### Skip to Next Queued Media (Skip Forward)

For this operation, a Controller only needs to remove the currently playing
media by referencing the queue ID of the currently playing media.

#### Skip to Previously Queued Media (Skip Back)

For this operation, a Controller will need to keep track of previously played
media, re-add them, then move the re-added media to their appropriate location
at the beginning of the Play Queue.


### Use Cases

#### Sink Handoff

A user has a music library stored on their phone.  They setup a play queue to
listen to while driving home from work.  The music from their play queue plays
over the car's audio system while they are driving.  When they reach home, the
home audio system comes on and the music resumes playing on the home audio
system where it left off in the car.

The phone is operating as both the Controller and the Source in this case.  The
car and home audio systems are operating as Sinks.

To enable this use case, the Controller implementation in the phone will need to
take note of the current play queue and the location within the currently
playing song when the car shuts off so that it can restore that play queue on
the home audio system and seek to the last play location in the currently
playing song.

#### Private Party

A person invites a bunch of friends over for a party.  Each person has a
collection of music on their respective phones.  Each person can use their
respective phones to search for music on everyone's phones to be played.
Everyone has access to the play queue and can add, remove, and rearrange what
music gets played.

(Note, this is the basis of the AllJoyn JamJoyn demo presented by Qualcomm at
CES in 2012.)

Here, one device operates as the Sink while many phones operate as Sources and
Controllers.

#### Dedicated Baby Monitor

While this is effectively a degenerate case for these interfaces, there is no
reason that these interfaces can't be used to enable the dedicated baby monitor
use cases.  In this cases, there are usually 2 devices: the microphone and the
speaker.  While the microphone is obviously the Source and the speaker is
obviously the sink, either device can be implemented with a Controller that is
written to only find it's paired counterpart and set the Sink's (speaker's) play
queue to only render media from the Source (microphone).

This is actually a use case where the Controller does not have any kind of user
interface.

#### Jukebox at a Bar

In many ways, this is similar to the Private Party use case except that the Sink
can only play music from an authorized Source.  This is to ensure compliance
with copyright laws regarding public performances of recorded media.

In this use case, the Jukebox will operate as both the Sink and the Source.
Attempts by Controllers to add music to the play queue from unauthorized source
will be rejected.  Monetization of this service is beyond the scope of the
interfaces defined here.  One possibility could be that a patron would pay a fee
to be added the Security 2.0 group that is authorized to add music to the play
queue.  The rearrange and remove features of the play queue in the Sink would
likely need to be disabled so that paying patrons don't get cheated out of
hearing the selection they payed for.

## References

  * The description of the [SourceSelection interface](SourceSelection-v1)
  * The description of the [SourceDiscovery interface](SourceDiscovery-v1)
  * The description of the [PlayQueue interface](PlayQueue-v1)
  * The description of the [Multimedia Metadata Schema](Metadata-v1)
  * The Bulk Data Transfer [Theory of operation](/org.BulkDataTransfer/theory-of-operation)
  * The description of the [org.BulkDataTransfer.Stream interface](/org.BulkDataTransfer/Stream-v1)
  * The description of the [org.BulkDataTransfer.StreamSeek interface](/org.BulkDataTransfer/StreamSeek-v1)
  * The description of the [org.BulkDataTransfer.StreamSeekAbsolute interface](/org.BulkDataTransfer/StreamSeekAbsolute-v1)
