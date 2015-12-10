# AllJoyn Multimedia Metadata Schema version 1

## Introduction

This document only defines the AllJoyn Multimedia metadata schema.  This
information is broken out in a separate document due to it size and the fact
that the information presented here is referenced by multiple interfaces.

Even though this document only dictionary entries, the IRB guidelines about
newer versions not breaking backward compatibility still applies.  This means
that the type and meaning of a metadata field, once defined, cannot be changed.
New fields can be added and all new fields added must be considered optional.

Given the nature of metadata for media, most fields are optional.  This is
mostly driven by the fact that metadata will often be missing on media or
otherwise not available.

There are effectively 2 categories of metadata: human informative and machine
informative.  Human informative metadata includes things like the title, artist,
genre, etc.  Machine informative includes things like media size, bit rate, MIME
types, etc.  Some metadata information may be classified as both human
informative and machine informative.  This document will not mandate the
presence of any attribute since many may not be applicable to specific types of
content or the information may simply not be available.  Applications may extend
the set of metadata attributes as necessary for the operation of that
application.

Date metadata fields are to only be represented as strings in one of the
following ISO 8601 formats:

  * Year only: “<year>” where <year> includes all digits of the year.
  * Date: “<year><month><day>” where <year> includes all digits of the year,
    <month> is a 2 digit representation of month (“01” = January), and <day> is
    the day of the month (e.g., “09”).  E.g., 20150315 is March 15, 2015.
  * Date and time: “<year><month><day>T<hour><minute><second>±<offset>”  where
    <year>, <month>, and <day> are the same as the date previously specified,
    and <hour> is the hour (“00” through “23”), <minute> is the minute (“00”
    through “59”), <second> is the second (“00” through “59”), ±<offset> is the
    offset from UTC (“-0800” corresponds to Pacific Standard
    Time). E.g. 20150315T123456-0700 would be 12:34:56 pm Pacific Daylight Time
    on March 15, 2015.
  * Date and time: “<year><month><day>T<hour><minute>±<offset>” is the same as
    the previous date and time specification except without the <second>
    (seconds) portion.
    
These are the only ISO 8601 date and time formats accepted.  This is to keep
parsing and sorting simple.

## Machine Informative Metadata Attributes

The following attributes are mostly useful for application use.  the primary use
of these attributes is for applications to determine if it can render the
referenced media.

  * **MediaUri** --- string --- Universal Resource Identifier for the stream.
    This follows standard URI syntax.  A local file would look like
    “file:///absolute/path/to/media.file”.  A stream sourced from the web would
    look like “http://example.org/path/to/media.file”.  A stream from an RTP
    server would look like “rtp://example.org/path/to/media.file”.

  * **MimeType** --- string --- MIME type specification for the media.

  * **StreamContainer** --- string --- This identifies the container format used
    to encapsulate the content.  This along the VideoPropertiesList and
    AudioPropertiesList provides a somewhat more precise alternative to the
    MimeTypes.  Precisely describing the exact arrangement of codecs and other
    properties for each possible track/stream/content in advanced containers
    such as MPEG-TS and Matroska is very complex and in some cases impossible.
    One difficult to handle case is a broadcast TV station switching from 720p
    to 1080i when transitioning from one program to the next.  Matroska live
    streams are even more difficult in that it allows for a change in the codec
    as well as the dimensional size of the video.

  * **VideoPropertiesList** --- VideoProperties[] --- List of properties
    for each video in the stream.

  * **AudioPropertiesList** --- AudioProperties[] --- List properties for each
    audio track in the stream.

  * **ImagePropertiesList** --- ImageProperties[] --- List of properties for
    each image in the stream or container.  (TIFF files can contain multiple
    images as different layers and each can have their own set of properties
    including dimensions.)

  * **textPropertiesList** ---TextProperties[] -- List of properties for
    stream-able text.  This would primarily be used for video subtitle overlays.

  * **SeekCapabilities** --- byte --- Bit field describing the ability to seek
    within the stream.  A value of 0 (zero) indicates that no seeking is
    supported.  The bit-field values are defined below:

        0x01 -- Seek to Start -- The data stream can support seeking to the
        beginning of data.

        0x02 -- Seek to Position -- The data stream can support seeking to an
        arbitrary absolute position within the data.
        
        0x04 -- Seek Forwards -- The data stream can support seeking to a
        relative position forward of the current position.
        
        0x08 -- Seek Backwards -- The data stream can support seeking to a
        relative position backwards of the current position.

  * **MediaSize** --- uint64 --- Size of the media in octets.  If mediaSize is
    0, then the media is from a streaming source and does not have a finite
    size.  One example of where mediaSize would be set to 0 would be streaming
    broadcast radio from a tuner card.

  * **Drm** --- string --- The presence of this attribute indicates that the
    media uses DRM.  If the value of the string is empty, then the application
    hosting the media is unable to determine which DRM scheme is being used.
    Otherwise, the value of the string will indicate the DRM scheme in use.
    Examples include “FairPlay” by Apple, “PlaysForSure” by Microsoft,
    “DVD-CSS”, “HDCP”, etc.  If the DRM scheme places specific requirements on
    the transfer of that media, it will be incumbent upon the application to
    implement those requirements.

