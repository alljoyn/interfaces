## Lock Theory of Operation version 1

This theory of operation explains the interaction of various interfaces to
assemble a locking mechanism for a device.

### Overview

A lock is defined as a device or mechanism that can be put into a locked/unlocked
state. The CDM service framework defines a common set of standard interfaces that work
across multiple device types. This document illustrates how to combine a subset
of those interfaces to create a lock.

#### org.alljoyn.SmartSpaces.Operation.IsLocked

This interface provides the capability to monitor the current
status of the locking mechanism and establish whether its currently engaged (locked) or
disengaged (unlocked). A minimal lock may only implement this interface and
require the act of locking/unlocking to be done via a physical control.

#### org.alljoyn.SmartSpaces.Operation.LockControl

This interface provides the capability to engage the locking mechanism.

#### org.alljoyn.SmartSpaces.Operation.UnlockControl

This interface provides the capability to disengage the locking mechanism.

#### org.alljoyn.SmartSpaces.Operation.RemoteControllability

This interface provides the capability for a locking mechanism to be have its
locking/unlocking controls enabled or disabled. When RemoteControllability is
disabled, the Lock/Unlock functionality will return an error.
