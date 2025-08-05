##################################
Shutter Timing and Motion Profiles
##################################

.. abstract::

   A description of how the shutter timing and motion profiles are calculated, along with the corresponding events that are recording in the raw data FITS headers.

Shutter Hardware/Firmware/Software Overview
===========================================

Shutter Timing Diagram
======================

.. image:: figs/shutter-timing.png
   :alt: Shutter timing diagram
   :align: center
   :width: 100%

This diagram assumes a single snap visit. (Multi snap visits have not been tested, and should be assumed to be non functional.) The timing of the shutter blade motions,
including the time between them (the exposure time), are controlled by the shutter PLC.
The timing of the readout is under control of the DAQ/sequencer.
All other timings are under the control of the CCS software.

The following events appear along the time axis

.. list-table:: Shutter timing events
   :widths: 20 30 50
   :header-rows: 1

   * - Event
     - Type
     - Description
   * - takeImages
     - command
     - The takeImages command is received from the observatory
   * - CLEARING
     - stateChange
     - The state change event issued by the camera to indicate it has started clearing the CCDs.
   * - INTEGRATING
     - stateChange
     - The state change event issued by the camera to indicate the CCDs have started integrating.
   * - OPENING
     - stateChange
     - The shutter has started to open
   * - OPEN
     - stateChange
     - The shutter is fully open
   * - ShutterMotionProfileOpen
     - LFA
     - The shutter motion profile event for the shutter opening is published (and sent to the Large File Annex (LFA))
   * - CLOSING
     - stateChange
     - The shutter has started to close
   * - CLOSED
     - stateChange
     - The shutter is fully closed
   * - READING_OUT
     - stateChange
     - The CCDs have started reading out
   * -  ShutterMotionProfileClose
     - LFA
     - The shutter motion profile event for the shutter closing is published (and sent to the Large File Annex (LFA))
   * - takeImages done
     - command
     - the takeImages command complete
   * - QUIESCENT
     - stateChange
     - The readout is complete



Shutter Related FITS headers
============================

The following headers related to the shutter are published as part of the camera raw FITS files.


.. list-table:: Shutter related FITS headers
   :widths: 20 80
   :header-rows: 1

   * - Header
     - Description
   * - DATE_BEG/MJD-BEG
     - The TAI timestamp of the INTEGRATING state change event
   * - DATE-END/MJD-END
     - The TAI timestamp of the READING_OUT state change event
   * - DATE-TRG/MJD-TRG
     - The TAI timestamp of the beginning of readout (as reported by the DAQ)
   * - SHUTTIME
     - The measured shutter open time (in seconds) (see `shuttime`_)
   * - EXPOSE
     - The requested exposure time (in seconds) from the takeImages command
   * - DARKTIME
     - The time difference (in seconds) between the INTEGRATING state change event and the READING_OUT state change event

Shutter Motion Profile
======================

For every shutter motion (open or close) the camera publishes a ShutterMotionProfile file.
This file is a json file sent to the summit Large File Annex (LFA).
The file consists of the following major sections.

1. A header which includes, the OBSID, the motion start time, which blade is moving (PLUSX or MINUSX), the beginning and end position, and whether this is an open or close motion.
2. Raw position/time measurements of the leading edge of the shutter blade, derived from the motor encoder (encodeSamples).
3. Raw position/time measurements of the leading edge of the shutter blade, derived from the hall sensors and magnets on the shutter blades (hallTransitions).
   Each measurement in this set also includes the hall sensor triggered (each sensor will be triggered multiple times during a single motion since there are multiple magnets on each shutter blade), and whether the sensor switched on or off.
4. The result of fits to each set of raw measurements (fitResults)

Selected lines from a typical json file is shown below.

.. rst-class:: technote-wide-content

.. literalinclude:: files/MC_O_20250603_000104_shutterMotionProfileOpen.json
   :language: json
   :linenos:

.. _shuttime:

Measured Shutter Time (SHUTTIME)
================================

The measured shutter time (SHUTTIME) written to the FITS header is derived from the fits to the hall sensors for the cossesponding open and close motion profiles.

Related Documents
=================

* `CTN-002 <https://ctn-002.lsst.io/>`_ Camera Shutter Motion Analysis
* `CTN-003 <https://ctn-003.lsst.io/>`_ Configuring the PTP setting on the MOXA network switch
* `CTN-004 <https://ctn-004.lsst.io/>`_ Rubin Observatory Raw Data File Format
* `LCA-16765-F <https://docs.google.com/document/d/13UD9avhirOHS5riFfHv0uwBHQeEatxwsPUEW7Bqp2TE/edit?usp=sharing>`_ Manual for CCS Shutter System
* `LCA-00056-E <https://docushare.lsst.org/docushare/dsweb/Get/LCA-56>`_ Shutter Specification
* `Shutter MRR <https://confluence.slac.stanford.edu/spaces/LSSTCAMREV/pages/241346320/Shutter+Final+MRR+January+16+2019?preview=/241346320/243090047/Shutter%20MRR%20-%20Electronics%20and%20Controls.pptx>`_ Electronics and Motion Control

See the `Documenteer documentation <https://documenteer.lsst.io/technotes/index.html>`_ for tips on how to write and configure your new technote.
