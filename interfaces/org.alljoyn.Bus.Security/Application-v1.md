# org.alljoyn.Bus.Security.Application version 1


## Theory of Operation

The Application interface is provided by all security v2 applications and
provides the basic interface of security v2 applications regardless of the state
they are in.

## Specification

|                       |                               |
|-----------------------|-------------------------------|
| Version               | 1                             |
| Annotation            | org.alljoyn.Bus.Secure = true |

### Properties

#### ApplicationState

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

An enumeration representing the current state of the application.  The list of
valid values:

| Value | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0     | Not claimable.  The application is not claimed and not accepting  |
|       | claim requests.                                                   |
| 1     | Claimable.  The application is not claimed and is accepting claim |
|       | requests.                                                         |
| 2     | Claimed. The application is claimed and can be configured.        |
| 3     | NeedUpdate. The application is claimed, but requires a            |
|       | configuration update (after a software upgrade).                  |

#### ManifestTemplateDigest

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Digest                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The SHA256 digest of the manifest template.

#### PublicKey

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | EccPublicKey                                             |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |


The public key used by the application to identify itself.

#### ManufacturerCertificate

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | array of Certificates                                    |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The manufacturer certificate chain.  This chain is installed by the manufacturer
at production time.  If no manufacturer certificate is available then an empty
array will be returned.

#### ManifestTemplate

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | array of Rules                                           |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The manifest template defined by the application developer.  The manifest
contains the permission rules the application is required to operate.

#### ClaimCapabilities

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint32                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The claim schemes supported by the application represented as a bit mask.
The list of valid masks:

| Mask  | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0x1   | claiming via ECDHE_NULL                                           |
| 0x2   | claiming via ECDHE_PSK with security manager generated passphrase |
| 0x4   | claiming via ECDHE_PSK with application generated passphrase      |
| 0x08  | claiming via ECDHE_ECDSA                                          |

### Named Types

#### struct EccPublicKey

This struct represents the Elliptic Curve Cryptography public key.

  * **algorithm** --- byte --- the algorithm.  The current valid value is
0 – ECDSA with SHA256
  * **curveIdentifier** --- byte --- The ECC curve id.  The current valid value is
0 – NIST P-256
  * **xCoordinate** --- byte array --- The x coordinates of the point
  * **yCoordinate** --- byte array --- The y coordinates of the point

#### struct Certificate

This struct represents a certificate
  * **encoding** --- byte --- the data encoding schemed.  Current values are:
0 -- X.509 DER; 1 -- X.509 DER PEM
  * **certificateData** --- byte array --- The certificate data depending on the
encoding

#### struct Digest

This struct represents a digest
  * **algorithm** --- byte --- the digest algorithm.  Current values are:
0 -- SHA-256
  * **digestData** --- byte array --- The digest data

#### struct Rule

Refer to ManagedApplication interface description for the definition of this struct.

### Interface Errors

This interface uses the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The
table below lists the possible errors raised by this interface.

| Error name                             | Error message             |
|----------------------------------------|---------------------------|
| org.alljoyn.Bus.ER_PERMISSION_DENIED   | Permission denied         |

## References

  * The XML definition of the [Application interface](Application-v1.xml)
  * The definition of the [ManagedApplication interface](ManagedApplication-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
