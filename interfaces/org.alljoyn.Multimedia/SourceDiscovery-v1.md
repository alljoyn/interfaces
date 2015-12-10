# org.alljoyn.Multimedia.SourceDiscovery version 1

## Theory of Operation

(( QUESTION: Is it reasonable for a bus object to present data differently to
different clients on a per session basis?  Particularly with respect to
properties?  The current design of this interface assumes the answer to that
question is "Yes".  If it is determined that the answer is "No", then this
interface will need to be modified to include some sort of token to identify
which set of filters are active.  Also, the properties would need to replaced
with method calls that use that token. ))

This interface defines a mechanism by which a media controller can query,
browse, and search for media that can be streamed to a sink.

It is expected that this interface will be used to access media repositories
that are too large to represent as AllJoyn bus objects or even as entries in a
single list.

## Specification

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

#### SortingSpec

Controls the ordering of media metadata in the query operations.  If this is
unset, the implementor is free to sort results in any order they see fit with
the caveat that the sorting order does not change until the next write operation
to this property.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | SortSpec[]                                                            |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |


#### SortableItems

This lists which metadata attributes may be specified in the SortingSpec property.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | string[]                                                              |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |

((_Note: Change EmitsChangedSignal to const when AllJoyn supports const_))


#### SearchFilter

Controls which media metadata will be provided in query and browse operations
according to search terms applied to all metadata fields.  This property is
simply an array of strings where only media with metadata that matches all
search terms will be accessible via the query and browse operations.  An empty
list matches all media metadata.  The matching set is ultimately **anded** with
the result from the MimeTypeFilter and MetadataFilter.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | string[]                                                              |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |

#### MimeTypeFilter

Controls which media metadata will be provided in query and browse operations
according to the media's MIME types.  This property is simply an array of
strings where only media with metadata that matches any of the specified MIME
types will be accessible via the query and browse operations.  An empty list
matches all media metadata.  The matching set is ultimately **anded** with
the result from the SearchFilter and MetadataFilter.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | string[]                                                              |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |


#### MetadataFilter

Controls which media metadata will be provided in query and browse operations
according to specific metadata values.  This can be used to browse media in a
hierarchical manner.  For example, the artist metadata field could be set to a
specific artist to enable searching for albums by that artist.  Then both the
artist and album metadata fields could be set to enable browsing specific tracks
on a given album.  Only those media with metadata that matches all specified
values will be accessible via the query and browse operations.  An empty list
matches all media metadata.  The matching set is ultimately **anded** with the
result from the SearchFilter and MetadataFilter.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | MetadataDictionary                                                    |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = false              |

#### MatchCount

This indicates how many media entries can be browsed or queried given the
filtering criteria set in the other properties.

|                       |                                                                       |
|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint32                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.Property.EmitsChangedSignal = true               |

(( QUESTION: Should the type be uint64 to account for media repositories like
YouTube? ))


### Methods

#### GetAttributeValues(attribute, index, size) -> (attributeValues)

This returns the set of possible values for a given attribute after the
filtering properties have been applied.  For example, setting the MetadataFilter
such that "artist"="Michael Jackson" would cause this function to return "Bad"
and "Thriller" when attribute is set to "album".  The implementor may return
fewer values than the requested size.

Input arguments:

  * **attribute** --- string --- Name of the metadata attribute to return values for.
  * **index** --- uint32 --- First entry to retrieve.
  * **size** --- uint32 --- Maximum number of entries to retrieve.

Output arguments:

  * **attributeValues** --- variant[] --- This lists all the values for the
    specified attribute filtered by the filtering properties and ordered
    according to the SortingSpec.  The type of the variant data is dictated by
    the value type associated with the metadata attribute (see the Multimedia
    Metadata document for details).

Errors raised by this method:

  * **org.alljoyn.Multimedia.Error.OutOfRange** --- The value of index to larger
    than the MatchCount property.


#### GetMetadataList(index, size, attributes) -> (metadataList)

This returns the desired metadata for the specified range of media.  The
returned list is subject to the filtering properties and ordered according to
the SortingSpec property.

Input arguments:

  * **index** --- uint32 --- Index of the first entry to return.
  * **size** --- uint32 --- Maximum number of entries to return.
  * **attributes** --- string[] --- List of which attributes to include in the
    resulting metadata list.

Output arguments:

  * **metadataList** --- MetadataDictionary[] --- The list of metadata entries
    for media matching the filtering criteria.  Only the specified attributes
    along with the mediaUri attribute will be included.  The mediaUri attribute
    must always be included regardless of the contents of the attributes
    parameter.

Errors raised by this method:

  * **org.alljoyn.Multimedia.Error.OutOfRange** --- The value of index to larger
    than the MatchCount property.


#### GetMetadata(mediaUri, attributes) -> (metadata)

This returns the metadata for a specific media entry.  It is not affected by
filtering or the SortingSpec properties.

Input arguments:

  * **mediaUri** --- string --- Reference to the media to get the metadata for.
  * **attributes** --- string[] --- Metadata attributes to include in the
    returned data.

Output arguments:

  * **metadata** --- MetadataDictionary --- Metadata for the requested media.
    This only includes values for the requested attributes.

Errors raised by this method:

  * **org.alljoyn.BulkDataTransfer.Error.NotFound** --- The mediaUri refers to
    unknown media.


### Signals

#### MetadataChanged(mediaUri)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This is sent to indicate that the metadata for a given media entry was updated
with new values.

Output arguments:

  * **mediaUri** --- string --- Reference to the media that was updated.


#### MediaDatabaseChanged(added, removed)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

This is sent to indicate that media was added to and/or removed from the
underlying media database.  Setting a value for the SortingSpec will cause the
updates to take effect for the querying operations.

Output arguments:

  * **added** --- uint32 --- Number of entries added.
  * **removed** --- uint32 --- Number of entries removed.


### Data Structures

#### struct SortSpec

  * **sortField** --- string --- Metadata attribute to sort on.  Must be listed
    in the SortableItems property.
  * **direction** --- boolean --- Set to 'true' for ascending and 'false' for
    descending.


#### dictionary MetadataDictionary

  * **key** --- string --- Metadata attribute name to match against.
  * **value** --- variant --- The matching criteria.  The type must match that
    specified for the given attribute.


### Interface Errors

| Error name                                  | Error message       |
|---------------------------------------------|---------------------|
| org.alljoyn.Multimedia.Error.OutOfRange     | Value out of range. |
| org.alljoyn.BulkDataTransfer.Error.NotFound | mediaUri not known. |


## References

  * The XML definition of the [SourceDiscovery interface](SourceDiscovery-v1.xml)
  * The Multimedia [Theory of operation](theory-of-operation)
  * The description of the [SourceSelection interface](SourceSelection-v1)
  * The description of the [Multimedia Metadata Schema](Metadata-v1)

