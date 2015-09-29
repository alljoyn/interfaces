# org.alljoyn.BulkDataTransfer

## Theory of Operation

The interfaces defined under org.alljoyn.BulkDataTransfer enable the transfer of
large, arbitrary datasets.  Ultimately there are 2 kinds of bulk data transfer:

  * Bounded - typically a file of finite size
  * Unbounded - typically a stream of data of indeterminate size

It is expected that there will be some sort of data/media/file discovery and
selection mechanism that will be used to select the data to be transfered.  Such
a selection will result in the creation of a bus object that will implement the
appropriate org.alljoyn.BulkDataTransfer interfaces.

Separating out the discovery and selection mechanism allows for different
mechanisms to be developed that are tailor made for specific usage scenarios.
For example, a firmware update scenario could provide an interface where a
device being updated provides an identifier for its currently running firmware
image and the server would return a bus object for the upgrade image.  In
another case, an interface could allow searching for artist, album, and song
title in a music collection.  In both of those case, there is no need for the
client to know the actual filenames to get the data they are interested in
retrieving.  The one thing that all discovery and selection interfaces will need
to incorporate, however, is a method to get a bus object that represents the
desired data.

No guidance as to the name of the bus object path will be given here.  Because
the interface of the discovery and selection mechanism will need to provide the
bus object path name, the actual path name is irrelevant to the Bulk Data
Transfer interfaces.  Bus Object path names are therefore implementation
defined, though implementations should still follow best practices by providing
bus object path names that are children of the bus object implementing the
discovery and selection interface.

Implementation of BulkDataTranfer will take the form of a standard client/server
model.  The data/file/media server will provide the bus objects that implement
the org.alljoyn.BulkDataTransfer interfaces while the clients will join a
session with the server and use those interfaces.  Should access to a file need
to be secured, that security will be applied to the bus objects that implement
the BulkDataTransfer interface since it is the contents represented by the bus
object that needs to be protected.

Note that the interfaces are defined as insecure to support use cases where
security is not required.  Security shall be applied on a per bus object basis.

### Bounded bulk data transfer

Bulk data transfers of bounded data are typically backed by a file or data store
of some sort.  This can have certain data race implications for scenarios that
support both writing and reading.  This theory of operation cannot mandate
specific behavior in such a case since different scenarios may have their own
required behavior that must be followed.  Such behavioral requirements are
expected to be defined in the theory of operation for the data/media/file
discovery and selection mechanism to which the usage scenario applies.

In the cases where the usage scenario does not dictate the behavior, it is
recommend that the following behavior be adopted.  There are effectively 6
different cases to be considered and for each case the suggested behavior will
be described.

**Case 1: 2 readers.**

This case is the most trivial to handle.  Both readers will access the same
read-only bus object.


**Case 2: 2 writers.**

Each writer should get its own bus object.  Calls to Put on one object will not
affect the contents of the data represented by the other object.  If the server
implements some sort of backing store for the data, the last one to call the
relevant close method will "win".  That is, the server will keep the data from
the last bus object to be closed.


**Case 3: 1 reader, 1 writer, reader accesses data/media/file first.**

In this case the reader will access the data via a read-only bus object that
refers to contents that will remain static regardless of writes done by the
writer.  The writer will get a different bus object that is read/write.  Data
written to this bus object will not affect the contents of the read-only bus
object the reader is using.  In other words, the contents of the read-only bus
object will remain static for the lifetime of that bus object.


**Case 4: 2 readers, 1 writer, 1 reader accesses the file first while the writer
accesses the file second leaving the 2nd reader to access the file third.**

This is really just an extension of Case 3 except that the 2nd reader will use
the same bus object as the first reader.


**Case 5: 2 readers, 1 writer, 1 reader accesses the file first while the writer
access the file second.  The writer closes before the 2nd reader to accesses the
file but the first reader is still accessing the file.**

While this looks very similar to Case 4, the 2nd reader gets a different bus
object in case that corresponds to the new content written by the writer that
completed its operation.


**Case 6: 1 reader, 1 writer, the writer access the file first.**

This is a variation of Case 4 and the reader gets a bus object that represents
the data before the writer opened the file.  This is allows for lock-less file
access while preventing the data from changing unexpectedly for the reader.


To support the behavior described above, the implementation on the server side
would need to create a temporary copy of the file when a file gets opened for
writing.  All writes would go to this temporary file.  When all opened
references to the file have been closed, then it can be moved appropriately in
the underlying filesystem.

### Unbounded bulk data transfer

_This section intentionally left blank.  It is to be filled when the unbounded
bulk data transfer interfaces are defined._

## References

 * The description of the [Information interface](Information-v1)
 * The description of the [Get interface](Get-v1)
 * The description of the [Put interface](Put-v1)
