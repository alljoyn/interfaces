# org.alljoyn.Multimedia.Seek version 1

## Theory of Operation

This defines an auxiliary interface to the
org.alljoyn.BulkDataTransfer.Stream.Seek interface to provide the ability to
perform relative seek operations in units of time rather than in units of bytes.
This is due to the fact that it is often more convenient to seek to a time
offset for audio and video media than it is to a specific byte position.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

_None_

### Methods

#### SeekRelative(position)

As the name implies, this method changes the current position within the
streaming data that will be sent next to a relative position.  Due to the
asynchronous nature by which streaming data is delivered, consumers may wish to
pause the stream prior to calling SeekRelative in the event they need to move
the position to a precise location.

Input parameters:

  * **position** --- float --- The target position relative to the current
    streaming position in units of seconds.  Negative values move to earlier
    positions while positive values move to later positions.  +3600.0 seeks
    forward one hour while -0.01 seeks backward 10 ms.

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

#### SeekCompleted(position)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This indicates that a seek operation completed.  This is sent as a sessioncast
signal rather than as a reply to the seek method call so that other members of
the session can be informed of the seek operation.  The next Payload signal sent
will have an offset equal to position.  If the consumer paused the stream prior
to calling this method, it must unpause the stream to resume delivery of data.

Output parameters:

  * **position** --- double --- New position in the data the stream will continue
    at.  This may be negative for unbounded data streams if the seek moved to
    previously recorded data from a live data source.

### Interface Errors

| Error name                                         | Error message                                                 |
|----------------------------------------------------|---------------------------------------------------------------|
| org.alljoyn.BulkDataTransfer.Error.InvalidPosition | Attempt seek past the beginning or end of the available data. |


## References

 * The XML definition of the [StreamSeek interface](StreamSeek-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Stream interface](Stream-v1)
 * The description of the [StreamSeekAbsolute interface](StreamSeekAbsolute-v1)
