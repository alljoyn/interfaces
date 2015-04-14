# org.alljoyn.Services.Security.TrustedSecurityManager

## Introduction

The TrustedSecurityManager interface focuses on exchanging information between
2 security managers which a trust relations. Communication MUST be done over a
ECHDE_ECDSA session. The usage of security 2.0 is a first line of protecting
this interface, but a second layer of access needs to be added in the security
manager application itself. Two extra layers of access are required. This
extra access check are based on the peer certificates.
1. Check if the peer is a representative of the security manager: The trust is
based on the signing key of the peer security manager. This key should not be
used to communicate. The peer must be present a certificate providing proof he
is in fact a valid security manager.
2. When a request comes in on behalf of a security manager, then the reply
should only contain what is relevant to manager. Reply should not expose
information on other trusted security managers.

The main use cases for the TrustedSecurityManager interface are to support
restricted user and delegation scenarios.

## Theory of Operation

This interface provides support four flows. Two flows for delegation and 2 for
restricted user. Both scenarios have a flow where one peer request rights and
one where a peer hands out rights.

Rights are the actions you want to be able to do. This is similar to manifest.
Rights are a set a of Rules. Requesting rights means that the peer needs to
approve the request, just like manifests of application are approved. When
those rights are granted, applications need to know how they can use them. This
might require a policy update. The flows are allow the granter of rights to
tell the receiver how he can use them. A policy template is sent as well.
Compared to a manifest a policy template does contain more then rules. It also
defines the peers to whom those rules apply. A received template should not be
considered a full policy, but more like an extension to existing ones.

The policy templates for the restricted user case are not exchanged. The
template can be calculated based on the requested rights. Is is An ACL with a
single peer of restricted user type. The key of this peer matches the manager
signing key. The Rules in this ACL are almost identical to the requested rules.
The only difference is the action mask. Provide becomes read and modify. Read
and/or modify becomes provide.

The following flows assumes trust has been established by via the
SecurityManager interface.

Flow 1 describes the flow where Administrator 1 grant delegation rights to
administrator 2.

```
===============================================================================

   0               -----                              -----               0
  \|/              |   |                              |   |              \|/
   |               |   |                              |   |               |
  / \              -----                              -----              / \

Admin 1            Mngr 1                             Mngr 2           Admin 2
   |                 |                                  |                 |
   |                 |-- MngrIdentity1                  |-- MngrIdentity2 |
   |                 |-- WireIdentity1                  |-- WireIdentity2 |
   |                 |                                  |                 |
   |                 |                    Announce SecurityManager        |
   |                 |                 Announce TrustedSecurityManager    |
   |                 |                                  |                 |
   |            Announced             <--               |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
select & define      |                                  |                 |
  delegation         |                                  |                 |
   scenario          |                                  |                 |
   |                 |                                  |                 |
grant rights         |                                  |                 |
to Mngr2             |                                  |                 |
   |             generate                               |                 |
   |          certificate(s)                            |                 |
   |             identity                               |                 |
   |      (optional memberships)                        |                 |
   |                 |                                  |                 |
   |         update policy of                           |                 |
   |        local applications                          |                 |
   |                 |                                  |                 |
   |             generate                               |                 |
   |          policy template                           |                 |
   |                 |                                  |                 |
   |            Look for Mngr2                          |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 |   <Set up ECDHE_ECDSA session>   |                 |
   |                 |                                  |                 |
   |          Validate peer                             |                 |
   |           certificate                              |                 |
   |                 |                                  |                 |
   |                 | call GrantDelegationRights -->   |                 |
   |                 |      DelagetedIdentity2          |                 |
   |                 |      policy template             |                 |
   |                 |                            Validate peer           |
   |                 |                             certificate            |
   |                 |     <-- completed = false        |                 |
   |                 |                                  | process request |
   |                 |                                  | show to Admin 2 |
   |                 | optional call       -->          |                 |
   |                 | GrantMemberships(certs, template)|                 |
   |                 |                            Validate peer           |
   |                 |                             certificate            |
   |                 |     <-- completed = false        |                 |
   |                 |                                  | process request |
   |                 |                                  | show to Admin 2 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 |                                  | <-- approve &   |
   |                 |                                  | update config   |
   |                 |                                  |                 |
   |                 |        <--  signal               |                 |
   |                 |  GrantDelegationRightsCompleted  |                 |
   |                 |         success                  |                 |
   |                 |        <--  signal               |                 |
   |                 |    GrantMembershipsCompleted     |                 |
   |                 |         success                  |                 |
   |                 |                                  |                 |
   |                 |   <Close ECDHE_ECDSA session>    |                 |
   |                 |                                  |                 |
   |                 |                           update policy of         |
   |                 |                          local applications        |
   |                 |                             & distribute           |
   |                 |                             certificates           |
   |                 |                                  |                 |
===============================================================================
```

