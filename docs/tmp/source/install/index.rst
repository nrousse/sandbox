.. _install:

============
Installation
============

  - :ref:`install_general`

  - Installation procedure :
    :ref:`in development case <install_dev>`
    | :ref:`in production case <install_prod>`

  - See also : :ref:`Softwares installation <install_softwares>`

.. _install_general:

General
=======

During development, the ws web services can be installed on the development server provided by :term:`django`. Then once developed, it can be deployed on a production server in order to be used by web users. This implies a production environment, in addition to the tools and libraries already used for development.

Source software
---------------

Once the installation finished, the ws resulting hierarchy is such as :

    .. literalinclude:: hierarchy_source.txt

Development environment
-----------------------

The ws software is developed with :

    - python language (python 3.7 version),
    - django framework,
    - some django applications, like for example django-rest-framework. 
    - SQLite for databases.
    - Sphinx for documentation.

    See the requirement.txt file :

    .. literalinclude:: ../../../ws/install/requirement.txt

Additional production environment
---------------------------------

In our case, the software has been deployed on a virtual machine with **Debian 10** (**Buster**), **Apache2** server and **mod_wsgi**.

.. _install_procedure:

Installation procedure
======================

  - :ref:`install_dev`

  - :ref:`install_prod`

  Once installed, the ws web services allow to run the softwares that have
  been delivered and installed as required by ws :
  see :ref:`Softwares installation <install_softwares>`.

