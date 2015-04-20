# org.alljoyn.Bus.Application version 1


## Theory of Operation
The interface is provided by all applications (from release 15.08 onwards) for
discovery purposes. This interface is implemented by the Alljoyn framework and
targets core framework features and services like Security v2. Application
developers should still use About to allow others to discover their services or
to find providers of services they are interested in.

The interface is not secure.

## Specification

|                       |       |
|-----------------------|-------|
| Version               | 1     |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false |

The interface version number.

### Signals

#### State -> (publicKey, claimState)

|                       |                                  |
|-----------------------|----------------------------------|
| Signal Type           | sessionless                      |

The State signal is used to advertise the state of an application.  It is
sessionless, because the signal is intended to discover applications. Discovery
is not done by using 'About'.  Applications must add extra code to provide About.
Not all applications will do this as pure consumer applications don't need to be
discovered by other applications.  Still they need to be discovered by the
framework to support certain some core framework features. Furthermore we want to
avoid interference between core framework events and application events.

Output arguments:

  * **publicKey** --- EccPublicKey --- the application public key
  * **state** --- uint16 --- the application state.  An enumeration
    representing the current state of the application.  The list of valid
    values:

| Value | Description                                                       |
|-------|-------------------------------------------------------------------|
| 0     | NotClaimable.  The application is not claimed and not accepting   |
|       | claim requests.                                                   |
| 1     | Claimable.  The application is not claimed and is accepting claim |
|       | requests.                                                         |
| 2     | Claimed. The application is claimed and can be configured.        |
| 3     | NeedUpdate. The application is claimed, but requires a            |
|       | configuration update (after a software upgrade).                  |

### Named Types

#### struct EccPublicKey

This struct represents the Elliptic Curve Cryptography public key.

  * **algorithm** --- byte --- the algorithm.  The current valid value is
0 – ECDSA with SHA-256
  * **curveIdentifier** --- byte --- The ECC curve id.  The current valid value is
0 – NIST P-256
  * **xCoordinate** --- byte[] --- The x coordinate of the point
  * **yCoordinate** --- byte[] --- The y coordinate of the point

## References

  * The XML definition of the [Application interface](Application-v1.xml)
  * The Security 2.0 HLD can be found at [the Security 2.0 wiki](https://wiki.allseenalliance.org/core/security_enhancements)
