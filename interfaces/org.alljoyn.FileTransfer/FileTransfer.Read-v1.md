# org.alljoyn.FileTransfer.Read version 1


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


### Methods
  

#### StartChunkRead(requestedChunkSize) -> (length, chunkSize)

Begins a file read operation with a producer.  Calling this again from the
same session before reading all bytes, restarts the read operation.

Input arguments:

  * requestedChunkSize -- uint32 -- the maximum chunk size in bytes that the
    consumer can accept. If the requested chunkSize is 0 or greater than the
    maximum supported AllJoyn array size, the maximum supported array size will
    be requested.

Output arguments:

  * length --- uint32 --- number of total bytes of file to read.
  
  * chunkSize --- uint32 --- the maximum chunk transfer size in bytes.  This 
    cannot exceed the requestedChunkSize and is typically, but not 
    required to be, the producer service's non-volatile storage allocation unit
    size (e.g., 512, 1024, 2048, 4096 etc.).  This value must be greater 
    than 0.

Errors returned by this method:
  
 * org.alljoyn.FileTransfer.Error.ResourceLocked
   If a write operation is already in progress.  This may also be returned if
   a read operation is already in progress and multiple readers are not 
   allowed by the implementation.
   
 * org.alljoyn.FileTransfer.Error.OpenFailed
   If file could not be opened for read.


#### ReadNextChunk() -> (buffer)

Reads a chunk sized (or less) chunk of data from a producer.  This method should 
be repeatedly called until all bytes are received from the producer.

Output arguments:

  * buffer --- uint8[] --- chunk sized or less array of bytes from the
    producer. 

Errors returned by this method:
   
  * org.alljoyn.FileTransfer.Error.ReadFailed
    If file could not be read.   This error ends the current read transfer.

  * org.alljoyn.FileTransfer.Error.ReadNotStarted
    Indicates that all bytes have already been returned to the caller or that
    a read has never been started for the current session.

    
#### StopChunkRead

Terminates the in progress read operation of the current session

Errors returned by this method:
   
  * org.alljoyn.FileTransfer.Error.OperationNotStarted
    No read or write operation was currently in progress
    
    
### Signals

None

## References

For the corresponding Introspection XML, refer to: FileTransfer-v1.xml

See [FileTransfer-Thops.md] for Theory of Operations
See [FileTransfer.Write-v1.xml] for Write Interface
