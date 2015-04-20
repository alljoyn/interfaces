# org.alljoyn.Bus.Security.ApplicationState version 1


## Theory of Operation

The interface provided by all security 2.0 applications for discovery
purposes. It is separated from the Application interface as the
ApplicationState interface is not secure while the Application interface is
secure.

## Specification

|                       |       |
|-----------------------|-------|
| Version               | 1     |

### Signals

#### State -> (publicKey, claimState)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | sessionless                      |

The State signal is used to advertise the state of an application.  It is
sessionless, because the signal is intended to discover security v2 applications.
Discovery is not done by using 'About'.  Applications must add extra code to
provide About.  Not all applications will do this as pure consumer applications
don't need to be discovered by other applications.  Still they need to be
discovered by the security manager. Furthermore we want to avoid interference
between core framework events and application events.

Output arguments:

  * **publicKey** --- EccPublicKey --- the application public key
  * **state** --- uint16 --- the application state.  An enumeration
    representing the current state of the application.  The list of valid
    values:

| Value | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0     | Not claimable.  The application is not claimed and not accepting  |
|       | claim requests.                                                    |
| 1     | Claimable.  The application is not claimed and is accepting claim |
|       | requests.                                                          |
| 2     | Claimed. The application is claimed and can be configured.        |
| 3     | NeedUpdate. The application is claimed, but requires a            |
|       | configuration update (after a software upgrade).                  |

### Named Types

#### struct EccPublicKey

Refer to Application interface description for the definition of this struct.

## References

  * The XML definition of the [ApplicationState interface](ApplicationState-v1.xml)
  * The definition of the [Application interface](ClaimableApplication-v1.md)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