### Named Types


#### struct VideoProperties
Properties for a given video within a stream.

  * **codec** --- string --- Name of the codec used for the video in the stream.

  * **bitrate** --- uint32 --- Bit rate of the video contained in the stream.  0
    if the bit rate information is not provided in the stream.

  * **framerate** -- uint16 -- Number of frames displayed per second.  0 if the
    frame rate is described as variable within the stream.

  * **width** --- uint32 --- Width of the video in pixels.  0 if not
    determinable (i.e. a live stream where the dimensions could change
    mid-stream).

  * **height** --- uint32 --- Height of the video in pixels.  0 if not
    determinable (i.e. a live stream where the dimensions could change
    mid-stream).

  * **aspectRatio** --- double --- The display aspect ratio expressed as
    <value>:1.  Common values are 1.33 (4:3 – standard definition TV), 1.77/1.78
    (16:9 – HDTV), 1.85 (common with movies), 2.35 (common with
    movies), 2.39/2.40 (common with movies and included in Blu-Ray spec).  0
    indicates that the aspect ratio can be computed from the width and height
    and the pixels are perfectly square or that the aspect ratio is not
    determinable (i.e., a live stream where the aspect ratio could change
    mid-stream).


#### struct AudioProperties
Properties for a given audio track within a stream.

  * **codec** --- string --- Name of the codec used for the audio track in the
    stream.

  * **bitrate** --- uint32 --- Bit rate of the audio contained in the stream.  0
    if the bit rate information is not provided in the stream.

  * **sampleRate** --- uint32 --- Number of samples per second.  Common values
    are 44100 for CD audio and 48000, 96000, and 192000 for other audio streams.

  * **channels** --- uint16 --- Number of audio channels in the audio track.
    Common values are 2 for stereo, 6 for 5.1 surround.  Dolby Atmos supports up
    to 64 channels.


#### struct ImageProperties
Properties for a given image.

  * **format** --- string --- Name of the image format.  Common values include
    “JPEG”, “PNG” and “TIFF”.

  * **width** --- uint32 --- Width of the image in pixels.

  * **height** --- uint32 --- Height of the image in pixels.

  * **depth** --- byte --- Number of bits used to express color depth per
    channel.  Typical values include 8 for JPEG images and 16 for HDR (High
    Dynamic Range) images.

  * **channels** --- byte --- Number of image data channels.  Typical values
    include 3 for red/green/blue, 4 for red/green/blue/alpha.  Higher values may
    indicate the inclusion of overlay masks.

  * **alpha** --- byte --- The channel number to use as the alpha mask.

    
#### struct TextProperties
Properties for a given text/subtitle track.

  * **encoding** --- string --- The encoding system for the text.  Typically
    “ASCII”, “UTF-8”, “ISO 1159”, etc.  (While AllJoyn is primarily standardized
    on ASCII/UTF-8, the source media may use other encoding schemes.)

  * **language** --- string --- Language the text is in.


