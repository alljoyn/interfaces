# org.alljoyn.Bus.Security.ManagedApplication version 1


## Theory of Operation

Once an application is claimed it becomes a Security 2.0 manageable
application.  The ManagedApplication is an interface that provides the
mechanism for an admin to manage the application's security configuration.

A high level description and flow diagrams are provided in the Security 2.0 HLD
under sections **Installing a policy**, **Add an application to a security
group**, and **Add a user to a security group**.

## Specification

|            |                                                          |
|------------|----------------------------------------------------------|
| Version    | 1                                                        |
| Annotation | org.alljoyn.Bus.Secure = true                            |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The interface version number.

#### Identity

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Certificate[]                                            |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |


The identify certificate chain.

#### Manifest

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Rule[]                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The manifest.

#### IdentityCertificateId

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | CertificateId                                            |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The serial number and issuer of the currently installed identity certificate.

#### PolicyVersion

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint32                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The version number of the currently installed policy.

#### Policy

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Policy                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The currently installed policy.

#### DefaultPolicy

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | Policy                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The default policy regardless of the currently installed policy.

#### MembershipSummaries

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | CertificateId[]                                          |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The list of serial numbers and issuers of the currently installed membership
certificates.

### Methods

#### Reset()

This method allows an admin to reset the application to its original state
prior to claim.  The application's security 2.0 related configuration is
discarded.  The application is no longer claimed.

If the keystore is cleared by the BusAttachment::ClearKeyStore() call, this
Reset() method call is not required.  The Configuration service's
FactoryReset() call in fact clears the keystore, so this Reset() call is not
required in that scenario.

Input arguments: None.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the caller does
    not have permission.

#### UpdateIdentity(certificateChain, manifest)

This method allows an admin to update the application's identity certificate
chain and its manifest.

Input arguments:

  * **certificateChain** --- Certificate[] --- the identity certificate
  chain
  * **manifest** --- Rule[] --- the application manifest

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the caller does
  not have permission.
  * org.alljoyn.Bus.Error.InvalidCertificate --- Error raised when the identity
  certificate chain is not valid
  * org.alljoyn.Bus.Error.InvalidCertificateUsage --- Error raised when the
  Extended Key Usage is not AllJoyn specific
  * org.alljoyn.Bus.Error.DigestMismatch --- Error raised when the digest of the
  not have permission.

#### UpdatePolicy(policy)

This method allows an admin to install the permission policy to the
application.  Any existing policy will be replaced if the new policy version
number is greater than the existing policy's version number.

Input arguments:

  * **policy** --- Policy --- the new policy.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the caller does
  not have permission.
  * org.alljoyn.Bus.Error.PolicyNotNewer --- Error raised when the new policy
  does not have a greater version number than the existing policy.

#### ResetPolicy()

This method allows an admin to remove the currently installed policy.  The
application reverts back to the default policy generated during the claiming
process.

Input arguments: None.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the caller does
not have permission or the application is unclaimed or not claimable.

#### InstallMembership(certChain)

This method allows the amdin to install a membership cert chain to the
application.

Input arguments:

  * **certChain** --- Certificate[] --- the membership certificate chain.
It can be a single cert if it is issued by the security group authority.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the caller does
not have permission.
  * org.alljoyn.Bus.Error.DuplicateCertificate --- Error raised when the
  membership certificate is already installed.
  * org.alljoyn.Bus.Error.InvalidCertificate --- Error raised when the
  membership certificate is not valid.

#### RemoveMembership(certificateId)

This method allows an admin to remove a membership certificate chain from the
application.

Input arguments:

  * **certificateId** --- CertificateId --- the serial number and issuer of the
  membership leaf certificate

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.Error.PermissionDenied --- Error raised when the caller does
not have permission.
  * org.alljoyn.Bus.Error.CertificateNotFound --- Error raised when the
  certificate is not found

### Named Types

#### struct EccPublicKey

Refer to Application interface description for the definition of this struct.

#### struct Certificate

Refer to Application interface description for the definition of this struct.

#### struct Policy

This struct represents the permission policy.

  * **specificationVersion** --- uint16 --- specification version number.  The
  current value is 1
  * **version** --- uint32 --- policy version number.  The default policy has
  version 0
  * **acls** --- Acl[] --- the array of Acls

#### struct Acl

This struct represents the ACL.

  * **peers** --- Peer[] --- the array of peers granted in this
  ACL
  * **rules** --- Rule[] --- the array of rules

#### struct Peer

This struct represents the peer.

  * **type** --- byte --- the type of peer.  The valid values are:

| Value | Description                                                     |
|-------|-----------------------------------------------------------------|
| 0     | ALL: matches all peers including anonymous peers.               |
| 1     | ANY_TRUSTED: matches any peer trusted by the application.       |
| 2     | FROM_CERTIFICATE_AUTHORITY: matches all peers with certificates |
|       |     issued by the specified certificate authority.              |
| 3     | WITH_PUBLIC_KEY: a single peer identified by a public key.      |
| 4     | WITH_MEMBERSHIP: all members of specific security group.        |

  * **publicKey** --- EccPublicKey[] --- zero or one public key depending
  on the peer type:

| Peer Type                  | Key Value                                      |
|----------------------------|------------------------------------------------|
| ALL                        | No key required                                |
| ANY_TRUSTED                | No key required                                |
| FROM_CERTIFICATE_AUTHORITY | The public key of certificate authority        |
| WITH_PUBLIC_KEY            | The public key of the peer                     |
| WITH_MEMBERSHIP            | The public key of the security group authority |

  * **groupID** --- byte[16]--- the security group ID.  This field is ignored
  for other types.

#### struct Rule

This struct represents the permission rule.

  * **obj** --- string --- the object path
  * **ifn** --- string --- the interface name
  * **mbrs** --- Member[] --- the interface members

#### struct Member

This struct represents the member of the interface.

  * **mbr** --- string --- the member name
  * **type** --- byte --- the message type.  Valid values are:

| Value | Meaning     |
|-------|-------------|
| 0     | Any type    |
| 1     | Method call |
| 2     | Signal      |
| 3     | Property    |
  * **action** --- byte --- the action bit mask.  The valid masks are:

| Mask | Name    | Description                                          |
|------|---------|------------------------------------------------------|
| 0x01 | Provide | sending signal, exposing method calls and properties |
| 0x02 | Observe | receiving signals and reading properties             |
| 0x04 | Modify  | setting properties and making method calls           |

#### struct CertificateId

This struct represents a basic certificate identifier

  * **serialNumber** --- ay --- the certificate serial number
  * **issuer** --- EccPublicKey --- the public key of the issuer

## References

  * The XML definition of the [ManagedApplication interface](ManagedApplication-v1.xml)
  * The definition of the [Application interface](Application-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
