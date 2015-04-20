# org.alljoyn.Bus.Security.ManagedApplication version 1


## Theory of Operation

Once an application is claimed is becomes manageable.  The ManagedApplication
is an interface that provides the mechanism for an admin to manage the
application's security configuration.

High level description and flow diagrams are provided in the Security 2.0 HLD
under sections **Installing a policy**, **Add an application to a security
group**, and **Add a user to a security group**.

## Specification

|                       |                               |
|-----------------------|-------------------------------|
| Version               | 1                             |
| Annotation            | org.alljoyn.Bus.Secure = true |

### Properties

#### Identity

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | array of Certificates                                    |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |


The identify certificate chain.

#### Manifest

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | array of Rules                                           |
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
| Type       | array of CertificateIds                                  |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The list of serial numbers and issuers of the currently installed membership
certificates.

### Methods

#### Reset()

This method allows an admin to reset the application to its original state.
The application's configuration is discarded.  The application is no longer
claimed.

Input arguments: None.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### UpdateIdentity(certificateChain, manifest)

This method allows an admin to update the application's identity certificate
chain and its manifest.

Input arguments:

  * **certificateChain** --- array of Certificates --- the identity certificate
  chain
  * **manifest** --- array of Rules --- the application manifest

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### UpdatePolicy(policy)

This method allows an admin to install the permission policy to the
application.  Any existing policy will be replaced if the new policy version
number is greater than the existing policy's version number.

Input arguments:

  * **policy** --- Policy --- the new policy.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### ResetPolicy()

This method allows an admin to remove the currently installed policy.  The
application reverts back to the default policy.

Input arguments: None.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### InstallMembership(certChain)

This method allows the amdin to install a membership cert chain to the
application.

Input arguments:

  * **certChain** --- array of Certificates --- the membership certificate chain.
It can be a single cert if it is issued by the security group authority.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
  not have permission.

#### RemoveMembership(certificateId)

This method allows an admin to remove a membership certificate chain from the
application.

Input arguments:

  * **certificateId** --- CertificateId --- the serial number and issuer of the
  membership leaf certificate

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
  not have permission.

### Named Types

#### struct EccPublicKey

Refer to Application interface description for the definition of this struct.

#### struct Certificate

Refer to Application interface description for the definition of this struct.

#### struct Policy

This struct represents the permission policy.

  * **specificationVersion** --- uint16 --- specification version number
  * **version** --- uint32 --- policy version number
  * **acls** --- array of struct Acl --- the array of Acls

#### struct Acl

This struct represents the Acl.

  * **peers** --- array of struct Peer --- the array of peers granted in this
  Acl
  * **rules** --- array of struct Rule --- the array of rules

#### struct Peer

This struct represents the peer.

  * **type** --- byte --- the type of peer.  The valid values are:

| Value | Description                                             |
|-------|---------------------------------------------------------|
| 0     | Anonymous: matches all peers.                           |
| 1     | Any: matches all peers trusted by the application.      |
| 2     | Restricted: Matches all peers with certificates issued  |
|       |             by the specified certificate authority.     |
| 3     | Public key: a single peer identified by a public key.   |
| 4     | Security group: all members of specific security group. |

  * **publicKey** --- array of EccPublicKey --- zero or one public key depending
  on the peer type:

| Peer Type      | Key Value                                      |
|----------------|------------------------------------------------|
| Anonymous      | No key required                                |
| Any            | No key required                                |
| Restricted     | The public key of certificate authority        |
| Public key     | The public key of the peer                     |
| Security group | The public key of the security group authority |

  * **groupID** --- byteArray --- the security group ID.  This field is ignored
  for other types.

#### struct Rule

This struct represents the permission rule.

  * **obj** --- string --- the object path
  * **ifn** --- string --- the interface name
  * **mbrs** --- array of Member --- the interface members

#### struct Member

This struct represents the member of the interface.

  * **mbr** --- string --- the member name
  * **type** --- byte --- the message type.  Valid values are:

| Value | Meaning     |
|-------|-------------|
| 0     | Any  type   |
| 1     | Method call |
| 2     | Signal      |
| 3     | Property    |
  * **action** --- byte --- the action bit mask.  A rule with none of the bits set (no rights are granted) is an explicit deny rule.  The valid masks are:

| Mask | Name    | Description                                          |
|------|---------|------------------------------------------------------|
| 0x01 | Provide | sending signal, exposing method calls and properties |
| 0x02 | Observe | receiving signals and reading properties             |
| 0x04 | Modify  | setting properties and making method calls           |

#### struct CertificateId

This struct represents a basic certificate identifier

  * **serialNumber** --- ay --- the certificate serial number
  * **issuer** --- EccPublicKey --- the public key of the issuer

### Interface Errors

The method calls in this interface use the AllJoyn error message handling
feature (`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error
message. The table below lists the possible errors raised by this interface.

| Error name                             | Error message             |
|----------------------------------------|---------------------------|
| org.alljoyn.Bus.ER_PERMISSION_DENIED   | Permission denied         |

## References

  * The XML definition of the [ManagedApplication interface](ManagedApplication-v1.xml)
  * The definition of the [Application interface](Application-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
