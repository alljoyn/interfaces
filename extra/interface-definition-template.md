# org.allseen.Foo.Bar version 1

## Theory of Operation

[If you are defining multiple interfaces that belong together, split the Theory
of Operation section off into a separate document in the root of your chosen
interface namespace. Be sure to reference it in the References section below!]

Provide information that helps explain the interface. Typically, this includes

  * a summary of the interaction roles (normally, _producer_ and _consumer_)
  * a list of common use cases
  * common interaction flows (discovery, what conditions trigger signal emission, ...)
    Keep this bit very brief - if longer walkthroughs are necessary, add them at the
    end of the document in an Examples section.
  * relation with other Interfaces (e.g., there is a clear relationship between the
    Lighting Service Framework's Lamp Interface and the Lighting Controller Interface)

## Specification

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = true                                         |

### Properties

#### ComplexProperty

|-----------------------|-----------------------------------------------------------------------|
| Type                  | Baz                                                                   |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = true        |
| Introduced in version | n (only add this line for n > 1)                                      |

There is room for a long, tedious and incredibly detailed explanation of the property
here, in multiple paragraphs and with lists if needed.

#### SimpleProperty

|-----------------------|-----------------------------------------------------------------------|
| Type                  | int32                                                                 |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = invalidates |
| Introduced in version | n (only add this line for n > 1)                                      |

There is room for a long, tedious and incredibly detailed explanation of the property
here, in multiple paragraphs and with lists if needed.

#### z

|-----------------------|-----------------------------------------------------------------------|
| Type                  | string                                                                |
| Access                | read-write                                                            |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = false       |
| Introduced in version | n (only add this line for n > 1)                                      |

Descriptions can be short and sweet, too.


### Methods

#### DoSomething(in1) -> (out1)

|-----------------------|---------------------------------------------|
| Introduced in version | n (only add this line for n > 1)            |

Description of the method. This may be multi-line.

Input arguments:

  * **in1** --- Frob --- Frob is the kind of dictionary that really deserves a
    thorough discussion when you're using it as an argument to a method. It really
    does. How else would we test multi-line argument descriptions?

Output arguments:

  * **out1** --- float[] --- an array of floating point values

Errors raised by this method:

 * org.allseen.Foo.Bar.Error.BazNotHome --- with some explanation if needed

#### DoNothing()

|-----------------------|---------------------------------------------|
| Introduced in version | n (only add this line for n > 1)            |
| Annotation            | org.freedesktop.DBus.Method.NoReply = true  |

Description of the method. It does nothing, there are no inputs, no replies and no
errors. So we can leave out all those sections below.

### Signals

#### SomethingHasBeenDone -> (what)

|-----------------------|-----------------------------------|
| Introduced in version | n (only add this line for n > 1)  |
| Signal Type           | sessioncast                       |

(possible signal types are: unicast, sessioncast, sessionless, local broadcast,
global broadcast. As per the Interface Design guidelines, the default signal
type is sessioncast. If this signal is not sessioncast, make sure to explain
the rationale.)

Description of the signal.

Output arguments:

  * **what** --- string --- textual description of what happened

### Named Types

[Delete this section if not applicable]

#### struct Baz

A description of the meaning and purpose of struct Baz

  * **x** --- string --- description of the x field
  * **y** --- int32 --- description of the y field

#### dictionary Frob

A description of the meaning and purpose of dictionary Frob

  * **key** --- string --- description of the dictionary key
  * **value** --- byte --- description of the value

If applicable, provide a list of expected key values, and a precise description
of the expected values for each key.

### Interface Errors

[Delete this section if not applicable.]

The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                             | Error message             |
|----------------------------------------|---------------------------|
| org.allseen.Foo.Bar.Error.BazNotHome   | Baz is not home.          |

## Examples

[Delete this section if not applicable]

In this section, you can place more elaborate illustrations of the typical call
flow, that would overburden the Theory of Operation section.

## References

In this section, list references for the Interface definition (standards, RFCs, related interfaces, etc.).
Make sure to include at least the following references:

  * the formal XML definition of the Interface described in this document
  * the Theory of Operation document, if you decided to split it off
  * the interface definition document for related interfaces (especially those
    that are described in the same Theory of Operation document)
