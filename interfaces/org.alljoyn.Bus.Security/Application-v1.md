# org.alljoyn.Bus.Security.Application version 1


## Theory of Operation

The Application interface is provided by all security v2 applications and
provides the basic interface of security v2 applications regardless of the state
they are in.

## Specification

|              |                                                          |
|--------------|----------------------------------------------------------|
| Version      | 1                                                        |
| Annotation   | org.alljoyn.Bus.Secure = true                            |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The interface version number.

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
| 0     | NotClaimable.  The application is not claimed and not accepting   |
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

The SHA-256 digest of the manifest template.

#### EccPublicKey

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | EccPublicKey                                             |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |


The Elliptic Curve Cryptography public key used by the application's keystore
to identify itself. The public key persists across any
ManagedApplication.Reset() call.  However, if the keystore is cleared via the
BusAttachment::ClearKeyStore() or using Config.FactoryReset(), the public key
will be regenerated.

#### ManufacturerCertificate

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Certificate[]                                            |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The manufacturer certificate chain. The leaf cert is listed first.  The signing
certs follow.  This chain is installed by the manufacturer at production time.
If no manufacturer certificate is available then this is an empty array.

#### ManifestTemplate

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Rule[]                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The manifest template defined by the application developer.  The manifest
contains the permission rules the application requires to operate.

#### ClaimCapabilities

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The authentication mechanisms the application supports for the claim process.
It is a bit mask.

| Mask  | Description                                                   |
|-------|---------------------------------------------------------------|
| 0x1   | claiming via ECDHE_NULL                                       |
| 0x2   | claiming via ECDHE_PSK                                        |
| 0x4   | claiming via ECDHE_ECDSA                                      |

#### ClaimCapabilityAdditionalInfo

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The additional information information on the claim capabilities.
It is a bit mask.

| Mask  | Description                                                   |
|-------|---------------------------------------------------------------|
| 0x1   | PSK generated by Security Manager                             |
| 0x2   | PSK generated by application                                  |

### Named Types

#### struct EccPublicKey

This struct represents the Elliptic Curve Cryptography public key.

  * **algorithm** --- byte --- the algorithm.  The current valid value is
0 – ECDSA with SHA-256
  * **curveIdentifier** --- byte --- The ECC curve id.  The current valid value is
0 – NIST P-256
  * **xCoordinate** --- byte[] --- The x coordinate of the point
  * **yCoordinate** --- byte[] --- The y coordinate of the point

#### struct Certificate

This struct represents a certificate
  * **encoding** --- byte --- the data encoding scheme.  Current values are:
0 -- X.509 DER; 1 -- X.509 DER PEM
  * **certificateData** --- byte[] --- The certificate data depending on the
encoding

#### struct Digest

This struct represents a digest
  * **algorithm** --- byte --- the digest algorithm.  Current values are:
0 -- SHA-256
  * **digestData** --- byte[] --- The digest data

#### struct Rule

Refer to ManagedApplication interface description for the definition of this struct.

### Interface Errors

This interface uses the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The
table below lists the possible errors raised by this interface.

| Error name                                      | Error message     |
|-------------------------------------------------|-------------------|
| org.alljoyn.Bus.Security.Error.PermissionDenied | Permission denied |

## References

  * The XML definition of the [Application interface](Application-v1.xml)
  * The definition of the [ManagedApplication interface](ManagedApplication-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
