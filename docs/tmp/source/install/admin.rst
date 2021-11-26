.. _admin:

==============
Administration
==============

General
=======

The following information concerns the production situation, and in part the
development situation.

You will find some admin help notes all along the installation procedure page 
(:ref:`in production case <install_prod>` |
:ref:`in development case <install_dev>`).

ws database
===========

- To access the Admin web page : :ref:`api_get_admin`

- To clean the database : see "Admin help note to clean the database" into 
  the installation procedure page (:ref:`in production case <install_prod>` |
  :ref:`in development case <install_dev>`).

Survey
======

- Control that ws is well online at :

  - ws web site     : http://147.100.179.250
  - ws web services : http://147.100.179.250/api

- Regularly verify that mount is active,
  and mount again if mount is not active anymore : see "Mount monitoring"
  into 'install_muse.txt' file of the installation procedure page
  (:ref:`in production case <install_prod>` |
  :ref:`in development case <install_dev>`).

- Apache server (production situation) :

  - Connexion to the ws production virtual machine :
    ssh nrousse@147.100.179.250

  - to re-run Apache : see "Apache run" into the
    :ref:`installation procedure page in production case <install_prod>`.

Logs
====

- There are apache log files (production situation) :
  **/var/log/apache2/error.log** and **/var/log/apache2/access.log**

- There is another log file, where the ws web services record some loggin
  information (events...) :
  **/opt/ws/fab/log/ws.log**

  *(More exactly, see LOG_FILE path value into the 
  /opt/ws/ws/apps/conf/config.py file).*

Debug
=====

*DEBUG* (production situation)
------------------------------

  *DEBUG* (of /opt/ws/ws/projects/ws/ws/settings.py file) must value 'False'.

  *DEBUG* can **temporary** be set to 'True' to get more error messages.

Adding some new softwares
=========================

To add a software :

- Declare the new software into the ws database by : :ref:`api_get_admin`

- See :ref:`Softwares installation <install_softwares>`

