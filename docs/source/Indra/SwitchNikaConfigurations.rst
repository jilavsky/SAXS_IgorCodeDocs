.. _indra-switch-nika-configurations:
.. _switch_nika_configurations:

.. index::
    Indra; Switch Nika configurations

Switch Nika Configurations
==========================

To reduce both SAXS and WAXS data within a single Igor experiment, two
separate Nika configurations are required. However, only one configuration
(SAXS or WAXS) can be active at any given time. Use Nika's Configuration
Manager tool to switch between them:

.. Figure:: media/ConfManagerMenu.png
   :align: center
   :width: 280px

Select this menu item to open the Configuration Manager panel:

.. Figure:: media/ConfigurationManager1.jpg
   :align: center
   :width: 380px

Select "*Create New Configuration*". Nika will ask whether to save the current
configuration — if you plan to return to SAXS reduction later, save it and
name it "SAXS".

.. Figure:: media/ConfigurationManager2.jpg
   :align: left
   :width: 380px
   :figwidth: 820px

.. Figure:: media/ConfigurationManager3.jpg
   :align: left
   :width: 380px
   :figwidth: 820px

Nika restarts with a new, unconfigured instance. If the 9ID configuration
panel was open, it will be reopened automatically.

Configure and reduce WAXS data as needed. When finished, save the WAXS
configuration.

To switch back to any saved configuration, select it from the dropdown menu.
In the example below, both SAXS and WAXS configurations have been saved and
are available for selection:

.. Figure:: media/ConfigurationManager4.jpg
   :align: left
   :width: 380px
   :figwidth: 820px

Nika will ask whether to save the current configuration before switching. You
can overwrite an existing configuration or save under a new name.

.. warning::

   Configurations consume significant space in Igor experiment files. Avoid
   saving more configurations than necessary, as files can become excessively
   large.

Next step: :ref:`reduce WAXS data <reduce_WAXS_data_procedure>`.
