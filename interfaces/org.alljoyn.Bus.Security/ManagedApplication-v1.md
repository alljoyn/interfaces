# org.alljoyn.Bus.Security.ManagedApplication version 1


## Theory of Operation

Once an application is claimed is becomes manageable.  The ManagedApplication
is an interface that provides the mechanism for an admin to manage
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

#### PolicyVersion

|        |           |
|--------|-----------|
| Type   | uint32    |
| Access | read-only |

The version number of the currently installed policy.

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

  * **certificateChain** --- CertificateChain --- the identity certificate
  chain
  * **manifest** --- array of Rules --- the application manifest

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### GetIdentity() -> (certificateChain)

This method retrieves the identity certificate chain from the application.

Input arguments: None.

Output arguments:

  * **certificateChain** --- CertificateChain --- the identity certificate chain

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### GetManifest() -> (manifest)

This method retrieves the application manifest

Input arguments: None.

Output arguments:

  * **manifest** --- array of Rules --- the application manifest

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### GetIdentityCertificateSerialNumber() -> (serialNum, issuer)

This convenient method provides a quick check on the installed identity
certificate without retrieving the whole identity certificate chain.  It
returns the information on the leaf cert only.

Input arguments: None.

Output arguments:

  * **serialNumber** --- byte array --- the cert's serial number
  * **issuer** --- PublicKey --- the cert's issuer public key

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

#### GetPolicy() -> (policy)

This method retrieves the currently installed permission policy from the
application.

Input arguments: None.

Output arguments:

  * **policy** --- Policy --- the permission policy.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
not have permission.

#### GetDefaultPolicy() -> (policy)

This method retrieves the default policy that is generated during the claim
process.  This default policy is returned regardless of the currently
installed policy.

Input arguments: None.

Output arguments:

  * **policy** --- Policy --- the default permission policy.

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

  * **certChain** --- CertificateChain --- the membership certificate chain.
It can be a single cert if it is issued by the security group authority.

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
  not have permission.

#### RemoveMembership(serialNum, issuer)

This method allows an admin to remove a membership certificate chain from the
application.

Input arguments:

  * **serialNum** --- byte array --- the serial number of the membership leaf
certificate
  * **issuer** --- PublicKey --- the issuer's public key

Output arguments: None.

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
  not have permission.

#### GetMembershipSummaries() -> (certificateSummaries)

A utility method to retrieve the list of serial numbers and issuers of the
currently install membership certificates.

Input arguments: None.

Output arguments:

  * **certificateSummaries** --- array of (byte array, PublicKey) pairs ---
  The list of serial numbers and issuer public keys template

Errors raised by this method:

  * org.alljoyn.Bus.ER_PERMISSION_DENIED --- Error raised when the caller does
  not have permission.


### Named Types

#### struct PublicKey

Refer to Application interface description for the definition of this struct.

#### struct Certificate

Refer to Application interface description for the definition of this struct.

#### CertificateChain

Certificate chain is an array of certificates.

#### struct Policy

This struct represents the permission policy.

  * **specificationVersion** --- uint16 --- specification version number
  * **version** --- uint32 --- policy version number
  * **ACLs** --- array of struct ACL --- the array of ACLs

#### struct ACL

This struct represents the ACL.

  * **peers** --- array of struct Peer --- the array of peers granted in this
  ACL
  * **rules** --- array of struct Rule --- the array of rules

#### struct Peer

This struct represents the peer.

  * **type** --- byte --- the type of peer.  The valid values are:

| Value |  Meaning           |
|-------|--------------------|
| 0     | ANONYMOUS          |
| 1     | ANY                |
| 2     | RESTRICTED         |
| 3     | PUBKEY             |
| 4     | SECURITY_GROUP     |

  * **publicKey** --- array of PublicKey --- optional public key.  Maximum
array size is 1.  Only applicable for type RESTRICTED, PUBKEY, and
SECURITY_GROUP.
  * **groupID** --- byteArray --- the security group ID

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
| 0     | any         |
| 1     | method call |
| 2     | signal      |
| 3     | property    |
  * **action** --- byte --- the action mask flag.  The valid masks are:

| Mask | Name    | Description                                          |
|------|---------|------------------------------------------------------|
| 0x01 | Denied  | explicited denied                                    |
| 0x02 | Provide | sending signal, exposing method calls and properties |
| 0x04 | Observe | receiving signals and getting properties             |
| 0x08 | Modify  | set properties and make method calls                 |

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
