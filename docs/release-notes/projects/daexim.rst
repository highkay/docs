==================
Data Export/Import
==================

Major Features
==============

odl-daexim
----------

* **Feature URL:** https://git.opendaylight.org/gerrit/gitweb?p=daexim.git;a=blob;f=features/odl-daexim/src/main/feature/feature.xml;hb=refs/heads/stable/oxygen
* **Feature Description:** This is a wrapper feature which includes all
  the sub features provided by daexim project.
* **Top Level:** Yes
* **User Facing:** Yes
* **Experimental:** Yes
* **CSIT Test:** `Single node <https://jenkins.opendaylight.org/releng/view/daexim/job/daexim-csit-1node-basic-only-oxygen/>`_. `Three node cluster <https://jenkins.opendaylight.org/releng/view/daexim/job/daexim-csit-3node-clustering-basic-only-oxygen/>`_.


Documentation
=============

* **User Guide(s):**

  * :ref:`Data Export/Import User Guide <daexim-user-guide>`

* **Developer Guide(s):**

  * :ref:`Data Export/Import Developer Guide <daexim-dev-guide>`

Security Considerations
=======================

* Do you have any external interfaces other than RESTCONF?

  * No

* Other security issues?

  * N/A

Quality Assurance
=================

* `Link to Sonar Report <https://sonar.opendaylight.org/dashboard?id=org.opendaylight.daexim%3Adaexim>`_
* Code coverage is 78.1%.
* There are extensive unit-tests in the code.


Migration
---------

* Is it possible to migrate from the previous release? If so, how?

  * Migration should work across all releases.


Compatibility
-------------

* Is this release compatible with the previous release? Yes
* Any API changes? Yes. Added ability to export data on only one node. Details in yang module.
* Any configuration changes? No.

Bugs Fixed
----------

* List of bugs fixed since the previous release

  * All known bugs have been resolved.

Known Issues
------------

* None.
* `Current list of bugs <https://jira.opendaylight.org/projects/DAEXIM/issues/?filter=allopenissues>`_

End-of-life
===========

* List of features/APIs which are EOLed, deprecated, and/or removed in
  this release

  * None

Standards
=========

* List of standards implemented and to what extent

  * None

Release Mechanics
=================

* Describe any major shifts in release schedule from the release plan

  * None
