# The SecurityManager Interfaces

## Introduction

Within security 2.0 applications communicate in a secure way with each other.
Every application or device needs to be configured and managed. Configuring an
ecosystem does require knowledge of the whole system. Who is involved, what
permissions are granted to whom. System administrators need tools to help them
with this. These tools are security managers. Many of the features security 2.0
is offering (like security groups, identity management, group memberships) are
handled within the scope of a single manager. Others like delegation and
security group equivalence require interaction between security managers.

In case of delegation, one security manager needs to generate a certificate
and hand it over to the latter. Once the second security manager gets the
certificate, it can/should generate membership certificates based on it. But
not only membership certificates need to be shared. Both security managers need
to update the policy of the applications they manage. They should be able to
give hints each other on what they can offer and what they'd like to use.

## Interfaces
The security manager interfaces are provided by security managers to
applications and other security managers in the local network. The service is
split into three interfaces:
1. The SecurityManager interface
2. The PskIdenityExchanger Interface
3. The TrustedSecurityManager interface

The SecurityManager interface is provided by security managers so that other
peers can discover them and retrieve some basic information from them. As it
is also used for discovery purposes, means that involved peers might not have
established a trust relationship.  This interface is secured, but should be
configured to allow access from anonymous users. But it also means that you
don't know to which peer you are talking to. Is it the one you intended to or
a fake one? Is there a man in the middle or not? We don't know. We should
design the interface in such a way that we don't leak privacy sensitive
information and that a man-in-the-middle can't do any harm.

Establishing trust between peers requires some attention. The discovery and
initial trust establishment happens between anonymous user. This will require
that all info sent over this session needs to be validated. As this involves
validating public keys, we should look for alternatives. ECDHE_PSK offers means
to setup a session based on a pre-shared secret. Once we exchange the secret,
we can setup a session between 2 peer known peers. They can exchange all the
information needed to setup an authenticated session. This information exchange
is defined by the PskIdenityExchanger interface.

Once trust between two peers is established, peers should communicate via the
TrustedSecurityManager interface. This interface is secure and allows only
authenticated and authorized access. All functionalities provided by security
managers will be accessible over this interface.
