# org.alljoyn.Multimedia.Source version 1

## Theory of Operation

This interface controls the lifetime of bus objects that implement
org.alljoyn.BulkDataTransfer interfaces.  It is distinct from the
SourceDiscovery interface which deals exclusively with media lookup.  The
separate allows for other media lookup and discovery interfaces to be developed
without the need to alter this interface.  Besides, a media sink only needs the
functionality in this interface while a media controller only needs the
functionality in the SourceDiscovery interface.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

_None_

### Methods

#### OpenMedia(mediaUri) -> busObjectPath

This creates a new bus object from which a media sink can receive media for
rendering.  If multiple sinks call this method with the same mediaUri, the same
busObjectPath will be returned.  Each time a given media is opened, the media
source will reference count the open operations so that it does not remove the
bus object until all sinks have completed cease receiving media data.

Input arguments:

  * **mediaUri** --- string --- A URI referencing media the source is capable of
    streaming.  Generally, this will be determined by a controller using the
    SourceDiscovery interface.

Output arguments:

  * **busObjectPath** --- object_path --- The object path to the bus object
    implementing the org.alljoyn.BulkDataTransfer.Stream interface.  The bus
    object may implement the org.alljoyn.BulkDataTransfer.Stream.Seek and
    org.alljoyn.BulkDataTransfer.Stream.Seek.Absolute interfaces as well.

Errors raised by this method:

  * **org.alljoyn.BulkDataTransfer.Error.NotFound** --- This indicates that
    either the mediaUri is not known.


#### CloseMedia(busObjectPath)

This lets the media source know that the given sink no longer needs access to
the media data.  It is strongly recommended that sinks call this prior to
leaving the session rather than just leaving the session.  If busObjectPath is
invalid, it will be silently ignored.  The media source will keep track of which
media URIs were opened on which sessions and behave as though this method was
called in the event that the session is severed unexpectedly.  Once all sinks
have closed their access to a media bus object, the bus object will be removed
from the system.

Input arguments:

  * **busObjectPath** --- object_path --- The object path to the bus object
    implementing the org.alljoyn.BulkDataTransfer.Stream interface.

Output arguments:

_None_

Errors raised by this method:

_None_


### Signals

_None_

### Interface Errors

| Error name                                  | Error message       |
|---------------------------------------------|---------------------|
| org.alljoyn.BulkDataTransfer.Error.NotFound | mediaUri not known. |


## References

  * The XML definition of the [SourceSelection interface](SourceSelection-v1.xml)
  * The Multimedia [Theory of operation](theory-of-operation)
  * The description of the [SourceDiscovery interface](SourceDiscovery-v1)
  * The description of the [Multimedia Metadata Schema](Metadata-v1)
  * The Bulk Data Transfer [Theory of operation](/org.BulkDataTransfer/theory-of-operation)
  * The description of the [org.BulkDataTransfer.Stream interface](/org.BulkDataTransfer/Stream-v1)
