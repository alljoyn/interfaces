# org.alljoyn.Firmware.FirmwareSelection version 1


## Theory of Operation

This interface defines the means for a device to retrieve a firmware update
image.  There are 2 roles in the firmware update process: the firmware update
provider and the device to be updated.

While the typical AllJoyn service is generally implemented as one or more bus
objects on a device that announce their presence using the About feature and
hosts sessions that are joined by clients, the Firmware Update service operates
differently to keep the number of open sessions to an absolute minimum.

For the Firmware Update service, devices that support firmware updates via
AllJoyn will announce this capability by adding a special entry to the About
data dictionary.  The key will be "AllJoynFirmwareUpdate".  The value will be a
struct containing a session port number and a list of firmwareIds.  A firmwareId
is the SHA-256 hash of the firmware image and is formatted as an array of bytes
in both the About data and in this interface.  In other words, the value
signature will be "(qaay)".  Additionally, the entry must be included in the
About announcement sessionless signal.

Devices that support updating their firmware via AllJoyn must host a session that
firmware update providers can join for firmware update delivery.

Firmware update providers will monitor About messages for devices that include
the AllJoynFirmwareUpdate entry in their About data.  The firmware update
providers will keep track of the firmwareIds along with the associated devices
and their session port numbers.

When a firmware update provider gets a firmware update image for one of the
firmwareIds it is tracking, it will join a session at the specified session port
for each device that is running that firmwareId.  Once the session is
established, the firmware update provider will send the UpdateAvailable signal
to inform the device about the availability of the firmware update image.

When a device supporting firmware updates via AllJoyn gets an UpdateAvailable
signal, it will call the OpenUpdate method with the firmwareId to be updated.
Note that the UpdateAvailable signal is a sessioncast signal and not sessionless
signal.  This ensures that the device to be updated gets the signal when the
firmware update provider has a new firmware image to be delivered and has spare
capacity to do so.  The device does not need to make the OpenUpdate method call
immediately, it may wait until a convenient time to perform the update.  It should
be noted that the "push" model of delivering firmware updates is effectively
replicated when a device does respond immediately to the UpdateAvailable signal by
calling OpenUpdate.  After the device receives the response with the bus object
path for the object containing the updated firmware image, the device may then
proceed to download the firmware image using the org.alljoyn.BulkDataTransfer.Get
interface on that bus object.  The device may use the Sha256Hash property from the
org.alljoyn.BulkDataTransfer.Information interface to verify the download was
successful.

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
number of changes to the About contents and allows the firmware update provider
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

While this theory of operation has discussed the 2 roles of firmware image
provider and device being updated, this interface can actually be used to
implement a hierarchy of firmware image providers.  This divides the firmware
image providers into 2 categories: update brokers and canonical firmware image
provider.  An update broker is generally a "middleman" who gets firmware updates
from a canonical firmware image provider and delivers that image to devices on
the local network.  A canonical firmware image provider hosts all relevant
versions of firmware images for a given set of devices.

A canonical firmware image provider will likely be a publicly accessible service
on the Internet.  As such, it may not support AllJoyn for communications.  Thus
update brokers will (often) need to be written to support the communication
protocol used by canonical firmware image providers found on the Internet.

Because canonical firmware image providers host all versions of a firmware
codebase for a given device, it will also need to keep track of which version
replace(s) which other version(s).  While updating from one version to the next
may appear to be a simple chain, from the image provider's perspective it will
actually resemble a tree given that some updates may be able to skip over
versions.

While update brokers will typically download and cache firmware updates for
local distribution, it is not strictly required that update brokers do so.  It
is possible to implement an update broker that is simply a data relay or bridge
to a different firmware update distribution protocol.

Update brokers that do cache firmware images can use the information from the
device's About announcements to determine if all devices have been successfully
updated or not.  If all devices are updated, then the update brokers can remove
the image from its cache.  If another device joins the network that needs an
update that was flushed from the cache, the update broker will need to
re-download the update image again.  A device that was discovered via the About
announcement whose firmware update session port cannot be joined would be
considered out of service and removed from the set of devices to monitor updates
for.

### Possible update broker design examples

#### Mobile app

An update broker implemented in a mobile app could allow the user to choose
when to update the firmware of devices.  It would monitor About messages from
devices on the home network.  After receiving the firmware hash of a device
it would query the canonical firmware image provider.  The canonical firmware
image provider would return the authoritative graph of hashes that apply to
that firmware image, which would designate the allowed firmware update paths.
The graph would provide metadata for each hash that would contain useful
information like the firmware version string and release notes.  This
information would be displayed in the mobile app, allowing the user to see
the current firmware version of a device, and choose to upgrade or downgrade
firmware versions.  Once the user chooses to downgrade or upgrade, the mobile
app would download the correct image, cache it locally, then send it to the
device.

