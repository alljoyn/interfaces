# org.alljoyn.Bus.Security.ClaimableApplication version 1


## Theory of Operation

ClaimableApplication is an interface that provides the action to claim an
application in order to install permission enforcement policy. This interface
is provided by security 2.0 applications that are in the claimable state.  A
high level description and flow diagrams are provided in the Security 2.0 HLD.

## Specification

|                       |                                     |
|-----------------------|-------------------------------------|
| Version               | 1                                   |
| Annotation            | org.alljoyn.Bus.Secure = true       |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The interface version number.

### Methods

#### Claim(certificateAuthority, authorityKeyIdentifier, adminSecurityGroupId, adminSecurityGroupAuthority, adminGroupAuthorityKeyIdentifier, identityCertificateChain, manifest)

This method allows an admin to claim the application to enable the permission
enforcement. Members of the admin security group are allowed
to manage the application. High level description and call flows are listed
under section **Claim a factory-reset application** in the Security 2.0 HLD.

Input arguments:

  * **certificateAuthority** --- EccPublicKey --- the public key of the
certificate authority. The app will trust any peer's identity certificate
that chains up to this certificate authority.
  * **authorityKeyIdentifier** --- byte[] --- the key identifier of the
certificate authority public key.
  * **adminSecurityGroupId** --- byte[] --- the admin security group id.
  * **adminSecurityGroupAuthority** --- EccPublicKey --- the admin security group id public key.
  * **adminGroupAuthorityKeyIdentifier** --- byte[] --- the key identifier of the admin group authority
  * **identityCertificateChain** --- Certificate[] --- the identity certificate
  chain for the claimed app.  It can be a single cert if it is issued by the
  certificate authority.
  * **manifest** --- Rule[] --- the manifest.

Output arguments: None

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the
  application is not claimable
  * org.alljoyn.Bus.Error.InvalidCertificate --- Error raised when the identity
  certificate chain is not valid
  * org.alljoyn.Bus.Error.InvalidCertificateUsage --- Error raised when the
  Extended Key Usage is not AllJoyn specific
  * org.alljoyn.Bus.Error.DigestMismatch --- Error raised when the digest of
  the manifest does not match the digested listed in the identity certificate


### Named Types

#### struct EccPublicKey

Refer to Application interface description for the definition of this struct.

#### struct Certificate

Refer to Application interface description for the definition of this struct.

#### struct Rule

Refer to ManagedApplication interface description for the definition of this struct.

## References

  * The XML definition of the [ClaimableApplication interface](ClaimableApplication-v1.xml)
  * The definition of the [Application interface](Application-v1.md)
  * The definition of the [ManagedApplication interface](ManagedApplication-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
