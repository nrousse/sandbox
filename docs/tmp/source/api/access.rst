.. _api_access:

==================
Access information
==================

  :ref:`auth` *is required to run softwares.*

Description
===========

  Requests to know **access relations** between **users** and softwares,
  between **softwares** and users, by taking into account - or not - one
  **access level**.

  In case of a request done for a **container** (instead of a software),
  understand that an access is available for all the softwares embedded into it.

Common requests parameters
==========================

  :ref:`softwarename<p_gai_softwarename>` | :ref:`level<p_gai_access_level>`
  | :ref:`username<p_gai_username>` | :ref:`containername<p_gai_containername>`

.. _p_gai_softwarename:
 
Parameter softwarename
----------------------

  - Description : software name
  - Type : string
  - Note : to list all available softwares, use :ref:`api_get_software_list`

.. _p_gai_access_level:

Parameter level
---------------

  - Description : *level* defines the rights ...
  - Type : integer.
  - Available values :

    === ============= =====================================================
     1   (visitor)     has minimum rights, no access to some parameters.
    --- ------------- -----------------------------------------------------
     2   (user)        has access to most of parameters but maybe with
                       limited values domain.
    --- ------------- -----------------------------------------------------
     3   (superuser)   has access to most of parameters generally without
                       limitations.
    --- ------------- -----------------------------------------------------
     4   (admin)       has all rights.
    === ============= =====================================================

  - Unavailable values : returns an error 

.. _p_gai_username:
 
Parameter username
------------------

  - Description : username of the ws user 
  - Type : string

.. _p_gai_containername:
 
Parameter containername
-----------------------

  - Description : container name
  - Type : string
  - Note : to list all available containers, use :ref:`api_get_container_list`

.. _api_get_software_name_access_user_name_list:

GET software/{softwarename}/access/user/name/list
=================================================

URL ::

  http://127.0.0.1:8000/api/software/{softwarename}/access/user/name/list

Description
-----------

  To get the list of the names of users with an access (any level) to the
  software named *softwarename*.

Request parameters
------------------

  - :ref:`softwarename<p_gai_softwarename>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_software_name_access_level_user_name_list:

GET software/{softwarename}/access/{level}/user/name/list
=========================================================

URL ::

  http://127.0.0.1:8000/api/software/{softwarename}/access/{level}/user/name/list

Description
-----------

  To get the list of the names of users with an access (*level* level) to the
  software named *softwarename*.

Request parameters
------------------

  - :ref:`softwarename<p_gai_softwarename>` (mandatory)
  - :ref:`level<p_gai_access_level>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_user_name_access_software_name_list :

GET user/{username}/access/software/name/list
=============================================

URL ::

  http://127.0.0.1:8000/api/user/{username}/access/software/name/list

Description
-----------

  To get the list of the names of softwares for which the user named
  *username* has an access (any level).

Request parameters
------------------

  - :ref:`username<p_gai_username>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_user_name_access_level_software_name_list :

GET user/{username}/access/{level}/software/name/list
=====================================================

URL ::

  http://127.0.0.1:8000/api/user/{username}/access/{level}/software/name/list

Description
-----------

  To get the list of the names of softwares for which the user named
  *username* has an access (*level* level).

Request parameters
------------------

  - :ref:`username<p_gai_username>` (mandatory)
  - :ref:`level<p_gai_access_level>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_container_name_access_user_name_list:

GET container/{containername}/access/user/name/list
===================================================

URL ::

  http://127.0.0.1:8000/api/container/{containername}/access/user/name/list

Description
-----------

  To get the list of the names of users with an access (any level) to the
  container named *containername* (and so to all the softwares embedded into
  this container).

Request parameters
------------------

  - :ref:`containername<p_gai_containername>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_container_name_access_level_user_name_list:

GET container/{containername}/access/{level}/user/name/list
===========================================================

URL ::

  http://127.0.0.1:8000/api/container/{containername}/access/{level}/user/name/list

Description
-----------

  To get the list of the names of users with an access (*level* level) to
  the container named *containername* (and so to all the softwares embedded
  into this container).

Request parameters
------------------

  - :ref:`containername<p_gai_containername>` (mandatory)
  - :ref:`level<p_gai_access_level>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_user_name_access_container_name_list :

GET user/{username}/access/container/name/list
==============================================

URL ::

  http://127.0.0.1:8000/api/user/{username}/access/container/name/list

Description
-----------

  To get the list of the names of containers for which the user named
  *username* has an access (any level).

Request parameters
------------------

  - :ref:`username<p_gai_username>` (mandatory)

Request response
----------------

  The requested list

.. _api_get_user_name_access_level_container_name_list :

GET user/{username}/access/{level}/container/name/list
======================================================

URL ::

  http://127.0.0.1:8000/api/user/{username}/access/{level}/container/name/list

Description
-----------

  To get the list of the names of containers for which the user named
  *username* has an access (*level* level).

Request parameters
------------------

  - :ref:`username<p_gai_username>` (mandatory)
  - :ref:`level<p_gai_access_level>` (mandatory)

Request response
----------------

  The requested list

