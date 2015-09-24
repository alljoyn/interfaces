# org.alljoyn.FileTransfer.Write version 1

## Specification

|                       |                                               |
|-----------------------|-----------------------------------------------|
| Version               | 1                                             |
| Annotation            | org.alljoyn.Bus.Secure = true                 |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true  |

The interface version number.

#### MaxWriteLength

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint32                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true  |

The total amount of write space available in bytes.


### Methods

#### StartChunkWrite(length, requestedChunkSize) -> (chunkSize)

Begins a file write operation with a service.  Calling this again from 
the current session before writing all data restarts the write operation.

Input arguments:

  * length --- uint32 --- number of total file bytes to write to the producer
  
  * requestedChunkSize -- uint32 -- the maximum chunk size that the consumer
    can accept. If the requested chunkSize is 0 or greater than the maximum 
    supported AllJoyn array size, the maximum supported array size will be 
    requested.
  
Output arguments:

  * chunkSize --- uint32 --- the maximum chunk transfer size in bytes.  This 
    cannot exceed the requestedChunkSize and is typically, but not 
    required to be, the producer service's non-volatile storage allocation unit
    size (e.g., 512, 1024, 2048, 4096 etc.).  This value must be greater 
    than 0.

Errors returned by this method:

 * org.alljoyn.FileTransfer.Error.NotSupported
   If write operation is not supported.
   
 * org.alljoyn.FileTransfer.Error.ResourceLocked
   If another read or write operation is already in progress.
   
 * org.alljoyn.FileTransfer.Error.OpenFailed
   If file could not be created or opened for write.
   
 * org.alljoyn.FileTransfer.Error.LengthInvalid
   If length argument is 0.
   
 * org.alljoyn.FileTransfer.Error.LengthTooLarge
   If length argument is too large for the destination device. See 
   MaxWriteLength property.
   

#### WriteNextChunk(buffer)

Writes a chunk sized (or less) data buffer to a producer.  This method should 
be repeatedly called with a new buffer of data until all bytes are copied.

Input arguments:

  * buffer --- uint8[] --- the byte array of data to write to the producer
    not greater than the chunk size established by the StartChunkWrite method.

Errors returned by this method:
      
  * org.alljoyn.FileTransfer.Error.WriteFailed
    If file could not be written.  This error ends the current write transfer.

  * org.alljoyn.FileTransfer.Error.WriteNotStarted
    No write operation is in progress for current session, or if a transfer has
    already finished.    

  * org.alljoyn.FileTransfer.Error.BufferTooLarge
    The buffer length passed was larger than the chunk size. This error ends 
    the current write transfer.
    
  * org.alljoyn.FileTransfer.Error.TooMuchData
    The number of bytes passed was greater than the remaining bytes to write. 
    This error ends the current write transfer.
    
  * org.alljoyn.FileTransfer.Error.TooLittleData
    The number of bytes passed was smaller than the remaining bytes to write. 
    This error ends the current write transfer.
    
    
#### StopChunkWrite

Terminates the in progress write operation of the current session

Errors returned by this method:
   
  * org.alljoyn.FileTransfer.Error.OperationNotStarted
    No read or write operation was currently in progress
    
    
### Signals

None

## References

See [FileTransfer-Thops.md] for Theory of Operations
See [FileTransfer.Write-v1.xml] for write interface
