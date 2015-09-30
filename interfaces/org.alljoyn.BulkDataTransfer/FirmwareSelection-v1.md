# org.alljoyn.BulkDataTrasfer.FirmwareSelection version 1


## Theory of Operation

This interface defines the means for a device to retrieve a firmware update
image.  There are 2 roles in the firmware update process: the firmware update
provider and the device to be updated.

While the typical AllJoyn service is generally implemented as one or more bus
objects on a device that announce their presence using the About feature and
hosts sessions that are joined by clients, the Firmware Update service operates
differently to keep the number of open sessions to an absolute minimum.

For the Firmware Update service, devices that support firmware updates via
AllJoyn, will announce this capability by adding a special entry to the About
data dictionary.  The key will be "AllJoynFirmwareUpdate".  The value will be a
struct containing a session port number and a list of firmwareIds.  A firmwareId
is the SHA-256 hash of the firmware image and is formatted as an array of bytes
both the About data and in this interface.  In other words, the value signature
will be "(qaay)".

Devices the support updating their firmware via AllJoyn must host a session that
Firmware update providers can join for firmware update delivery.

Firmware update providers will monitor About messages for devices that include
the AllJoynFirmwareUpdate entry in their About data.  The firmware update
providers will keep track of the firmwareIds along with the associated devices
and their session port numbers.

When a firmware update provider gets firmware update image for one of the
firmwareIds it is tracking, it will join a session at the specified session port
for each device that is running that firmwareId.  Once the session is
established, the firmware update provider will send the UpdateAvailable signal
to inform the device about the availability of the firmware update image.

When a device supporting firmware updates via AllJoyn gets an UpdateAvailable
signal, it will call the OpenUpdate method with the firmwareId to be updated.
The device does not need to make that method call immediately, it may wait until
a convenient time to perform the update.  After the device receives the response
with the bus object path for the object containing the updated firmware image,
the device may then proceed to download the firmware image using the
org.alljoyn.BulkDataTransfer.Get interface on that bus object.  The device may
use the Sha256Hash property from the org.alljoyn.BulkDataTransfer.Information
interface to verify the download was successful.

The approach of having the firmware update provider join the session keeps
resource utilization to a minimum.  If a more traditional approach were to be
taken, then the firmware update provider would need to keep sessions to possibly
hundreds of devices alive for a long time in the rare event that an update
becomes available.  For TCP sessions, this means keeping the TCP connection open
and alive.  This could negatively impact low power devices as well as the costs
associated with keeping long lived TCP connection alive on the firmware update
provider.  Using About to announce new firmware versions would mean devices
would need to parse About messages more frequently, often times for firmware
updates that don't apply to them.  The approach described above reduces the
number of changes to the About contents and allows the firmware update provide
control of how many devices it will update at a time.  Also, sessions between the
firmware update provider and devices are only in use for the actual firmware
update data transfer.

Note that all bus objects created by this interface will only implement
org.alljoyn.BulkDataTransfer.Get and org.alljoyn.BulkDataTransfer.Information.
This means there are no contention problems that would arise if the
org.alljoyn.BulkDataTransfer.Put interface were to be implemented as well.  If a
firmware update provider were to implement some other data/media/file discovery
and selection interface as well, it is recommended that bus objects created by
that interface not interfere or interact in any way with bus objects created
during the use of this interface.


## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

### Properties

This interface does not define any properties.


### Methods

#### OpenUpdate(firmwareId) -> (busObjectPath)

This creates a new bus object from which a device can download the updated
firmware image.  If multiple devices call this method with the same firmwareId,
the same busObjectPath will be returned.  Each time a given firmware update
image is opened, the firmware image provider will reference count the open
operations so that it does not remove the bus object until all devices have
completed retrieving the updated image.

Input arguments:

  * **firmwareId** --- byte[] --- A 32 byte array containing the SHA-256 hash of
    the _currently_ running firmware.

Output arguments:

  * **busObjectPath** --- object_path --- The object path to the bus object
    implementing the org.alljoyn.BulkDataTransfer.Get interface

Errors raised by this method:

  * **org.alljoyn.BulkDataTransfer.Error.NotFound** --- This indicates that
    either the firmwareId is not known or that there is no update for the given
    firmwareId.


#### CloseUpdate(busObjectPath)

This lets the firmware image provider know that the device no longer needs
access to the updated firmware image.  It is strongly recommended that devices
call this prior to leaving the session rather than just leaving the session.  If
busObjectPath is invalid, it will be silently ignored.  The firmware image
provider will keep track of which firmware images were opened on which sessions
and behave as though this method was called in the event that the session is
severed unexpectedly.  Once all devices have closed their access to a firmware
udpate bus object, the bus object will be removed from the system.

Input arguments:

  * **busObjectPath** --- object_path --- The object path to the bus object
    implementing the org.alljoyn.BulkDataTransfer.Get interface

Output arguments:

_None_

Errors raised by this method:

_None_


### Signals

#### UpdateAvailable(firmwareId)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This signal informs a device that there is an updated firmware image available
for download.  The firmwareId is provided so that the device knows which
firmware has an update for those devices that have different firmware images for
different subsystems.

Output arguments:

  * **firmwareId** --- byte[] --- A 32 byte array containing the SHA-256 hash of
    the firmware image to be replaced by the update.


### Interface Errors

The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                                  | Error message             |
|---------------------------------------------|---------------------------|
| org.alljoyn.BulkDataTransfer.Error.NotFound | firmwareId not known or has no update available. |


## References

 * The XML definition of the [Firmware Selection interface](FirmwareSelection-v1.xml)
 * The Bulk Data Transfer [Theory of operation](theory-of-operation)
 * The description of the [Information interface](Information-v1)
 * The description of the [Get interface](Get-v1)
