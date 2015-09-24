# Theory of Operation

The org.alljoyn.FileTransfer.Write and org.alljoyn.FileTransfer.Read 
interfaces, heretofore referred to as the File Transfer Interfaces,
provide a standardized mechanism for exchanging fixed size binary data
between an AllJoyn consumer and an AllJoyn producer. Example of such data
are managed files that are typically larger than maximum allowed in a single
method call. These interfaces are intended to be a generic chunked bulk data 
transfer mechanism for read and write operations.

Currently, AllJoyn provides an alternative mechanism for transferring large 
amounts of data.  The BusAttachment object supports the "GetSessionFd()" API.
This method returns a file descriptor that can be used for data transfer when
a raw session is created. Note that a raw session is required to use the file 
descriptor returned by GetSessionFd() and GetSessionFd() does not conform to 
the AllJoyn security architecture as it is not called as part of a secure 
interface.
 
The File Transfer Interfaces perform file transfers by transferring data in 
binary chunks.  The chunk size cannot exceed the standard AllJoyn array limit,
and negotiates down to the smallest maximum size that the producer and consumer
can accept.  For example, an AllJoyn producer might incorporate a flash memory 
with a block size of 4096 bytes.  A consumer may have enough resources to 
handle 128kbyte chunks.  During the transfer initiation phase, the consumer 
will request the maximum chunk size it can support, and then the producer will 
respond with the smaller of the consumer or producer's chunk size.  In this 
example, the producer would return 4096.

AllJoyn producers that implement this interface may have the need to expose 
multiple files (generally expected to be less than 5).  To support this, an 
AllJoyn producer would create a Bus Object for each file.  For example, the foo
device could expose an object for upgrading its alarm song by creating the 
org.alljoyn.FileTransfer.Write interface on the /foo/alarmsong bus object, and 
could expose another object for upgrading its display images by creating the 
/foo/theme bus object.

The File Transfer Interfaces permit one active read or write operation on a bus
object per session.  For example, if a read operation is started during a 
session on the /foo/alarmsong bus object, the next write or read operation in 
the same session can not start until the current read operation completes.  If 
desired, the producer that implements this interface may allow a separate 
concurrent session to start a concurrent read operation on the same bus object.
However, no readers are permitted while any one session is actively writing.  
Likewise, no write may begin if any readers are active.  

To perform a read operation, a consumer first calls the StartChunkRead method 
on a producer's bus object with its requested chunk size.  The producer 
returns the total length (Length) of the data file, and the chunk size 
(ChunkSize) that will be used.  The consumer repeatedly issues the 
ReadNextChunk method until Length bytes have been read from the producer.  The
size of each buffer read by the ReadNextChunk method will be ChunkSize bytes 
except for the last invocation, which will only have the remaining bytes.  The
file is guaranteed to be delivered sequentially.  For example, if the ChunkSize
is 4096 and the Length is 8193, the first invocation of ReadNextChunk delivers 
4096 bytes from file offset 0-4095, the second invocation of ReadNextChunk 
delivers 4096 bytes from file offset 4096-8191, and the final invocation of 
ReadNextChunk delivers 1 byte from file offset 8192. The file offset is not 
exchanged and is internally tracked by both the consumer and producer.

To perform a write operation, a consumer first calls the StartChunkWrite method
on a producer's bus object with its requested chunk size.  The consumer passes 
the total length (Length) of the data file to the producer and the producer 
returns the chunk size (ChunkSize) that should be used during the transfer.  
The consumer, repeatedly issues the WriteNextChunk method until Length bytes 
have been written to the producer.  The size of each buffer written by the 
WriteNextChunk method will be ChunkSize bytes except for the last invocation, 
which will only have the remaining bytes.  The consumer must deliver the file 
sequentially, waiting for each WriteNextChunk method call to return.  For 
example, if the ChunkSize is 4096 and the Length is 8193, the first invocation
of WriteNextChunk delivers 4096 bytes from file offset 0-4095, the second 
invocation of WriteNextChunk delivers 4096 bytes from file offset 4096-8191, 
and the final invocation of WriteNextChunk delivers 1 byte from file offset 
8192. The file offset is not exchanged and is internally tracked by both the
consumer and producer.

In the event that a write operation fails there is no means to resume the
write operation.  A full resend by the consumer is required.  The producer
should disregard all data written.  The producer should also implement measures
that ensure a failed write does not compromise operational integrity.
For example, writing new firmware to a device should be performed in such a
way that the firmware is written to a temporary buffer, and then swapped into
place or applied after all write operations are finished.

In the event that a session is lost during a transfer, the producer must 
release any resources acquired during the transfer.  For example, if a data
file was locked for read, then the file should be released.  If a temporary
file was created for write, then the temporary file should be deleted.

To mitigate Denial-of-Service attacks, the producer should incorporate a 
Transfer Progress Timer.  The purpose of the timer is to ensure that the
consumer advances a transfer at a reasonable pace.  The producer starts the
timer when either the StartChunkRead or StartChunkWrite methods are 
successfully called by the consumer.  If ReadNextChunk or WriteNextChunk are 
not called before the timer expires, then the producer should assume the 
transfer was aborted.  At the conclusion of each successful ReadNextChunk or
WriteNextChunk the timer should be reset.  The timeout period should be long 
enough to allow sufficient time for a consumer to process a chunk of data, but
short enough to prevent the producer from starving other consumers of a 
producer's resources.

During the transfer it may be necessary for a consumer to stop an active 
file transfer.  The org.alljoyn.FileTransfer.Write interface provides a 
StopChunkWrite method.  During an active file write session, the consumer 
calls StopChunkWrite to abandon the transfer.  In response, the producer 
aborts the transfer, stops the Transfer Progress Timer, and unlocks any
resources that were locked at the start of the write.  Likewise, 
the org.alljoyn.FileTransfer.Read interface provides a StopChunkRead method. 
During an active file read session, the consumer can prematurely abandon
a read by invoking the StopChunkRead method.  In response, the producer,
also aborts the transfer for the current session.