In flow 2 is administrator 1 ask delegation rights from administrator 2.
```
===============================================================================

   0               -----                              -----               0
  \|/              |   |                              |   |              \|/
   |               |   |                              |   |               |
  / \              -----                              -----              / \

Admin 1            Mngr 1                             Mngr 2           Admin 2
   |                 |                                  |                 |
   |                 |-- MngrIdentity1                  |-- MngrIdentity2 |
   |                 |-- WireIdentity1                  |-- WireIdentity2 |
   |                 |                                  |                 |
   |                 |                    Announce SecurityManager        |
   |                 |                 Announce TrustedSecurityManager    |
   |                 |                                  |                 |
   |            Announced             <--               |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
select delegation    |                                  |                 |
  rights needed      |                                  |                 |
   scenario          |                                  |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 |   <Set up ECDHE_ECDSA session>   |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |          Validate peer                             |                 |
   |           certificate                              |                 |
   |                 |                                  |                 |
   |                 | call RequestDelegationRights --> |                 |
   |                 |             rights               |                 |
   |                 |                            Validate peer           |
   |                 |                             certificate            |
   |                 |      policy template             |                 |
   |                 |     <-- requestID                |                 |
   |                 |                                  | process request |
   |                 |                                  | show to Admin 2 |
   |                 |                                  |                 |
   |                 |                                  | <-- approve &   |
   |                 |                                  | update config   |
   |                 |                              generate              |
   |                 |                           certificate(s)           |
   |                 |                              identity              |
   |                 |                       (optional memberships)       |
   |                 |                                  |                 |
   |                 |                         update policy of           |
   |                 |                        local applications          |
   |                 |                                  |                 |
   |                 |                               generate             |
   |                 |                            policy template         |
   |                 |                                  |                 |
   |                 |        <--  signal               |                 |
   |                 | DelegationRequestRightsCompleted |                 |
   |                 |         success                  |                 |
   |                 |         requestId                |                 |
   |                 |         certificates             |                 |
   |                 |         policy template          |                 |
   |                 |                                  |                 |
   |                 |   <Close ECDHE_ECDSA session>    |                 |
   |                 |                                  |                 |
   |         update policy of                           |                 |
   |        local applications                          |                 |
   |           & distribute                             |                 |
   |           certificates                             |                 |
   |                 |                                  |                 |
===============================================================================
```


## Specification

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

#### Version

|-----------------------|-----------------------------------------------------------------------|
| Type                  | q                                               |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = false       |

The version of this interface. 1 for this version.

### Methods

#### GrantDelegationRights(identityCertificate,requiredPermissions) -> (completed)

This method is called to grant a security manager delegation rights.

Input arguments:
  * **identityCertificate** --- [Certificate] --- an identity certificate for
    this security manager The provided certificate must be an identity
    certificate with delegation set to true. Upon receiving this method call
    the security manager must check if the certificate is acceptable and if the
    method caller is entitled to give you this permissions. The security
    manager should store this certificate. If it can't do this in a reasonable
    time, it should return this method call with completed set to false. The
    caller should keep the session alive and listen for the
    GrantDelegationRightsCompleted signal. The identity of the security manager
    to trust.
  * ** requiredPermissions** --- a[Acl] --- The ACLs this security manager
    needs to add to the policies to be able to use this identity certificate.
Output arguments:
  * **completed** --- y --- True if the request was handled in line. false if
    the request is handled asynchronously.

#### GrantMemberships(membershipCertificates,requiredPermissions) -> (completed)

This method is called to grant a security manager delegated membership
certificates.
Input arguments:
  * **membershipCertificates** --- a[Certificate] --- The granted certificates.
    The provided certificates must be membership certificates with delegation
    set to true. Upon receiving this method call the security manager must
    check if the certificates are acceptable and if the method caller is
    entitled to give you this permissions. The security manager should store
    the certificates. If he can't do this in a reasonable time, he should
    return this method call with completed set to false. The caller should keep
    the session alive and listen for the GrantMembershipsCompleted signal.
  * ** requiredPermissions** --- a[Acl] --- The ACLs this security manager
    needs to add to the policies to be able to use these membership
    certificates.
Output arguments:
  * **completed** --- y --- True if the request was handled in line. false if
    the request is handled asynchronously.

