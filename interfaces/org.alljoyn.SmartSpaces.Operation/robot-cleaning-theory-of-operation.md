## Robot Cleaning Theory of Operation version 1

This theory of operation explains the interaction of various interfaces to
assemble a robot cleaner.

### Overview

Robot cleaner is an autonomous robotic vacuum cleaner that has an intelligent
programming and a limited vacuum cleaning system. HAE service framework
defines a common set of standard interfaces that work across multiple device
types. This document illustrates how to use those interfaces for monitoring
and control of typical robot cleaner operation.

The followings are the candidate standard interfaces for AllJoyn repretation of
typical robot cleaner. The robot cleaner is classified as a cyclic operation
device in HAE service framework.

#### org.alljoyn.SmartSpaces.Operation.OnOffStatus

It provides a capability to monitor power on and off status of
the robot cleaner.

#### org.alljoyn.SmartSpaces.Operation.CycleControl

It provides a capability to monitor operational state of the robot cleaner
such as **Idle**, **Working**, **Paused**, and **EndOfCycle**, etc.

* **Idle** --- the robot cleaner is in the home station, and typically it
is charging its internal battery.
* **Working** --- the robot cleaner is working, and its current cycle phase
of working can be further monitored through RobotCleaningCyclePhase interface.
* **Paused** --- the cleaning operation is paused. The robot cleaner remains
at its last location where it received the **Pause** command from CycleControl
interface.
* **EndOfCycle** --- After completing its cycle operation, typically cleaning
and homing, it may inform users or peer devics momentarily its end of cylce.
After a short period of EndOfCycle, its operational state will typically transit
to **Idle** state.

This interface also provides a capability to control the operation of the
robot cleaner by excuting the commands such as **Start**, **Pause**, and
**Stop**.

* **Start** or **Resume** --- start or resume the cycle operation.
* **Pause** --- pause its cleaning or homing operation.
* **Stop** --- stop its cycle operation and return back to the home station.

#### org.alljoyn.SmartSpaces.Operation.BatteryStatus

It provides a capability to monitor whether the battery of the robot cleaner
is being charged or not and its current level of remaining battery.

#### org.alljoyn.SmartSpaces.Operation.RobotCleaningCyclePhase

It provides a capability to monitor the current cycle phase of robot cleaning
operation. The cycle of robot cleaning operation consists of **Cleaning** and
**Homing** phases.