## Human Informative Metadata Attributes

The following attributes are mostly useful for human use.  The primary use of
these attributes is for humans to determine if they want to playback the media.

The Category attribute defined below is mandatory for all media.  It's value
helps Controllers with both classification and with determining which other
metadata fields to expect.

  * **Category** --- byte --- Media category name that indicates the set of
    metadata attributes that will be present.  The following categories are
    defined:
      * 0x00 -- unknown -- This is for media for which a category cannot be
        determined.
      * 0x01 -- music -- This is for music and music videos.
      * 0x02 -- book -- This is for audio books.
      * 0x03 -- movie -- This is for commercially produced movies.
      * 0x04 -- episodic -- This is for episodic content such as TV and radio
        shows as well as many podcasts.
      * 0x05 -- private recording -- This is for personal media clips including
        home movies or short segments from other media sources used for personal
        purposes.  This is also for media that is generated live on the source
        device.  This can include audio from a microphone, video from a camera,
        or even a screen captures.
      * 0x06 -- stream relay -- This is for media streamed from an Internet
        service or a tuner card.
      * 0x07 -- still image -- This is for stand-alone still images (not to be
        confused with images like album art that is embedded within the
        metadata).

### Metadata Schema for Music and Music Videos

This set of attributes are intended to describe media that is a single,
primarily musical composition.  Examples would be a recording of Beethoven’s 5th
Symphony or Michael Jackson’s “Thriller” music video.

  * **album** --- string --- UTF-8 string of the album title.
  * **trackNumber** --- uint16 --- Track number of the title within the album.
  * **totalTrackCount** --- uint16 --- Total number of tracks on the album.
  * **title** --- string --- UTF8-8 string of the music title.
  * **artist** --- string --- UTF-8 string of the artist that performed the
    title.  Typically the same as albumArtist, but not always (i.e.,
    multi-artist hits compilations).
  * **length** --- unit32 --- Length of the track in milliseconds.
  * **date** --- string --- The release year or date.
  * **genres** --- string[] --- List of genres applicable to this music.  Each
    genre string will use UTF-8 encoding.
  * **artwork** --- string --- Media URI for the associated artwork
    image.  (Using a URI allows for handling arbitrarily large images
    in a uniform manner.)
  * **albumArtist** --- string --- UTF-8 string of the album artist.
  * **userRating** --- byte --- “Star” rating on a scale of 0 to 100.
  * **discNumber** --- uint16 --- Disc number in a multi-disc release.
  * **totalDiscs** --- uint16 --- Total number of discs in a multi-disc
    release.
  * **sortAlbum** --- string --- UTF-8 string of album title with words arranged
    for proper sorting (i.e., “The Shining” becomes “Shining, The”).
  * **sortAlbumArtist** --- string ---UTF-8 string of album artist with words
    arranged for proper sorting.
  * **sortTitle** --- string ---UTF-8 string of title with words arranged for
    proper sorting.
  * **sortArtist** --- string ---UTF-8 string of artist with words arranged for
    proper sorting.
  * **live** --- bool --- True indicates the music is a live recording.  False
    indicates that it is a studio recording.
  * **advisory** --- bool --- True indicates music contains possibly
    objectionable material.  False indicates otherwise.
  * **fingerprintID** --- FingerprintInfo --- Music fingerprint IDs.


### Metadata Schema for Movies

