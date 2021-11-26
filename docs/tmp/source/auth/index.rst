.. _auth:

==============
Authentication
==============

Overview
========

  You can freely browse ws softwares.
  However if you want to **run** them, **authentication** will be required.

  Authentication is managed by **JWT** (**JSON Web Token**).

  First of all, you must be **identified** as a ws user. That will allow you
  to ask for a JWT value.


User
----

  For ws web services, a **user** is an identifier that can be shared by
  several persons. It is not systematically dedicated to a single person.

  Each ws user can have access to some softwares, with some rights depending
  on a :ref:`level <p_access_level>` value (assigned to his access).

  You can ask ws for a new user, or get an existing shared user.

  A ws user is defined by a *username* and its *password*.

JWT
---

  Authentication is managed by **JWT** (**JSON Web Token**).

  **In practise** to ask for running a software, you must identify yourself by
  giving an available JWT value as *jwt* parameter of the request.

  So you must previously have acquired an available JWT value.

  A JWT value is available only for a limited duration, after which you will
  have to ask for a new one.

.. include:: ../api/ui_token_right.rst

API
===

  - :ref:`JWT<api_jwt>` (get, verify)

  - :ref:`Access relation<api_access>` (between users, softwares...)

A JWT is required for :
:ref:`api_post_software_run_name` | :ref:`api_post_software_muse_run_name` 

