.. index::
	single: FAQ
	see: Frequently asked questions; FAQ

**************************
Frequently Asked Questions
**************************

This is a list of Frequently Asked Questions about the Data Buffer tool. Feel free to suggest new entries!

General questions
=================

**How do I get a copy of the tool?**

	The latest version of the tool can be downloaded from GitHub (`MapInfo version <https://github.com/LERCAutomation/DataBuffer-MapInfo/releases>`_ or `ArcGIS version <https://github.com/LERCAutomation/DataBuffer---ArcObjects/releases>`_). Please ensure that you use the correct configuration file, an example copy of which is also included with the release.

**Can several people use the tool at the same time?**

	Any number of users can use the tool simultaneously if they have a copy of it loaded in their own copy of MapInfo or ArcGIS. The tool uses the data layers that are loaded in GIS in a read-only fashion, so there is no limit to the number of users of the tool. However, where results are written to a central (network) location, and the output is written to the same output files, conflicts may occur.

**Does the tool work with QGIS?**

	Currently only MapInfo and ArcGIS implementations of the tool exist. However, if funding was available the tool could be adapted to also support QGIS.

Operating the tool
==================

**One of the data tables I want to use isn't showing in the form. How do I get it to show up?**

	This issue can arise in several ways:

	- The table / GIS layer isn't loaded in the active workspace. In this case, a :ref:`message will pop up <figlaunchwarning>` before the form is shown telling you the layer isn't loaded. Add the layer to the workspace and the problem should be resolved.
	- The table / GIS layer isn't listed in the XML configuration document. Please refer to the :doc:`setup <../setup/setup>` section and add it as a input layer.
	- The table / GIS layer is listed in the XML configuration document, but the :ref:`TableName <intables>` is spelled incorrectly. Note that the name must follow the exact format of the name of the layer in the active workspace.


Tool issues
===========

**How do I report a new bug or propose a change in the tool?**

	Please check the existing known issues and change requests on the LERCAutomation pages on GitHub (`MapInfo <https://github.com/LERCAutomation/DataBuffer-MapInfo>`_  or `ArcGIS <https://github.com/LERCAutomation/DataBuffer---ArcObjects>`_) before reporting/proposing new issues or changes. If you have a new issue or request you can submit it there and it will be picked up by the developers. Alternatively, you can email suggestions to `Hester <mailto:Hester@HesterLyonsConsulting.co.uk>`_ or `Andy <mailto:Andy@AndyFoyConsulting.co.uk>`_. 