#### Home gateway/router

An update broker implemented in a home gateway/router device could be fully
autonomous.  It would monitor About messages from devices on the home network
along with canonical firmware image providers.  When it finds an updated
firmware image for one of the devices on the home network, it would download
that image from the canonical image provider, cache it locally, then distribute
it to the relevant devices on the home network.

#### Corporate IT

An update broker implemented for corporate environments would still monitor
About messages and canonical image providers for updates.  Instead of automatic
download and distribution, it would notify IT staff.  IT staff could then direct
the update broker to only update a test device to allow the IT staff to vet the
update.  Once the IT staff vetted the update, they can then direct the update
broker to distribute the update company wide on a schedule of their choosing.

A corporate update broker may also cache many versions of firmware images and
act as a pseudo-canonical firmware image provider.  It could even provide a
means for IT do rollback of an update by rearranging the graph that describes
which version updates to which other versions.

#### Friendly neighbor device

Another possibility would be for a device that gets updated to also include
limited update broker functionality.  If a device keeps track of the SHA-256
hashes of the preceding firmware versions for the version it is currently
running, then that device could update its neighbors with its own version.

(Note that this might be in conflict with the needs of a corporate environment.)


### Firmware Hash Graph Notation

With any device there is always a path for transitioning firmware versions.  To
represent firmware upgrade and downgrade paths the DOT graph description
language should be used.  This notation allows for the graph to be rendered in
many commonly-used and broadly available software programs.  It also allows for
widely-available libraries to be used for traversing the graphs (e.g.,
libcgraph).

#### Graph Format Specification

There are three sections in each graph: Image Declaration, Upgrade Path, and
Links.  Each section allows for special attributes to be specified in
addition to reserved DOT attributes.  All string attributes are human-
readable and are used to display information to end-users.  Additional
vendor-specific attributes may be used if desired.

##### Image Declarations

The first section, Image Declarations, lists the hashes that are available in
the graph along with any attributes that are applicable to the image.  For the
purpose of supporting localization each attribute can be repeated by appending
a suffix to the attribute name that indicates the IETF language tag as specified
by RFC 5646.  The language tag always begins after the first hyphen ('-')
character in the attribute name.  Attributes may be provided without a language
tag, in which case they are assumed to be the default value.  Examples:
"notes-de" (German), "name-zh_hans" (Simplified Chinese).  

The attributes in this section are the following:
- **version** or **version-xx** - *string* - the human-readable version of the
  firmware image (e.g., "1.3.0").  This attribute is overridden if also specified
  in a Links section where this hash is present.  The _xx_ version of this
  attribute allows the string to be translated into multiple languages, where _xx_
  indicates the corresponding IETF language tag as specified by RFC 5646.
- **notes** or **notes-xx** - *string* - human-readable notes to describe the
  firmware image.  This may be in normal string format or in valid HTML markup.
  This attribute is overridden if also specified in a Links section where this
  hash is present.  For example: "Release Notes\n\nFixed bug 102 - crash on
  exit\nAdded a neat feature".  The _xx_ version of this attribute allows the
  string to be translated into multiple languages, where _xx_ indicates the
  corresponding IETF language tag as specified by RFC 5646.
- **name** or **name-xx** - *string* - a human-readable name for the image.  In a
  multi-image system this should indicate the partition to which the image will be
  applied.  The _xx_ version of this attribute allows the string to be translated
  into multiple languages, where _xx_ indicates the corresponding IETF language
  tag as specified by RFC 5646.


##### Upgrade Paths

The second section, Upgrade Paths, lists the upgrade or downgrade paths that
the firmware can take.  For example, if it's possible to upgrade from version
1.1 (hash1) to 1.2 (hash2) then the upgrade path is `hash1 -> hash2`.  If it
is also possible to upgrade from 1.2 to 1.3 (hash3) and from 1.1 to 1.3, then
the upgrade paths would be `hash1 -> hash2 -> hash3` and `hash1 -> hash3`.

In addition, downgrade paths can be specified when the *downgrade=true*
attribute is specified. For example, if it is possible to downgrade from
1.3 to 1.2 then the upgrade path declaration for the downgrade would be
`hash3 -> hash2 [downgrade=true]`.

The attributes that may be specified in this section are the following:
- **downgrade** - *Boolean* - (default: false) if true, this specifies that
  this path is a downgrade path only.
- **order** - *integer* - the order in which this upgrade path should be
  applied with respect to other partitions, starting with the least value
  and ending with the greatest value.  This is useful in a multi-image
  system where the order of the application of the images may be important.

##### Links

