# org.alljoyn.BulkDataTransfer.Put version 1

## Theory of Operation

This interface defines the Put operation for Bulk Data Transfer.  It will be
implemented on bus object representing media/files of a predetermined size that
will be written by client applications.

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

### Properties

This interface does not define any properties.


### Methods

#### Put(startOffset, dataSegment) -> (size)

This sends a block of data to the server starting at the position
indicated by startOffset.

Input arguments:

  * **startOffset** --- uint64 --- The starting position within the media/file
    for the block of data to be written at.  Typically this will point
    to the end of the file.
  * **dataSegment** --- byte[] --- The block of data to write.

Output arguments:

  * **size** --- uint32 --- The number of bytes actually written.
    This cannot be greater than the size of dataSegment but may be
    less if the server could not write the complete contents of
    dataSegment for whatever reason.

Errors raised by this method:

 * org.alljoyn.BulkDataTransfer.EndOfFile --- startOffset is past the
   end of file.

#### Truncate(newSize)

This trims the media/file to the specified size.

Input arguments:

  * **newSize** --- uint64 --- New size of the media/file.  Must be
    less than or equal to the current size prior to calling this
    method.

Errors raised by this method:

 * org.alljoyn.BulkDataTransfer.EndOfFile --- newSize is past the
   end of file.


### Signals

This interface does not define any signals.


### Interface Errors

| Error name                             | Error message                          |
|----------------------------------------|----------------------------------------|
| org.alljoyn.BulkDataTransfer.EndOfFile | Attempt to write past the end of file. |


## References

 * The XML definition of the [Put interface](Put-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Information interface](Information-v1)
 * The description of the [Get interface](Get-v1)

