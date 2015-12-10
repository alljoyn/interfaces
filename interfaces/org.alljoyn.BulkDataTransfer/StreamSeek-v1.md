# org.alljoyn.BulkDataTransfer.Stream.Seek version 1

## Theory of Operation

This interface defines the relative seek operations for streaming bulk data
transfers.  Absolute seeking is defined in an auxiliary interface since any
interface capable of seeking to and absolute position is also capable of seeking
to a relative position.

An example of where relative seeking is possible but absolute seeking is not
would be a feature similar to TiVo's "Pause live TV" capability where the device
maintains a copy of live data for a given amount of time.  In the case of TiVo,
it keeps a copy of the most recent 30 minutes of a TV broadcast.  With this
feature, it is possible to seek back, say 10 minutes, and resume streaming data
captured 10 minutes ago.  It also possible to seek forward from that point to as
far as the current live data allows.  In this scenario, there is no absolute
position to support absolute seeking.


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

  * **position** --- int64 --- The target position relative to the current
    streaming position.  Negative values move to earlier positions while
    positive values move to later positions.

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

  * **position** --- int64 --- New position in the data the stream will continue
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
