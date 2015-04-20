# org.alljoyn.Bus.Security.ApplicationState version 1


## Theory of Operation

The interface provided by all security 2.0 applications for discovery
purposes. It is seperated from the Application interface as the
ApplicationState interface is not secure while the Application interface is
secure.  High level description and flow diagrams are provided in the
Security 2.0 HLD.

## Specification

|                       |       |
|-----------------------|-------|
| Version               | 1     |

### Signals

#### ClaimState -> (publicKey, claimState)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | sessionless                      |

The ClaimState signal is used to advertise the state of an application.  It is
sessionless, because the signal is intended to discover the applications.
Discovery is not done by using the About signal as pure consumer application
might not even bother to send out an signal and even if they did, we do not
want to pollute the About info with core framework information.

Output arguments:

  * **publicKey** --- PublicKey --- the application public key
  * **claimState** --- uint16 --- the claimable state.  An enumeration
    representing the current state of the application.  The list of valid
    values:

| Value | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0     | Not claimable.  The application is not claimed and not accepting  |
|       | claim request.                                                    |
| 1     | Claimable.  The application is not claimed and is accepting claim |
|       | request.                                                          |
| 2     | Claimed. The application is claimed and can be configured.        |
| 3     | NeedUpdate. The application is claimed and requires a             |
|       | configuration update (after a software upgrade)                   |

### Named Types

#### struct PublicKey

Refer to ClaimableApplication interface description for the definition of this struct.

## References

  * The XML definition of the [ApplicationState interface](ApplicationState-v1.xml)
  * The definition of the [ClaimableApplication interface](ClaimableApplication-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
