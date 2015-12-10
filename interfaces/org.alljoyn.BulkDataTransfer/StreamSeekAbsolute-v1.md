# org.alljoyn.BulkDataTransfer.Stream.Seek.Absolute version 1

## Theory of Operation

This interface defines the absolute seek operations for streaming bulk data
transfers.  This is a supplemental interface to the
org.alljoyn.BulkDataTransfer.Stream.Seek interface and expects that interface to
be likewise implemented since it provides the SeekComplete() signal.

Because absolute seeking requires an absolute start point, this interface is
typically only useful for bounded data streams.

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties


### Methods

#### SeekAbsolute(position)

As the name implies, this method changes the current position within the
streaming data that will be sent next to an absolute position within bounded
data.  Because this seeks to an absolute position, pausing is not necessary to
move to a precision location.

Input parameters:

  * **position** --- uint64 --- The absolute target position relative to the
    beginning of the streamed data.

Output paremeters:

_None_

Errors raised by this method:

  * org.alljoyn.BulkDataTransfer.Error.InvalidPosition --- Attempted to seek
    past the beginning or the end of the available data.  Note to implementors
    of this interface: if it is possible to seek in the desired direction
    requested but the position is simply too far, it is preferred that the seek
    successfully complete to the beginning or end respectively rather than
    returning this error.  This error should only be delivered if the current
    postion is already at the beginning or end and the requested seek is to go
    beyond that position. 

### Signals

_None_

### Interface Errors

| Error name                                         | Error message                                                 |
|----------------------------------------------------|---------------------------------------------------------------|
| org.alljoyn.BulkDataTransfer.Error.InvalidPosition | Attempt seek past the beginning or end of the available data. |


## References

 * The XML definition of the [StreamSeekAbsolute interface](StreamSeekAbsolute-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Stream interface](Stream-v1)
 * The description of the [StreamSeek interface](StreamSeek-v1)
