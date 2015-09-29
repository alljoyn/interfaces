# org.alljoyn.BulkDataTransfer.Get version 1

## Theory of Operation

This interface defines the Get operation for Bulk Data Transfer.  It will be
implemented on bus object representing media/files of a predetermined size that
will be retrieved by client applications.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

### Properties

This interface does not define any properties.


### Methods

#### Get(startOffset, size) -> (dataSegment)

This retrieves a block of data from the server starting at the position
indicated by startOffset and containing no more data than indicated by size.

Input arguments:

  * **startOffset** --- uint64 --- The starting position within the media/file
    for the block of data to retrieve.
  * **size** --- uint32 --- The maximum number of bytes to retrieve.
    While the server should make every attempt to provide the requested
    number of bytes, it may return fewer if necessary, but may never
    return more.

Output arguments:

  * **dataSegment** --- byte[] --- The requested block of data.  The
    array size shall not exceed the requested size.

Errors raised by this method:

 * org.alljoyn.BulkDataTransfer.EndOfFile --- startOffset is past the
   end of file.


### Signals

This interface does not define any signals.


### Interface Errors

| Error name                             | Error message                         |
|----------------------------------------|---------------------------------------|
| org.alljoyn.BulkDataTransfer.EndOfFile | Attempt to read past the end of file. |


## References

 * The XML definition of the [Get interface](Get-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Information interface](Information-v1)
 * The description of the [Put interface](Put-v1)