The third section, Links, is optional.  This section is specified when there
are multiple firmware images to be applied to a single device for an atomic
firmware upgrade operation.  This section allows for creating subgroups of
firmware image nodes, which indicates they are compatible with one another.

When attributes are specified in this section it overrides the same
attributes for the grouped nodes if they were specified in the Image
Declarations section.  This essentially means that if this section is
available it is often unnecessary to use the Image Declarations section to
specify the *version* and *notes* attributes.

- **version** or **version-xx** - *string* - the human-readable version of
  the firmware image (e.g., "1.3.0").  When specified, this value overrides
  the same attribute for each node declared in this group.  The _xx_ version
  of this attribute allows the string to be translated into multiple
  languages, where _xx_ indicates the corresponding IETF language tag as
  specified by RFC 5646.
- **notes** or **notes-xx** - *string* - human-readable notes to describe the
  firmware image.  This may be in normal string format or in valid HTML markup.
  When specified, this value overrides the same attribute for each node
  declared in this group.  The _xx_ version of this attribute allows the string
  to be translated into multiple languages, where _xx_ indicates the
  corresponding IETF language tag as specified by RFC 5646.
- **rank** - the value is always specified as *same* (i.e., `rank=same`).
  This is used to tell the interpreter that these are ranked at the same
  level in the graph.  Without this attribute a DOT notation interpreter will
  not be able to appropriately render Links.


#### Firmware Hash Graph Examples

The following examples show how the DOT notation can be used to represent
firmware upgrade and downgrade paths.  While some of these may seem
complicated to produce, being in the DOT format means that graphical
editors can be used to generate these graphs much more easily than they
can be created by hand.

Note that these examples use hash1, hash2, etc. to reflect that the SHA-256
hash of the images is specified.  In practice this will always be the actual
SHA-256 hash.

##### Simple Firmware Graph

This is a very simple example showing the firmware upgrade tree for a device
that is updated with a single firmware image.

The version attribute indicates, in a human-readable form, the version that
the firmware image represents.  The notes are general human-readable notes
that give any details that the firmware provider wants to allow end-users to
read.

The second section shows that it is possible to upgrade from the first version,
hash1, to the second version, hash2. It also shows that it is possible to
upgrade from the second version, hash2, to the third version, hash3.

    digraph simple_example {
        # Image Declarations
        hash1 [name="Firmware for xyz", version="1.0", notes="first release"];
        hash2 [name="Firmware for xyz", version="1.1", notes="fixed foo"];
        hash3 [name="Firmware for xyz", version="1.2", notes="fixed bar"];
        
        # Upgrade Paths
        hash1 -> hash2 -> hash3;
        
        # Links (none in this example)
    }

##### Skippable Firmware Graph

This example shows how to specify an upgrade path to allow for skipping one
or more versions of firmware upgrades.  This can significantly speed up the
firmware upgrade process if such an upgrade path is possible for the hardware.

In this example it is possible to upgrade the device's firmware to the latest
version (1.2) if the device is currently at version 1.0 or version 1.1.

    digraph skippable_example {
        # Image Declarations
        hash1 [version="1.0", notes="original release"];
        hash2 [version="1.1", notes="fixed foo"];
        hash3 [version="1.2", notes="fixed bar"];
        subgraph {
            name="Firmware for xyz"; hash1; hash2; hash3;
        }
        
        # Upgrade Paths
        hash1 -> hash2 -> hash3;
        hash1 -> hash3;
        
        # Links (none in this example)
    }

Also notice that the *name* attribute is specified in a subgraph.  This is a
DOT language feature that allows one or more attributes to be applied to
several nodes at once.  In this case it prevents the need to write the same
*name* attribute in every hash.  The *version* and *notes* attributes should
not be specified in a subgraph inside the Image Declarations section.

##### Downgradable Firmware Graph

This example shows how to provide firmware downgrade paths.  In this example
it is possible to downgrade from version 1.2 to 1.1, from 1.1 to 1.0, or from
1.2 directly to 1.0.

    digraph downgradable_example {
        # Image Declarations
        hash1 [version="1.0", notes="original release"];
        hash2 [version="1.1", notes="fixed foo"];
        hash3 [version="1.2", notes="fixed bar"];
        subgraph {
            name="Firmware for xyz"; hash1; hash2; hash3;
        }
        
        # Upgrade Paths
        hash1 -> hash2 -> hash3;
        hash1 -> hash3;
        hash3 -> hash2 -> hash1 [downgrade=true];
        hash3 -> hash1 [downgrade=true];
        
        # Links (none in this example)
    }

##### Multi-image Firmware Graph

This example shows how a firmware graph might be specified for a device with
multiple firmware image partitions.  In this example there are two partitions,
the XYZ Device Boot Loader and the XYZ Device Kernel.