#### RequestDelegationRights(rights) -> (requestId)
Request the security manager to receive delegation rights.

Input arguments:
  * **rights** --- a[Rule] --- the rights requested

Output arguments:
  * **requestId** --- q --- The id of the request. A requestId is only valid in
    the AllJoyn session which made the request.
Error
 * RequestDenied

#### HasDelegationRights(certificateSerialNumber,issuerPublicKey) -> (hasCertificate)
Queries the security manager to see if it holds a specific identity
certificate. If the manager is unable to answer this query in a timely
manner an error should be returned.

Input arguments:
  * **certificateSerialNumber** --- s --- The serial number of the certificate.
  * **issuerPublicKey** --- [EccPublicKey] --- The public key of the issuer.

Output arguments:
  * **hasCertificate** --- b --- True if the manager has the certificate, false
    otherwise.
Error
 * RequestDenied
 * Unavailable

#### RequestRights(rights) -> (requestId)
Request the security manager to receive restricted user rights.

Input arguments:
  * **rights** --- a[Rule] --- the rights requested

Output arguments:
  * **requestId** --- q --- The id of the request. A requestId is only valid in
    the AllJoyn session which made the request.
Error
 * RequestDenied

#### GrantRights(rights) -> (requestId)
Grants the security manager rights to be used with restricted user rights.
The security manager will forward this request to its administrator and reply
asynchronously via the GrantRightsCompleted signal.

Input arguments:
  * **rights** --- a[Rule] --- The rules granted.

Output arguments:
  * **requestId** --- q --- The id of the request. A requestId is only valid in
    the AllJoyn session which made the request.
Error
 * RequestDenied

### Signals

#### GrantDelegationRightsCompleted -> (status)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                           |


This signal is sent to the requester to notify him when a
GrantDelegationRightsCompleted is completed.

Output arguments:
  * **status** --- q --- The result of the request. Values:
    0: Grant rights success
    1: Grant rights failed

#### GrantMembershipsCompleted -> (status)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                           |


This signal is sent to the requester to notify him when a
GrantMembershipsCompleted is completed.

Output arguments:
  * **status** --- q --- The result of the request. Values:
    0: Grant rights success
    1: Grant rights failed

#### DelegationRightsRequestCompleted -> (status,requestId,certificates,requiredPermissions)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                           |


This signal is sent to the requester to notify him when a
RequestDelegationRights call is completed.

Output arguments:
  * **status** --- q --- The result of the request. Values:
    0: Grant rights success
    1: Grant rights failed
  * **requestId** --- q --- The id of the request.
  * **certificates** --- a[Certificate] --- the certificates needed to exercise
    the rights. The certificate array should contain 1 identity certificate and
    optionally one or more membership certificates.
  * ** requiredPermissions** --- a[Acl] --- The ACLs this security manager
    needs to add to the policies to be able to use these certificates.

#### RightsRequestCompleted -> (status,requestId)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                           |


This signal is sent to the requester to notify him when a
RequestRights call is completed.

Output arguments:
  * **status** --- q --- The result of the request. Values:
    0: Grant rights success
    1: Grant rights failed
  * **requestId** --- q --- The id of the request.

#### GrantRightsCompleted -> (status,requestId)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                           |


This signal is sent to the requester to notify him when a
RequestDelegationRights call is completed.

Output arguments:
  * **status** --- q --- The result of the request. Values:
    0: Grant rights success
    1: Grant rights failed
  * **requestId** --- q --- The id of the request. A requestId is only valid in
    the AllJoyn session which made the request.

### Named Types

#### struct PublicKey

A struct providing the details of an Elliptic Curve public key.
  * **algorithm** --- y --- An enumeration describing the key type:
       0: ECDSA with SHA256
  * **curveIdentifier** --- y --- An enumeration identifying the curve this key
     is using.
       0: NIST P-256
  * **xCoordinate** --- ay --- The X coordinate of the public key.
  * **yCoordinate** --- ay --- The Y coordinate of the public key.

### struct Certificate

A struct providing the details of a certificate.
  * **encoding** --- q --- An enumeration representing the encoding of the
     certificate data. Values:
       0: a X.509 DER encoded certificate.
       1: a X.509 PEM encoded certificate.
  * **certificateData88 --- ay --- The encoded certificate data

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

### Interface Errors

This interface uses the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The
table below lists the possible errors raised by this interface.

| Error name                                           | Error message      |
|------------------------------------------------------|--------------------|
| org.alljoyn.Services.Security.Error.RequestTimeOut   | Request TimeOut    |
| org.alljoyn.Services.Security.Error.RequestDenied    | Request denied     |