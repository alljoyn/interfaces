# org.alljoyn.Bus.Security.Application version 1


## Theory of Operation

The Application interface is provided by all applications and provides the
basic interface of applications regardless of the state they are in.

## Specification

|                       |                               |
|-----------------------|-------------------------------|
| Version               | 1                             |
| Annotation            | org.alljoyn.Bus.Secure = true |

### Properties

#### ClaimState

|        |           |
|--------|-----------|
| Type   | uint16    |
| Access | read-only |

The claim state of the application.  An enumeration representing the current
state of the application.  The list of valid values:

| Value | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0     | Not claimable.  The application is not claimed and not accepting  |
|       | claim request.                                                    |
| 1     | Claimable.  The application is not claimed and is accepting claim |
|       | request.                                                          |
| 2     | Claimed. The application is claimed and can be configured.        |
| 3     | NeedUpdate. The application is claimed and requires a             |
|       | configuration update (after a software upgrade)                   |

#### ManifestTemplateDigest

|        |             |
|--------|-------------|
| Type   | byte array  |
| Access | read-only   |

The SHA256 digest of the manifest template.

#### ClaimCapabilities

|        |           |
|--------|-----------|
| Type   | uint32    |
| Access | read-only |

The claim schemes supported by the application represented as a bit mask.
The list of valid masks:

| Mask  | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0x1   | claiming via ECDHE_NULL                                           |
| 0x2   | claiming via ECDHE_PSK with security manager generated passphrase |
| 0x4   | claiming via ECDHE_PSK with application generated passphrase      |
| 0x08  | claiming via ECDHE_ECDSA                                          |

### Methods

#### GetPublicKey() -> (publicKey)

This method retrieves the application public key.

Input arguments: None.

Output arguments:

  * **publicKey** --- PublicKey --- the application public key

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### GetManufacturerCertificate() -> (certificateChain)

This method retrieves the application manufacturer certificate.  For release
15.08 it will return an empty cert chain.

Input arguments: None.

Output arguments:

  * **publicKey** --- PublicKey --- the application public key
  * **certificateChain** --- CertificateChain --- the certificate chain
  installed by the manufacturer

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### GetManifestTemplate() -> (manifestTemplate)

This method retrieves the manifest template defined by the application
developer.

Input arguments: None.

Output arguments:

  * **manifestTemplate** --- array of Rules --- the application manifest
  template

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

### Named Types

#### struct PublicKey

This struct represents the ECC public key.

  * **algorithm** --- byte --- the algorithm.  The current valid value is
0 – ECDSA with SHA256
  * **curve** --- byte --- The ECC curve id.  The current valid value is
0 – NIST P-256
  * **xCoordinate** --- byte array --- The x coordinates of the point
  * **yCoordinate** --- byte array --- The y coordinates of the point

#### struct Certificate

This struct represents a certificate
  * **encoding** --- byte --- the data encoding schemed.  Current values are:
0 -- X.509 DER; 1 -- X.509 DER PEM
  * **certData** --- byte array --- The certificate data depending on the
encoding

#### CertificateChain

Certificate chain is an array of certificates.

#### struct Rule

Refer to ManagedApplication interface description for the definition of this struct.

### Interface Errors

The method calls in this interface use the AllJoyn error message handling
feature (`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error
message. The table below lists the possible errors raised by this interface.

| Error name                             | Error message             |
|----------------------------------------|---------------------------|
| org.alljoyn.Bus.ER_PERMISSION_DENIED   | Permission denied         |

## References

  * The XML definition of the [Application interface](ManagedApplication-v1.xml)
  * The definition of the [ManagedApplication interface](ManagedApplication-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
