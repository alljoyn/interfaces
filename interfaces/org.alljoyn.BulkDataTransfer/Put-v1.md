# org.alljoyn.BulkDataTransfer.Put version 1

## Theory of Operation

This interface defines the Put operation for Bulk Data Transfer.  It will be
implemented on a bus object representing media/files of a predetermined size
that will be written by client applications.

When a client wishes to append data to what a server stores, it should simply
use the Put method with **startOffset** set to the same value as the **size**
property from the Information interface.  Actually, any Put operation where the
size of **dataSegment** plus **startOffset** exceeds the value in the **size**
property will cause new data to be appended.

Some systems may support sparse data sets.  That is, in some cases, it
may be possible to write data to an arbitrary location beyond the current end of
the data set.  In such cases, the Put method and Truncate methods will not
return an error when given an offset that is past the end of the data.

There may be cases where the size of the data stored by the server cannot be
changed or is somehow limited to either a maximum size, a minimum size, or both.
In the case of a maximum size limit where **startOffset** plus the size of
**dataSegment** exceeds the maximum size, the server will ignore excess data at
the end of **dataSegment** once the stored data has reached the maximum size.
In that case the returned size will reflect the actual amount of data stored.
If **startOffset** has the same value as the maximum data size then none of the
data in **dataSegment** will be stored and the returned size will be 0.

The behavior of a server in the event of an unexpected lost session (session was
lost before client called the Close method) will largely be implementation
dependent.  For example a data logger will most likely want to keep the data
received when the session was lost since there may be a clue in the logs as to
why the session was lost.  On the other hand, the data being written may be an
executable program that is completely useless if it was not completely
transferred.



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

#### SparseDataSetsSupported

|            |                                                                |
|------------|----------------------------------------------------------------|
| Type       | boolean                                                        |
| Access     | read-only                                                      |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true        |

If this interface supports writing of sparse data sets then this is true.
The EmitsChangedSignal of this property should be modified to const once that
feature is available.


### Methods

#### Put(startOffset, dataSegment) -> (size)

This sends a block of data to the server starting at the position indicated by
startOffset. In the case where a server supports sparse datasets the
org.alljoyn.BulkDataTransfer.EndOfData error will not be raised if an attempt
is made to write data with the startOffset beyond the end of the data.

Input arguments:

  * **startOffset** --- uint64 --- The starting position (in bytes) within the
    media/file for the block of data to be written at.  **startOffset** may have
    any value between 0 and the value in the **size** property of the Information
    interface.
  * **dataSegment** --- byte[] --- The block of data to write.

Output arguments:

  * **size** --- uint32 --- The number of bytes actually written.
    This cannot be greater than the size of dataSegment but may be
    less if the server could not write the complete contents of
    dataSegment for whatever reason.

Errors raised by this method:

 * org.alljoyn.BulkDataTransfer.EndOfData --- **startOffset** is past the end of
   data (i.e. **startOffset** is greater than the value in the size property
   from the Information interface).
 * org.alljoyn.Error.InvalidValue --- **newSize** was not a valid size for the
   server. This could include either a maximum or minimum size.

#### Truncate(newSize)

This trims the media/file to the specified size. If for any reason the server is
unable to support truncation of a file to the size specified by **newSize** then
it will fail with the org.alljoyn.BulkDataTransfer.InvalidArgument error and
the truncate operation will not occur.

Input arguments:

  * **newSize** --- uint64 --- New size in bytes of the media/file.  Must be
    less than or equal to the current size prior to calling this method.

Errors raised by this method:

 * org.alljoyn.BulkDataTransfer.EndOfData --- **newSize** is past the end of
   data (i.e. **newSize** is greater than the value in the size property from
   the Information interface).
 * org.alljoyn.Error.InvalidValue --- **newSize** was not a valid size for the
   server. This could include either a maximum or minimum size.


### Signals

This interface does not define any signals.


### Interface Errors

| Error name                                   | Error message                                   |
|----------------------------------------------|-------------------------------------------------|
| org.alljoyn.BulkDataTransfer.EndOfData       | Attempt to write past the end of data. If the   |
|                                              | server supports sparse data sets then this      |
|                                              | indicates that there was an attempt to write    |
|                                              | past the maximum size supported by the server.  |
|                                              |                                                 |
| org.alljoyn.Error.InvalidValue               | An invalid value was passed in to one of the    |
|                                              | methods.                                        |


## References

 * The XML definition of the [Put interface](Put-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Information interface](Information-v1)
 * The description of the [Get interface](Get-v1)

