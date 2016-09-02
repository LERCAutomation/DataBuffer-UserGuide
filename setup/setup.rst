*******************
Setting up the tool
*******************

Before the Data Buffer tool will function, it needs to be installed and configured. It is recommended that the configuration is carried out first.

.. index::
	single: Configuring the tool

Configuring the tool
====================

The configuration is stored in an XML file called 'DataBuffer.xml', an example of which can be found in the :doc:`Appendix <../appendix/appendix>`. Attributes and settings are presented as nodes (beginning with a start node, e.g. ``<example>``, and finishing with an end note, e.g. ``<\example>``), with the value for the setting held between the ``<value>`` and ``<\value>`` tag. 

.. caution:: 
	The name of the configuration file must be 'DataBuffer.xml'. The tool will not load if a different name is used.

The XML file can be edited in a text editor such as Notepad or Wordpad, or using a more feature rich XML editor such as as `Sublime Text <https://www.sublimetext.com/3>`_. The configuration file is split into three sections:

_`General attributes`
	General and default attributes for the tool.

_`Input Tables`
	Deals with how each input GIS layer should be handled.

_`Output Table`
	Deals with how the new output GIS layer should be created.

.. caution::
	It is important that the structure of the file is maintained as it is presented in the :doc:`Appendix <../appendix/appendix>`. Any changes to the structure may result in the Data Buffer tool not loading, or not working as expected.

Once editing has been completed and the edits have been saved, it is recommended that the configuration file is opened using an internet browser such as Internet Explorer which will help highlight any editing errors – only if the structure of the file is valid will the whole file be displayed in the internet browser.

.. note::
	It is recommended that the configuration file is kept in a central (network) location, so that all users use the same configuration. Additionally, it is essential that the configuration file is kept in the same folder as the compiled version of the tool.


.. index::
	single: Special characters in XML

.. raw:: latex

   \newpage

Special characters in XML
-------------------------

The characters ``&``, ``<`` and ``>`` are not valid within values and, so in order to be used, must be **escaped** with XML entities as follows:

<
	This must be escaped with ``&lt;`` entity, since it is assumed to be the beginning of a tag. For example, ``RecYear &lt; 2010``

>
	This should be escaped with ``&gt;`` entity. It is not mandatory -- it depends on the context -- but it is strongly advised to escape it. For example, ``RecYear &gt; 1980``

&
	This must be escaped with ``&amp;`` entity, since it is assumed to be the beginning of a entity reference. For example, ``TaxonGroup = 'Invertebrates - Dragonflies &amp; Damselflies'``


.. index::
	single: General attributes

General attributes
------------------

The first section of the configuration file deals with a series of general attributes for the Data Buffer tool. Each node specifies where files will be saved, where the log file will be saved as well as other overall settings. Details on these attributes (and their typical values where known) are outlined below. The list follows the order within which the attributes are found in the configuration file. This version of the configuration details is valid for the MapInfo version 1.0.7 of the Data Buffer tool.

_`ToolTitle`
	The title to use for the program in the MapInfo Tools menu.

_`LogFilePath` 	
	The folder to be used for storing log files. This folder must already exist.

_`DefaultPath`
	The default folder where output GIS layers will be stored. This can be overridden by the user when executing the tool.


.. index::
	single: Input table attributes

Input table attributes
----------------------

.. _InTables:

The details of all the input layers that can be included in the process are found within the ``<InTables>`` node. For each GIS layer to be included in the process a new child node must be created. The node name (e.g. ``<Badgers>``) is a user-defined name used to identify an individual layer - it must be unique. This name is name of the layer as it will be shown in the tool interface, and can be different from the layer name as it is known in the active MapInfo workspace (which will be set in a subsequent child node). A simple example of a map layer definition with limited attributes is shown in :numref:`figXMLExampleMapInfo`.

.. tip::
	If you wish to display spaces in any layer names in the tool menu use an underscore (``_``) for each space in the node name for the layer. XML does not allow spaces in node names, but the tool will translate these underscores into spaces when the form is opened.

.. _figXMLExampleMapInfo:

.. figure:: figures/InTableXMLExample.png
	:align: center

	Simplified example of input layer attributes configuration (MapInfo)

The attributes that are required for each input table are as follows:

TableName
	The name of the layer as it is known in the active workspace.

Columns
	A comma-separated list of columns that should be included in the data selected from this layer during the process. The column names (not case sensitive) should match the column names in the source table.

WhereClause
	Selection criteria that should be used to select records from this layer. This clause could, for example, ensure records are only included that have been entered after a certain date, are verified, are presence (not absence) records, or are a subset for a particular species. Leave this entry blank to select all records from the input layer.

	.. note::
		Any clause specified here must adhere to MapInfo SQL syntax as the clause will be run within MapInfo.

SortOrder
	A comma-separated list of columns indicating the order the data should be selected from this layer. The column names (not case sensitive) should match the column names in the source table.

	.. note::
		The order of the records may be important when it comes to identifying records with the same ``key`` attributes (e.g. species name(s), grid reference, location name). Hence it is recommended that the key attribute columns are specified in the sort order.

BufferSize
	The size of the buffer (in metres) to apply to records before being added to the output layer. A value of 0 (zero) indicates that the records will not be buffered for this input layer.

DissolveSize
	The proximity (in metres) of records that are to be dissolved together. Records within this distance of each other will be dissolved together when output to form a single contiguous area. A value of 0 (zero) indicates that the records should not be dissolved.

	.. note::
		Even if records are not dissolved (either because they are not within the specified distance of each other or because the value is 0) they may be combined together if their ``key`` attributes are the same.


