# org.alljoyn.BulkDataTransfer.Stream version 1

## Theory of Operation

This interface defines the Streaming operation for Bulk Data Transfer.  It will
be implemented on bus objects representing streams for media/files of an
undetermined sizes that will be received by client applications.

As a streaming interface, data will be delivered at a controlled rate.  The
application associated with the type of data will dictate the actual data rate,
once pre-buffering is complete.  It is expected that both the producer and
consumer of the data can handle the requisite data rate.  If the consumer were
to fall behind, implementations should discard data starting with oldest first
to ensure that its receive queues do not overflow or fill up.  Full receive
queues can cause data backups throughout the system.

To give an example of this, consider that streaming uncompressed CD quality
audio (16-bits/sample, 2 channels, 44100 samples per second) will yield a data
rate of 176400 bytes per second.  If the PayloadSize is 400 bytes, then the
Payload signal will be sent 441 times per second, while a PayloadSize of 3600
bytes will result in the Payload signal being sent 49 times per second.

When a stream is started or unpaused, the implementer of this interface will
send Payload signals as fast as possible until the amount of data sent minimally
exceeds the PrebufferSize value.

There may be cases where several Payload signals get sent as a group on a
periodic basis after the pre-buffering is complete.  One such scenario would be
streaming video data.  Video data is typically segmented into frames.  A single
frame of video data may be larger than the specified PayloadSize.  In such a
case the implementer would send multiple Payload signals in rapid succession so
as to deliver a complete frame of video data.

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties


#### PayloadSize

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint32                                                                |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

Controls the maximum amount of data that can be put into a payload signal.  A
payload size of 0 means that the producer is free to send any size payload
(within the limits of the AllJoyn message size).


#### PrebufferSize

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint32                                                                |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

Controls the amount of data initially sent for pre-buffering when the stream
starts and is unpaused.  0, the default value, means that no pre-buffering will
be done.


#### Pause

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | boolean                                                               |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

Indicates if the stream is paused or not.  Implementations must force this to be
false prior the start of streaming.  If the stream cannot be paused, then this
must remain set to false regardless of what gets written to it.  This behavior
is chosen over making this a separate interface since there are cases where
otherwise pause-able streams cannot be paused.  One such example would be a
device that records the last 30 minutes of live data.  The stream can be paused
for up to 30 minutes after which the stream must resume and can no longer be
paused until the device catches up with real time.  (This is what the TiVo DVR
does.)


#### CurrentPosition

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | int64                                                                 |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |

This indicates the current position within a stream.  This may be negative for
data streams that include recorded history from the live data source and a seek
back in history was performed.


### Methods

#### Start()

This informs the producer to begin streaming the data.  The first set of Payload
signals will be sent in rapid succession until the payload offset minimally
exceeds the PrebufferSize value, unless the data is from a live stream and
pre-buffering is not possible.

Only the first call to this method will have any affect.  All subsequent calls
will have no effect.  The Pause property can be used to pause and resume
streaming for data that is pause-able.

((Question: Should this be removed and only the Pause property control streaming?))


### Signals

#### Payload(offset, dataSegment)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This signal delivers media payload data to consumers.

Output parameters:

  * **offset** --- int64 --- The starting position within bounded data being
    streams where the data segment is located.  For unbounded data, this will
    typically be 0.  It may be negative for unbounded data in which case the
    negative value indicates how far back in history the stream data is relative
    to the live data source.
  * **dataSegment** --- byte[] --- This byte array contains the data segment
    being delivered to consumers.


#### EndOfStream()

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This signal indicates that there is no more data to be streamed.  This is sent
after the last segment of bounded data is delivered or the source of live data
has ceased to produce more data.

### Interface Errors

| Error name                             | Error message                         |
|----------------------------------------|---------------------------------------|
| org.alljoyn.BulkDataTransfer.EndOfData | Attempt to read past the end of data. |


## References

 * The XML definition of the [Stream interface](Stream-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [StreamSeek interface](StreamSeek-v1)
 * The description of the [StreamSeekAbsolute interface](StreamSeekAbsolute-v1)
