..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the master branch.

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

Purpose
=======

Clients who are eligible to receive data products from the LSST project
will require a service for the distribution of that data to their local
sites. The amount of data involved will be significant (hundreds of TBs
to PBs) so whatever tool is chosen must be capable of performing well
over long distance, high speed networks. The system must also be able to
operate in a sustained mode so that data can be transferred as soon as
it is ready, in order to reduce the likelihood of overwhelming the data
source on “flag day”. There are a variety of basic tools that could be
leveraged to implement such a system, such as ftp, rsync, REST, scp, and
so on. However, significant software development and ongoing maintenance
would be required to build the desired service out of these tools. With
an eye toward leveraging existing middleware tools that are supported by
a broad community, this paper will not discuss these utilities. There
are as well, a plethora of new open source projects such as upspin,
duplicity, rclone, and more, that have sprung up to aid in data transfer
amongst disparate cloud storage services. These applications are not
intended for use at the data volume required by LSST and, as such, are
also not considered here. A third class of storage services that do not
fit the prerequisites of a data distribution system, but that might
still be of interest to the project, are those such as Fedora Commons
(https://duraspace.org/fedora/), DSpace
(https://duraspace.org/dspace/about/), and even GitHub
(https://github.com/features). These services offer a variety of
platforms for data sharing and access.

Middleware Tools
================

These software systems are more broadly capable toolsets created by
communities with needs to distribute/share data similar to LSST’s
requirements. The first tools/services have well established communities
of users and developers. Zettar zx is included here as an item of
interest but is both relatively new and a commercial product.

-  dCache

   -  https://www.dcache.org/

-  Globus Online

   -  https://www.globus.org/

-  iRODS

   -  https://irods.org/

-  RUCIO

   -  https://rucio.cern.ch/

-  XRootD

   -  http://www.xrootd.org/

-  Zettar zx

   -  https://www.zettar.com

dCache
------

The dCache software package is designed to create a single namespace for
distributed data. It can support geographically dispersed locations and
provides data replication and access steering. The software is open
source and is used, largely, in the high energy physics centers such as
to support many of the tier one CERN LHC sites. It supports data access
via NFSv4.1 (jrpc), HTTP/WebDav and xrootd4j as well as a native API
(libdcap) and xRootD.

Pros:

-  Well established and supported product

-  Open source

-  Supports many modes of operation, including using tiered/HSM storage

-  Can run on a wide range of systems

-  Has support for immutable files

-  Has support for usage billing

Cons:

-  Complex to set up and administer

-  May require application integration

-  Global namespace may not be desired for LSST’s application

-  Requires client sites to “join” the file system

Globus Online
-------------

Globus Online is a data transfer and sharing service built on GridFTP
and integrated cloud services. It offers users both a simple to use web
interface as well as a Python API and a simple shell CLI. The service
provides highly resilient, “hands free” data transfer. The service
supports automated retries, encryption, file check summing, restarts,
and access to several cloud storage services. The service integrates
easily with OAuth2 authentication systems as well as easing local
account management by allowing Globus users to share and delegate access
rights to other Globus users, eliminating the need for local accounts to
be created on the storage systems.

Pros:

-  Easy to set up and maintain

-  Large number of academic and government research institutions already
   use Globus Online

-  Reduces local account management requirements

-  Data transfers are highly reliable

-  Interrupted file transfers can be restarted at the last known safe
   data offset

-  Simple programmatic APIs greatly reduce development and integration

-  LSST could leverage NCSA’s subscription

Cons:

-  Not truly open source (access to source code requires a paid
   subscription)

-  Free service level available to education and non-profit
   organizations is limited

-  Some European agencies have an aversion to Globus’ because of their
   move away from open source

-  Recent versions of GCS5 (Globus Connect Server) have reduced
   capabilities (one DTN/endpoint)

iRODS
-----

The iRODS software system is designed to create federation of
distributed data from heterogeneous pools of storage. It provides a
single unified namespace from specified data sources through an internal
metadata system. iRODS provides data management functions such as
replication, search/discoverability, and rule based automation. It has
many similarities to RUCIO. iRODS is open source and free but support
may be contracted through the iRODS Consortium or commercial versions .

Pros:

-  Open source software

-  Reasonably large user community

-  Support for most common operating systems

-  Many access tools already available and multiple language APIs

-  Highly configurable

-  Built-in data transfer service

Cons:

-  Complex to install, configure and maintain

-  Requires its own database for metadata

-  Automation requires manual application of file metadata for rules to
   function

-  May have metadata performance and scalability issues

-  May require additional server resource for meta-data database
   (load-balancing, high-availability, etc.)

RUCIO
-----

RUCIO is a data management software system developed to support the
needs of the ATLAS project . It has a proven track record on projects in
the high energy physics community and is now seeing adoption in other
communities, for example, Xenon1T and LSST. RUCIO allows the creation of
a federated view of heterogeneous and distributed pools of data. It
provides a rich set of services to manage, replicate and distribute data
sets with flexible name space organization. This set of slides is a very
good introduction:
https://indico.fnal.gov/event/16010/contribution/2/material/slides/0.pdf

Pros:

-  A proven system at large scale operations

-  LSST is already using RUCIO in its data backbone service and has
   current experience with the software

Cons:

-  May require integration with a third-party transfer tool, such as
   Globus, for external customers

-  May require external customers to join LSST’s namespace if there is
   no other transfer tool desired

xRootD
------

The xRootD software is intended to provide data access in a
high-performance, fault tolerant framework. It is similar in function to
Globus GridFTP in that is not a data management orchestration system. It
focuses on moving data between systems across networks. It is open
source and primarily distributed via GitHub
(https://github.com/xrootd/xrootd). xRootD is used for data transfer by
other management software, such as dCache and RUCIO.

Pros:

-  Has been in wide use in the HEP community since about 2015

-  Open source

-  Is already integrated with dCache and RUCIO

Cons:

-  Not widely used outside the HEP community

-  Is less feature rich than Globus

-  Limited OS support

-  Is only source distributed

Zettar zx
---------

There is little detail about this relatively new data transfer tool. It
appears to be sourced from Zettar (https://www.zettar.com/), which
claims to be a revenue-supported startup in Palo Alto. They have been
awarded grants from NSF and DOE, and have conducted research and testing
in cooperation with ESNet. So, while this appears to be a commercial
product, it bears watching and a better understanding of the license
rights that might be available. A white paper on some of that work is at
Intel’s web site at:
https://www.intel.com/content/dam/www/public/us/en/documents/white-papers/dependable-hyperscale-data-transfers-with-intel-ssd-dc-white-paper.pdf

Summary/Recommendations
=======================

Of the available tools for strict data transfer, Globus Online is the
clear winner. It provides robust data transfer capabilities, is easy to
deploy, and has a rich set of user data management functions. It is also
widely deployed in the educational and national research communities.
For a more comprehensive data management solution, RUCIO seems to be the
best candidate for LSST. It is already in use by the project so little
extra effort would be required to extend it into this data distribution
domain. Coupling RUCIO, for the data curation, with Globus Online, for
external distribution, would provide the most comprehensive solution
based on the currently understood requirements.

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