This set of attributes are intended to describe media that is primarily
stand-alone movies.  One example would be “It’s a Wonderful Life”.

  * **title** --- string --- Title of the movie or TV show.
  * **length** --- uint32 --- Length in milliseconds.
  * **date** --- string --- Release date.
  * **rating** --- string --- MPAA or other official recommended maturity
    rating.
  * **genres** --- string[] --- List of genres applicable to this music.  Each
    genre string will use UTF-8 encoding.
  * **userRating** --- byte --- “Star” rating on a scale of 0 to 100.
  * **artwork** --- string --- Media URI for the associated artwork
    image.  (Using a URI allows for handling arbitrarily large images
    in a uniform manner.)
  * **shortDescription** --- string --- UTF-8 string containing a brief synopsis
    limited to 256 characters (may be longer than 256 octets if multi-byte
    characters are used).
  * **longDescription** --- string --- UTF-8 string containing the full
    synopsis.
  * **actors** --- string[] --- List of actors appearing in the movie.  Strings
    are encoded in UTF-8.
  * **directors** --- string[] --- List of directors.  Strings are encoded in
    UTF-8.
  * **producers** --- string[] --- List of producer.  Strings are encoded in
    UTF-8.
  * **screenwriters** --- string[] --- List of screenwriters.  Strings are
    encoded in UTF-8.
  * **studio** --- string --- UTF-8 name of the studio that made the movie.
  * **sortTitle** --- string --- UTF-8 string of title with words arranged for
    proper sorting.


### Metadata Schema for Episodic Content

This set of attributes are intended to describe media that is primarily one part
of a long running series of content.  This will typically be episodes of TV
show, radio shows, and podcasts.  One example would be the TV show, “The
Simpsons”.

  * **title** --- string --- Title of the series.
  * **episodeTitle** --- string --- Title of the episode.
  * **length** --- uint32 --- length in milliseconds.
  * **season** --- uint16 --- Season number.  (This should be 0 for content that
    is not organized by seasons, e.g., podcasts).
  * **episode** --- uint16 --- Episode number.
  * **date** --- string --- Original air/broadcast date.
  * **rating** --- string --- MPAA or other official recommended maturity
    rating.
  * **genres** --- string[] --- List of genres applicable to this music.  Each
    genre string will use UTF-8 encoding.
  * **userRating** --- byte --- “Star” rating on a scale of 0 to 100.
  * **artwork** --- struct (mimeType, imageData) --- Artwork representing the
    episodic content.
  * **mimeType** --- string --- MIME type for the image data.  Should be for
    JPEG or PNG.
  * **imageData** --- array of bytes --- Image data in the format specified by
    mimeType.
  * **totalEpisodes** --- uint16 --- Total number of episodes in the season.
  * **totalSeasons** --- uint16 --- Total number of seasons.
  * **shortDescription** --- string --- UTF-8 string containing a brief synopsis
    limited to 256 characters (may be longer than 256 octets if multi-byte
    characters are used).
  * **longDescription** --- string --- UTF-8 string containing the full
    synopsis.
  * **actors** --- string[] --- List of actors appearing in the movie.  Strings
    are encoded in UTF-8.
  * **directors** --- string[] --- List of directors.  Strings are encoded in
    UTF-8.
  * **producers** --- string[] --- List of producer.  Strings are encoded in
    UTF-8.
  * **screenwriters** --- string[] --- List of screenwriters.  Strings are
    encoded in UTF-8.
  * **studio** --- string --- UTF-8 name of the studio that made the movie.
  * **sortTitle** --- string ---UTF-8 string of title with words arranged for
    proper sorting.
  * **sortEpisodeTitle** --- string ---UTF-8 string of episode title with words
    arranged for proper sorting.
  * **episodeID** --- uint32 --- asdfasdf
  * **network** --- string --- UTF-8 string of the network that originally
    broadcast the episode.


### Metadata Schema for Audiobooks

This set of attributes are intended to describe media that are primarily audio
books.

  * **title** --- string --- UTF-8 string of the book title.
  * **trackNumber** --- uint16 --- This roughly correlates to chapter numbers
    with the exception that prologs and epilogs are included in the numbering
    such that they are ordered appropriately with respect to the actual
    chapters.
  * **totalTrackCount** --- uint16 --- Total number of chapters, prologs, and
    epilogs.
  * **chapterTitle** --- string --- UTF8-8 string of the chapter title.
  * **authors** --- string[] --- List of authors that wrote the book.  Strings
    are encoded in UTF-8.
  * **length** --- unit32 --- Length of the track in milliseconds.
  * **date** --- string --- The year or date the book was published.
  * **genres** --- string[] --- List of genres applicable to this music.  Each
    genre string will use UTF-8 encoding.
  * **userRating** --- byte --- “Star” rating on a scale of 0 to 100.
  * **artwork** --- struct (mimeType, imageData) --- Book cover art.
  * **mimeType** --- string --- MIME type for the image data.  Should be for
    JPEG or PNG.
  * **imageData** --- array of bytes --- Image data in the format specified by
    mimeType.
  * **reader** --- string --- UTF-8 string of the person who performed the
    reading.
  * **publisher** --- string --- UTF-8 string of the publisher of the book.

