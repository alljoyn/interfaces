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
hint each other on what they can offer and what they'd like to use.

## Interfaces
The security manager interfaces are provided by security managers to
applications and other security managers in the local network. The service is
split into two interfaces:
1. The SecurityManager interface
2. The TrustedSecurityManager interface

The SecurityManager interface is provided by security managers so that other
peers can discover them and retrieve some basic information from them. As it
is also used for discovery purposes, means that involved peers might not have
established a trust relation.  This interface is not secured to allow this.
But it also means that you don't know to which peer you are talking to. Is it
the one you intended to or a fake one? Is there a man in the middle or not? We
don't know. We should design the interface in such a way that we don't leak
privacy sensitive information and that a man-in-the-middle can't do any harm.

Once trust between two peers is established, peers should communicate via the
TrustedSecurityManager interface. This interface is secure and allows
authenticated access. All functionalities provided by security managers will be
accessible over this interface.