.. index::
	single: Output table attributes

Output table attributes
-----------------------

.. _OutTable:

The details of the output layer to be created are found within the ``<OutTable>`` node and are specified as follows:

_`ColumnDefs`
	A comma-delimited list of the column headings, and their data types/lengths, that the output GIS layer should have.

_`CoordinateSystem`
	The coordinate system for the output GIS layer.

Columns
	This section defines how the input layer records are treated when buffering, combining and dissolving them for the output layer. It should consist of a set of child nodes, one for each column listed in the `ColumnDefs`_ node. The node names are not important but must be unique. Each child node has the following entries:

	.. caution::
		The order of the columns in the input layers must match the order of the columns specified here as well as the order of the columns listed in the `ColumnDefs`_ node.

	ColumnName
		The name of the input column in **all** of the input layers.

	ColumnType
		The type of column (and how it should be processed) for the output layer. The options are:

			Key
				Indicates that the column is a ``Key`` column. Only records with the same values for **all** key columns will be combined or dissolved. Values in the column will be written 'as is' to the output layer.
			Cluster
				If records are to be clustered for the input layer (i.e. ``DissolveSize > 0``) then the most common value in this column will written to the output layer. Otherwise values in the column will be written 'as is' to the output layer.
			First
				The **first** value in this column, for records with the same key columns, will be written to the output layer. This is typically used when **all** values with the same key columns are the same (e.g. the common name when the scientific name is used as a key column).
			Common
				The most common value in this column, for records with the same key columns, will be written to the output layer. This is useful when values may vary for the same key column values (e.g. the location name when the grid reference is used as a key column).
			Min
				The minimum value in this column, for records with the same key columns, will be written to the output layer separated by `` - ``. This is useful for numeric columns such as abundance counts or recorded years.
			Max
				The maximum value in this column, for records with the same key columns, will be written to the output layer separated by `` - ``. This is useful for numeric columns such as abundance counts or recorded years.
			Range
				The range of values in this column, for records with the same key columns, are written to the output layer separated by `` - ``. This is useful for numeric columns such as abundance counts or recorded years (e.g. ``1986 - 1988``).


Symbology
	The symbology definition for the output layer. Multiple symbols can be specified for use in the symbology using clauses. Each symbol is specified between ``<Symbol>`` and ``</Symbol>`` tags and is defined by the following child nodes:

	Clause
		The clause that defines the records which will be assigned this symbol. This can be left blank to apply the symbology to all records with the same ``<Object>`` type specified below.
	Object
		The object type that is symbolised using this symbol (e.g. ``Region``). All buffered objects will be 'Region' whereas non-buffered objects could be 'Point', 'Line' or 'Region'.
	Symbol
		The style to be used for the symbol. This attribute only applies to ``Point`` objects.
	Pen
		The style to be used for the symbol border (outline). This attribute applies to ``Region`` objects.
	Brush
		The style to be used for the symbol infill. This attribute applies to ``Region`` objects.

	.. tip::
		In order to find the syntax for the Pen and Brush attribute, set the desired symbol for a polygon (region) layer through **Options => Region style**, then write the following statement in the MapBasic window and hit enter: ``Print CurrentBorderPen()``. The printed pen definition (e.g. ``2,2,10526880``) can be used in the ``Pen`` attribute.  Repeat with ``Print CurrentBrush()``.


.. raw:: latex

   \newpage

.. index::
	single: Installing the tool

Installing the tool
===================

To install the tool, make sure that the configuration of the XML file as described above is complete, that the XML file is in the same directory as the tool MapBasic application (.MBX) and that all required GIS layers are loaded in the current workspace. Then, open `Tool Manager` in MapInfo by selecting :kbd:`Tools --> Tool Manager...` in the menu bar (:numref:`figToolManager`). 

.. _figToolManager:

.. figure:: figures/ToolManager.png
	:align: center

	The Tool Manager in MapInfo 12 or earlier

.. raw:: latex

   \newpage

In the `Tool Manager` dialog, click **Add Tool...**, then locate the tool using the browse button **...** on the `Add Tool` dialog (:numref:`figAddTool`). Enter a name in the **Title** box (e.g. 'DataBuffer'), and a description if desired. Then click **Ok** to close the `Add Tool` dialog.

.. _figAddTool:

.. figure:: figures/AddToolDialog.png
	:align: center

	Adding a tool in Tool Manager

.. raw:: latex

   \newpage

The tool will now show in the `Tool Manager` dialog (:numref:`figToolAdded`) and the **Loaded** box will be checked. To load the tool automatically whenever MapInfo is started check the **AutoLoad** box.  Then click **Ok** to close the `Tool Manager` dialog.

.. _figToolAdded:

.. figure:: figures/DataBufferLoaded.png
	:align: center

	The Data Buffer tool is loaded

The tool will now appear as a new entry in the `Tools` menu (:numref:`figToolMenu`).

.. _figToolMenu:

.. figure:: figures/DataBufferToolMenu.png
	:align: center

	The Data Buffer tool menu

.. note::
	The name that will appear in the `Tools` menu is dependent on the `ToolTitle`_ value in the configuration file, **not** the name given when adding the tool using the Tool Manager.

.. tip::
	It is recommended that a MapInfo Workspace is created that contains all the required GIS layers to run the tool. Once this workspace has been set up and the tool has been configured and installed, running the Data Buffer tool becomes a simple process.