### Metadata Schema for Stream Relay

This set of attributes describe the contents of data that is relayed from other
streaming sources such as from hardware tuners or Internet streaming services.
For relayed Internet streams the sourceURI attribute will contain the URL of the
Internet stream.

  * **streamCategory** --- uint8 --- This is similar to the “category” field
    except that it describes the category of media being streamed so that
    Controllers will know which set of metadata attributes to expect.  This may
    be any one of the defined categories excluding “internet” and “tuner” in
    order to describe which set of attributes may also be present.  Each
    category indicates the set of metadata attributes that will be present.
  * **channelID** --- string --- Tuner channel identification.
  * **rds** --- string --- Radio Data Service (may be available with FM radio
    tuners).  This can change over time.
  * **name** --- string --- Name of the stream source.

### Metadata Schema for Private Recording

This set of attributes describes video and/or audio media that was created for
personal use.  It is also describes media streams that are generated live
through any number of means including the use of microphones, cameras, screen
captures, etc.

  * **date** --- string --- Date (and time) the recording was made.
  * **title** --- string --- Title of the recording.
  * **gpsLocation** --- string --- Text representation of GPS coordinates of
    where the recording was made.  This is optional since not all recording
    devices have GPS receivers on them.
  * **location** --- string --- Name of where the recording was made.
  * **tags** --- string[] --- Set of tags provided by the user to aid in
    searching.
  * **userRating** --- byte --- “Star” rating of the media on a scale of 0
    to 100.

### Metadata Schema for Still Images

This set of attributes describe still images.  Any and all EXIF tags may be
included in the set of attributes so long as the tag names are prefixed with
“exif-“.  As an example, the EXIF tag “ISOSpeedRating” would become
“exif-ISOSpeedRating”.  Any and all IPTC tags may be included as well so long as
the tag names are prefixed with “iptc-“.  Where possible, contents of EXIF and
IPTC tags that correspond to the following attributes should be copied into
these attributes to aid in generic searches.

  * **date** --- string --- Date (and time) the still image was captured.
  * **title** --- string --- Title of the image.
  * **gpsLocation** --- string --- Text representation of GPS coordinates of
    where the image was captured.  This is optional since not all recording
    devices have GPS receivers on them.
  * **location** --- string --- Name of where the image was captured.
  * **tags** --- string[] --- Set of tags provided by the user to aid in
    searching.
  * **artist** --- string --- Name of the photographer or creator of the image.

The following table maps EXIF types used for EXIF tags into AllJoyn types that
will be used for the metadata encoding.

| EXIF type  | AllJoyn Metadata type   |
|------------|-------------------------|
| BYTE       | byte                    |
| ASCII      | string                  |
| SHORT      | uint16                  |
| LONG       | uint32                  |
| RATIONAL   | struct (uint32, uint32) |
| UNDEFINED  | byte                    |
| SLONG      | int32                   |
| SRATIONAL  | struct (int32, int32)   |

Many IPTC fields contain textual strings.  For those fields, the metadata should
use the string type.  For those IPTC fields that contain other, non-textual
encoding, an array of bytes should be used.


### Named Types

#### dictionary FingerprintInfo

Media fingerprint information.

  * **key** --- string --- UTF-8 string identifying the service, authority, or
    algorithm used to provide the fingerprint ID value.
  * **value** --- byte[] --- The actual fingerprint ID value.  This can be used
    for identifying true duplicates.  The meaning of the byte array
    will depend on the fingerprinting mechanism in use.


## References

  * The description of the [SourceDiscovery interface](SourceDiscovery-v1)
  * The Multimedia [Theory of Operation](theory-of-operation)
