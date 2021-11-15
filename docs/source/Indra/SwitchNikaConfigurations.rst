.. _switch_nika_configurations:


.. index::
    Switch Nika Configurations

Switch Nika Configurations
--------------------------

In order to be able to reduce both SAXS and WAXS data in one Igor experiment we need to have two Nika configurations available. However, only one Nika configuration (for SAXS or WAXS) can be available at any given time. To do this we will use Nika's Configuration manager tool :

.. Figure:: media/ConfManagerMenu.png
        :align: center
        :width: 280px

Select this menu item and you will get panel:

.. Figure:: media/ConfigurationManager1.jpg
        :align: center
        :width: 380px

Now we can select "Create New Configuration". Nika will ask, if we want to save current configuration - and if you want to reduce again SAXS data, you should do so. You need to give the existing (SAXS) configuration a name - I suggest SAXS.

.. Figure:: media/ConfigurationManager2.jpg
        :align: left
        :width: 380px
        :Figwidth: 820px

.. Figure:: media/ConfigurationManager3.jpg
        :align: left
        :width: 380px
        :Figwidth: 820px

And Nika will be restarted with new, unconfigured Nika. If 9ID configuration panel is opened, it will be reopened again.

Next step is to configure and reduce WAXS data. After you are done with WAXS data you can save the WAXS configuration.

You can return to any saved configuration by selecting it in the pull down menu, in the example below I have saved SAXS and WAXS configurations and can switch Nika between them:

.. Figure:: media/ConfigurationManager4.jpg
        :align: left
        :width: 380px
        :Figwidth: 820px

Nika will ask if you want to save the current configuration (you can give it new name or overwrite any existing one). Keep track of these configurations and keep it simple...

**NOTE: Configurations take a lot of space in Igor experiments. Do not have too many Nika configurations saved as files may get excessively large.**

Next step is to :ref:`reduce WAXS <reduce_WAXS_data_procedure>`.
