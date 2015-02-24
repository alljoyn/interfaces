Introduction
------------
This is the standardized AllJoyn Interface Definition repository for the Allseen
Alliance. It is maintained by the Interface Review Board.

Structure
---------
This repository is structured as follows:

  * /: the root of the repository. Contains this README file.
    * extra/: some auxiliary information and tooling
    * interfaces/: the standardized interface definitions
      * org.allseen.Foo/: contains the interface definitions for namespace
                          org.allseen.Foo
        * Bar-v1.xml: the formal interface definition for org.allseen.Foo.Bar v1
        * Bar-v1.md: the textual description of org.allseen.Foo.Bar v1
        * theory-of-operation.md: an overall Theory of Operation document that 
                                  covers all interfaces in this namespace.
                                  This is optional: the Theory of Operation may
                                  be contained in the individual interface
                                  definition documents.

Submission Procedure
--------------------
All standardized AllJoyn interfaces must be reviewed by the Interface Review
Board before they can be added to this repository. The submission procedure is
fairly simple:

* create a textual description of the interface following the template provided
  in extra/interface-definition-template.md
* create a formal XML description of the interface
* add both to this repository in the appropriate place (see the Structure
  section)
* submit the change to the Allseen Alliance Gerrit infrastructure, in the same
  way as you would do for a code contribution to some project.
* send an email to allseen-irb@lists.allseenalliance.org, requesting a review of
  your submission.

The IRB will then provide feedback through the Gerrit tool. The IRB will try to
provide feedback in at most two weeks. If your interface definition is accepted,
it will be merged into the interfaces repository. From that point on, it is
considered a standardized AllJoyn interface.
