.. image:: https://img.shields.io/badge/ctn--005-lsst.io-brightgreen.svg
   :target: https://ctn-005.lsst.io
.. image:: https://github.com/lsst/ctn-005/workflows/CI/badge.svg
   :target: https://github.com/lsst/ctn-005/actions/

##################################
Shutter Timing and Motion Profiles
##################################

CTN-005
=======

A description of how the shutter timing and motion profiles are calculated, along with the corresponding events that are recording in the raw data FITS headers.

**Links:**

- Publication URL: https://ctn-005.lsst.io
- Alternative editions: https://ctn-005.lsst.io/v
- GitHub repository: https://github.com/lsst/ctn-005
- Build system: https://github.com/lsst/ctn-005/actions/


Build this technical note
=========================

You can clone this repository and build the technote locally if your system has Python 3.11 or later:

.. code-block:: bash

   git clone https://github.com/lsst/ctn-005
   cd ctn-005
   make init
   make html

Repeat the ``make html`` command to rebuild the technote after making changes.
If you need to delete any intermediate files for a clean build, run ``make clean``.

The built technote is located at ``_build/html/index.html``.

Publishing changes to the web
=============================

This technote is published to https://ctn-005.lsst.io whenever you push changes to the ``main`` branch on GitHub.
When you push changes to a another branch, a preview of the technote is published to https://ctn-005.lsst.io/v.

Editing this technical note
===========================

The main content of this technote is in ``index.rst`` (a reStructuredText file).
Metadata and configuration is in the ``technote.toml`` file.
For guidance on creating content and information about specifying metadata and configuration, see the Documenteer documentation: https://documenteer.lsst.io/technotes.
