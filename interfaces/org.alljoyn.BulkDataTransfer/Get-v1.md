# org.alljoyn.BulkDataTransfer.Get version 1

## Theory of Operation

This interface defines the Get operation for Bulk Data Transfer.  It will be
implemented on bus objects representing media/files of a predetermined size that
will be retrieved by client applications.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

#### Version

|            |                                                                |
|------------|----------------------------------------------------------------|
| Type       | uint16                                                         |
| Access     | read-only                                                      |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true        |

The interface version.
The EmitsChangedSignal of this property should be modified to const once that
feature is available.


### Methods

#### Get(startOffset, size) -> (dataSegment)

This retrieves a block of data from the server starting at the position
indicated by startOffset and containing no more data than indicated by size.

Input arguments:

  * **startOffset** --- uint64 --- The starting position in bytes within the
    media/file for the block of data to retrieve.
  * **size** --- uint32 --- The maximum number of bytes to retrieve.  While the
    server should make every attempt to provide the requested number of bytes,
    it may return fewer if necessary, but may never return more.

Output arguments:

  * **dataSegment** --- byte[] --- The requested block of data.  The array size
    shall not exceed the requested size.

Errors raised by this method:

 * org.alljoyn.BulkDataTransfer.EndOfData --- startOffset is past the end of
   data.


### Signals

This interface does not define any signals.


### Interface Errors

| Error name                             | Error message                         |
|----------------------------------------|---------------------------------------|
| org.alljoyn.BulkDataTransfer.EndOfData | Attempt to read past the end of data. |


## References

 * The XML definition of the [Get interface](Get-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Information interface](Information-v1)
 * The description of the [Put interface](Put-v1)