The Upgrade Paths section specifies the paths that can be upgraded, and the
order in which the images should be applied to the device.  In this example
the boot loader image must be applied before the kernel.

In multi-image systems the *order* should always be specified for every path,
otherwise they will be randomly applied.

The Links section specifies the images that are linked together for a single
atomic firmware update.  In this example the first subgraph links hash1 and
hash4 together, which means that these two images are applied together.  It
also links hash2 with hash5, and hash3 with hash5 as well.  This means that
hash5 is able to be applied when either hash2 or hash3 are applied.  In this
example there were three updates to the boot loader but only two updates to
the kernel.  The second update of the kernel is compatible with both the
second and the third boot loader images.

    digraph multi_image_example {
        # Image Declarations
        subgraph {
            name="XYZ Device Boot Loader"; hash1; hash2; hash3;
        }
        subgraph {
            name="XYZ Device Kernel"; hash4; hash5;
        }

        # Upgrade Paths
        hash1 -> hash2 -> hash3 [order=1];
        hash3 -> hash2 [downgrade=true, order=1];
        hash4 -> hash5 [order=2];
        
        # Links
        subgraph {
            rank=same; version="1.0"; notes="original release"; hash1; hash4;
        }
        subgraph {
            rank=same; version="1.1"; notes="fixed bug xyz123"; hash2; hash5;
        }
        subgraph {
            rank=same; version="1.2"; notes="ship it"; hash3; hash5;
        }
    }

##### Complicated Firmware Graph

This example shows a more complex upgrade scenario.

There are three firmware image partitions: Boot Loader, Kernel, and
File System.

It's possible to upgrade from version 1.0 to 1.1, from 1.1 to 1.2, or from
1.1 to 1.2.  It is possible to downgrade from 1.2 to 1.1.

Notice that *order* is always specified for every path.

The Boot Loader image is applied first, the Kernel image second, and the
File System image third.

    digraph complicated_example {
        # Image Declarations
        hash1 [notes="original release"];
        hash2 [notes="fixed foo"];
        hash3 [notes="fixed bar"];
        hash4 [notes="Original version"];
        hash5 [notes="fixed bug xyz123"];
        hash6 [notes="first release"];
        hash7 [notes="works better now"];
        hash8 [notes="ship it"];
        subgroup {
            name="Boot Loader"; hash1; hash2; hash3;
        }
        subgroup {
            name="Kernel"; hash4; hash5;
        }
        subgroup {
            name="File System"; hash6; hash7; hash8;
        }
        
        # Upgrade Paths
        hash1 -> hash2 -> hash3 [order=1];
        hash1 -> hash3 [order=1];
        hash3 -> hash2 [downgrade=true, order=1];
        hash4 -> hash5 [order=2];
        hash6 -> hash7 -> hash8 [order=3];
        hash6 -> hash8 [order=3];
        hash8 -> hash7 [downgrade=true, order=3];
        
        # Links
        subgraph {
            rank=same; version="1.0"; hash1; hash4; hash6;
        }
        subgraph {
            rank=same; version="1.1"; hash2; hash5; hash7;
        }
        subgraph {
            rank=same; version="1.2"; notes="ship it"; hash3; hash5; hash8;
        }
    }

Also note that the Links subgraph for version 1.2 overrides the notes
attribute for hash3, hash5, and hash8.

## Specification

|                       |                                                     |
|-----------------------|-----------------------------------------------------|
| Version               | 1                                                   |
| Annotation            | org.alljoyn.Bus.Secure = true                       |

### Properties

#### Version

|            |                                                                |
|------------|----------------------------------------------------------------|
| Type       | uint16                                                         |
| Access     | read-only                                                      |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = const       |

The interface version.


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
update bus object, the bus object will be removed from the system.

Input arguments:

  * **busObjectPath** --- object_path --- The object path to the bus object
    implementing the org.alljoyn.BulkDataTransfer.Get interface

Output arguments:

_None_

Errors raised by this method:

_None_


### Signals

#### UpdateAvailable(firmwareId)

|                       |                                                     |
|-----------------------|-----------------------------------------------------|
| Signal Type           | sessioncast                                         |

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

| Error name                                  | Error message                                    |
|---------------------------------------------|--------------------------------------------------|
| org.alljoyn.BulkDataTransfer.Error.NotFound | firmwareId not known or has no update available. |


## References

 * The XML definition of the [Firmware Selection interface](FirmwareSelection-v1.xml)
 * The Bulk Data Transfer [Theory of operation](/org.alljoyn.BulkDataTransfer/theory-of-operation)
 * The description of the [Information interface](Information-v1)
 * The description of the [Get interface](Get-v1)
 * The DOT graph notation specification [DOT Graph Notation](http://www.graphviz.org/doc/info/lang.html)
