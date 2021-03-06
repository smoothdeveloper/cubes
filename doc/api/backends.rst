*************************************************
:mod:`backends` --- Aggregation Browsing Backends
*************************************************

.. module:: backends
   :synopsis: backends for browsing aggregates of various data sources

SQL - Default
=============

The standard SQL backend provides full-featured aggregation browser of 
denormalized data source. The helper class ``SQLDenormalizer`` creates view or 
a table from a star or snowflake schema. The created view is then passed to the 
borwser for slicing and dicing.


.. autoclass:: cubes.backends.sql.SQLDenormalizer
.. autoclass:: cubes.backends.sql.SQLBrowser

SQL - Star
==========

.. warning::

	Implementation of this backend is not yet finished.

.. autoclass:: cubes.backends.sql.StarBrowser

Slicer
======

This backend is just for backend development demonstration purposes.

.. autoclass:: cubes.backends.slicer.SlicerBrowser

Implementing Custom Backend
===========================

Custom backend is just a subclass of :class:`cubes.AggregationBrowser` class.

Slicer
------

If the backend is intended to be used by the Slicer server, then backend should 
be placed in its own module. The module should contain a method 
`create_workspace(model, config)` which returns a workspace object. `config` is 
a configuration dictionary taken from the config file (see below). The 
workspace object should implement `browser_for_cube(cube, locale)` which 
returns an :class:`AggregationBrowser` subclass.

The `create_workspace()` can be compared to `create_engine()` in a database 
abstraction framework and the `browser_for_cube()` can be compared to 
`engine.connect()` to get a connection from a pool.

The configuration for `create_workspace()` comes from slicer ``.ini`` 
configuration file in section ``[backend]`` and is provided as ``dict`` object.

.. note::

    You can override the section in the Slicer configuration file by providing 
    ``config_section`` variable in your backend module. This is however not 
    recommended. Currently it is used only by default SQL backend to maintain 
    backward compatibility with older versions of cubes where the configuration 
    for database backend was under ``[db]``.
