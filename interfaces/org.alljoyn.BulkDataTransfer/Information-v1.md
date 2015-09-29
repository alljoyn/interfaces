# org.alljoyn.BulkDataTransfer.Information version 1


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

#### Size

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint64                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

This indicates the size of the data/media/file being transfered.  In the case
that a client is writing new data, the size will be updated to reflect the size
as of the most recent Put or Truncate operation on this specific bus object
only.  Other, read-only bus objects that may refer to the same data will remain
unchanged.

#### Sha256Hash

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | byte[]                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

This provides the SHA-256 hash computed over the contents of the data/media/file
being transferred.  The length of the byte array will always be exactly 32
bytes.  This can be used to verify the whether the data was successfully
transferred or not.  In the case of a client writing data to the server, this
will be updated with the SHA-256 hash of the contents as of the most recent Put
or Truncate operation for this bus object only.  Other, read-only bus objects
that may refer to the same data will remain unchanged.


### Methods

There are no methods defined by this interface.

### Signals

There are no signals defined by this interface.

## References

 * The XML definition of the [Information interface](Information-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Get interface](Get-v1)
 * The description of the [Put interface](Put-v1)

