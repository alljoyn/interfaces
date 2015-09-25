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
      * org.alljoyn.Foo/: contains the interface definitions for namespace
                          org.alljoyn.Foo
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
  in extra/interface-definition-template.md . Make sure to follow the
  formatting guidelines as described in the template!
* create a formal XML description of the interface
* add both to this repository in the appropriate place (see the Structure
  section)
* make sure the markdown files render correctly to HTML (see below)
* submit the change to the Allseen Alliance Gerrit infrastructure, in the same
  way as you would do for a code contribution to some project.
* send an email to allseen-irb@lists.allseenalliance.org, requesting a review of
  your submission.

The IRB will then provide feedback through the Gerrit tool. The IRB will try to
provide feedback in at most two weeks. If your interface definition is accepted,
it will be merged into the interfaces repository. From that point on, it is
considered a standardized AllJoyn interface.

How To Check The Markdown Rendering
-----------------------------------
The IRB makes use of the tooling developed by the webdocs project to render
markdown files to HTML, and publish them on the Allseen Alliance web site.
We ask that you verify whether your submission renders correctly with this
tooling prior to submission for review.

Here is how:

* clone the webdocs repository (preferably next to the interfaces repository):
  `git clone https://git.allseenalliance.org/gerrit/extras/webdocs`
* `cd extras/webdocs`
* perform the necessary setup (installation of node.js and modules) as described
  in the webdocs README.md file.
* run the rendering script: `scripts/generate_interfaces.js`. If you cloned the
  webdocs repository in another location, you'll need to specify the location of
  the interfaces repository with the `-d` command-line switch.
* the rendered interface descriptions can be found in `out/public`.
* run the link checker script: `scripts/linkchecker.js -s /interfaces/index`
* for easy verification of the HTML rendering, you can start a local web server:
  `scripts/server.js`. Point your favorite web browser to
  `http://localhost:8000/` to view the result.
