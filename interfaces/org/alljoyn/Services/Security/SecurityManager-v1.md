# org.alljoyn.Services.Security.SecurityManager

## Introduction

The SecurityManager interface focuses on discovering security managers in the
local network. The discovery should allow finding security managers you already
trust, but also ones you don't trust. The interface is marked secure even though
anyone is allowed to access it. This will not protect us against malicious
managers. But at least if we set up a session with an honest manager
communication will remain private.
The policy rules for this interface must allow anonymous access

As anonymous access is allowed we assume:
   * All information exchanged over this interface will be public, so it should
      not contain privacy sensitive information
   * We don't know the peer we are talking to. All information we send must be
      considered public, and the information we receive may be malicious.
      We need ways to verify the content.

Note: Even if we try to limit the information that is sent over this
  interface, just mentioning that you implement the interface might be
  considered a risk.

## Theory of Operation

This interface should only be implemented by security managers. They should
advertise the presence of an object implementing it via About so that other
security managers can discover them.

A security manager is identified by the public key it uses to sign its
certificates with. So when you ask a security manager: Who are you?
Then this key is the answer. The interface offers a property to get this key.
This property cannot be retrieved by looking at the certificates used in to
set up the session. If you don't trust your peer, then setting up a ECDHE_ECDSA
session will fail as there is no trust. Furthermore the peer certificate will
be a certificate of the local agent and not one representing certificate
signing key of the security manager.
If you trust the key of the security manager, you can safely connect to
TrustedSecurityManager interface (assuming the peer security manager trust
yours as well). If not, you have to establish trust. Establishing trust means
validating that the presented key is indeed the key of the security manager you
want to interact with. Accepting this key MUST be done by a user. If two
persons want to establish trust between each other, then they have present
their keys to each other. Check if the key of the peer matches the one read
from the property and approve it.

Doing this check purely based on the public key is not user friendly at all. We
need to provide some extra meta information. This can be done with X.509
certificates. X.509 certificates are already used in core AllJoyn. Certificates
are signed so they can be transferred over an insecure channel. Who will sign
those certificates?
1. Self-signed: The security manager signs its own certificate. This enables
    transferring extra meta-data, but a man-in-the-middle can sign a
    certificate as well with the same info. Approval still needs to be done
    based on the public keys.
2. Trusted CAs: A third party could provide certificates vouching for the
    ownership of a public key. Companies hosting security managers in the
    cloud could provide additional certificates for the security managers. If
    the other user trusts the hosting company, then he can do approval based on
    certificate meta data (an e-mail address for example) and not on the public
    key. This increases the user friendliness.

If a public key needs to be approved, the security manager should provide tools
to facilitate this. Out-of-band techniques such as scanning a QR-code and
near-field-communication can be used.
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
   |                 |                                  |                 |
   |            Announced             <--               |                 |
   |                 |                                  |                 |
   |                 |    <Set up ECDHE_NULL session>   |                 |
   |                 |                                  |                 |
   |                 |    read Identity property -->    |                 |
   |                 |         <-- MngrIdentity2        |                 |
   |                 |                                  |                 |
   |  <-- Show Mngr  |                                  |                 |
   |   MngrIdentity2 |                                  |                 |
   |                 |                                  |                 |
   | use out-of-band |                                  |                 |
   | way to verify   |                                  |                 |
   | MngrIdentity2   |                                  |                 |
   |                 |                                  |                 |
   |    trust -->    |                                  |                 |
   |  MngrIdentity2  |                                  |                 |
   |                 |                                  |                 |
   |        Update trust anchors                        |                 |
   |            & policy                                |                 |
   |                 |                                  |                 |
   |                 |call RequestTrust(MngrIdentity1)->|                 |
   |                 |       <-- Pending, reqId1        |                 |
   |                 |                                  | Show request -->|
   |                 |                                  |                 |
   |                 |                                  | use out-of-band |
   |                 |                                  | way to verify   |
   |                 |                                  | MngrIdentity1   |
   |                 |                                  |                 |
   |                 |                                  |  <-- approve    |
   |                 |                                  |    request      |
   |                 |                         Update trust anchors       |
   |                 |                             & policy               |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 | <-- signal TrustRequestCompleted |                 |
   |                 |         (reqId1, ER_OK)          |                 |
   |                 |                                  |                 |
   |                 |   <Close ECDHE_NULL session>     |                 |
   |                 |                                  |                 |
   |                 |   <Set up ECDHE_ECDSA session>   |                 |
   |                 |                                  |                 |
   |                 |         make calls on            |                 |
   |                 |     TrustedSecurityManager       |                 |
   |                 |                                  |                 |
===============================================================================
```
Both security managers have 2 identity linked to them. A Manager identity and a
wire identity. The wire identity is the key pair used by the security manager
for doing AllJoyn communication. A security manager can have multiple of these
wire identities (one for each application representing it in AllJoyn). The
manager identity is holds the true identity of the manager and is the key pair
used to sign certificates with. This key pair is not always accessible to local
agent. The ECDHE_DSA session will be set up with WireIdentities, not the
manager identities.

The above flow assume Administrator 1 takes the lead. But nothing prevents
Administrator 2 to start the same flow so the out-of-band of keys happens in
parallel. In this case the RequestTrust is allowed to return Approved rather
then pending.

As the out-of-band key validation is not user friendly a PSK based alternative
is available using the PskIdentityExchanger interface on ECDHE_PSK session:
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
   |                 |                                  |                 |
   |            Announced             <--               |                 |
   |                 |                                  |                 |
   |                 |    <Set up ECDHE_NULL session>   |                 |
   |                 |                                  |                 |
   |                 |    read Identity property -->    |                 |
   |                 |         <-- MngrIdentity2        |                 |
   |                 |                                  |                 |
   |  <-- Show Mngr  |                                  |                 |
   | MngrIdentity2   |                                  |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |                 |                                  |                 |
   |  --> init PSK   |                                  |                 |
   |                 |                                  |                 |
   |           host session                             |                 |
   |          (only allow PSK)                          |                 |
   |             Generate PSK                           |                 |
   |                 |                                  |                 |
   |  <-- show PSK   |                                  |                 |
   |                 |                                  |                 |
   |                 | call JoinPskSession -->          |                 |
   |                 |  (sessionPort,MngrIdentity1)     |                 |
   |                 |   <Close ECDHE_NULL session>     |                 |
   |                 |                                  |  Show request   |
   |                 |                                  |                 |
   |                 |                                  |      Get PSK    |
   |                 |                                  |   (out-of-band) |
   |                 |                                  |                 |
   |                 |                                  | <-- give PSK    |
   |                 |   <Set up ECDHE_PSK session>     |                 |
   |                 |                                  |                 |
   |                 |  <-- call PskIdentityExchanger   |                 |
   |                 |       exchange(MngrIdentity2)    |                 |
   |                 |          MngrIdentity1 -->       |                 |
   |                 |                                  |                 |
   |        Update trust anchors               Update trust anchors       |
   |            & policy                           & policy               |
   |                 |                                  |                 |
   |                 |   <Close ECDHE_PSK session>      |                 |
   |                 |                                  |                 |
   |                 |   <Set up ECDHE_ECDSA session>   |                 |
   |                 |                                  |                 |
   |                 |         make calls on            |                 |
   |                 |     TrustedSecurityManager       |                 |
   |                 |                                  |                 |
===============================================================================
```
In the JoinPpsKSession the MngrIdentity is send as well. It can be used to
show extra data to the administrator. Both parties have each other identities
prior to setting up the PSK session. However since they were transferred over
NULL session, we exchange them again over the PSK session to make sure we have
receive them from a trusted source.

We offer 2 flows. One based on PSK and one over NULL sessions. In general the
PSK is the most user friendly. It requires the exchange of the PSK nothing more
The NULL based exchange requires the validation of the public keys. Which is
not at all user friendly. It becomes interesting however when the identities of
the security managers are described by a trusted 3rd party. Validation doesn't
have to be based on the public key, but the certificate meta data can be used.
For example: it could contain the email address of the owner.

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

#### Identity

|-----------------------|-----------------------------------------------------------------------|
| Type                  | SecurityManagerIdentity                                               |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = false       |

The identity property contains the security manager identity. It is a structure
containing the public key of the security manager and optional certificate
chain (an array of certificates). If this security manager does not have a
certificate to identify itself, then the SecurityManagerIdentity contains an
empty certificate array.

### Methods

#### RequestTrust(identity) -> (reply,replyId)

Request the implementing application to trust the identity provided as input.

Input arguments:

  * **identity** --- SecurityManagerIdentity --- The identity of the security
     manager to trust.

Output arguments:

  * **reply** --- y --- The answer to the request. An enumeration describing
     the reply. Values:
         0: Request approved
         1: Request denied
         2: Request pending
  * **requestId** --- q --- The id of the request.

Errors raised by this method:

 *

#### CancelTrustRequest(requestId)

Cancels an ongoing trust request.

Input arguments:
  * **requestID** --- q --- The id of the request to cancel.


#### IsTrustRequestCompleted(requestId) -> (progressStatus)

Checks the progress of trust request. Returns an AllJoyn error if
           the requestID is not known.

Input arguments:
  * **requestId** --- q --- The id of the request.

Output arguments:
  * **progressStatus** --- y --- An enumeration describing the reply. Values:
         0: Request approved
         1: Request denied
         2: Request pending

#### JoinPskSession(sessionPort,identity)

Ask this security manager to join a PSK session on the specified port with
the security manager identified by the given identity.

Input arguments:
  * **sessionPort** --- q --- The session port requester is listening on.
  * **identity** --- SecurityManagerIdentity --- The identity of the requester.

### Signals

#### TrustRequestCompleted -> (requestId,status)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                       |


This signal is sent to the requester to notify him when a request to trust is
completed.

Output arguments:

  * **requestId** --- q --- The identifier of the request.
  * **status** --- q --- The result of the request. Values:
         0: Request approved
         1: Request denied
         2: Request timed out
         3: Request failed

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

### struct SecurityManagerIdentity

A struct defining the identity of a security manager.
  * **managerPublicKey** --- [PublicKey] --- The public key identifying the
    security manager.
  * **managerIdentityCertificateChain** ---a[Certificate] --- The certificate
    chain identifying the manager or an empty array if the manager doesn't have
    a certificate or doesn't want to
                share it.

### Interface Errors

This interface uses the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The
table below lists the possible errors raised by this interface.

| Error name                                           | Error message      |
|------------------------------------------------------|--------------------|
| org.alljoyn.Services.Security.Error.InvalidRequestId | Invalid RequestId  |