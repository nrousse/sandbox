.. _api_jwt:

====================
JWT (JSON Web Token)
====================

  :ref:`auth` *is required to run softwares.*

.. include:: ui_token_right.rst

.. _api_post_jwt_obtain:

POST jwt/obtain
===============

URL ::

  http://127.0.0.1:8000/api/jwt/obtain

Description
-----------

  To get a JWT (JSON Web Token) value.

Request parameters
------------------

  - *username* of the ws user (mandatory)
  - *password* of the ws user (mandatory)

Request response
----------------

  JWT value

Example
-------

  *Under construction*

.. _api_post_jwt_verify:

POST jwt/verify
===============

URL ::

  http://127.0.0.1:8000/api/jwt/verify

Description
-----------

  To verify a JWT (JSON Web Token) value.

Request parameters
------------------

  - *jwt* : value of the JWT to be verified (mandatory)

Request response
----------------

  Says if the JWT is available or not.

Example
-------

  *Under construction*

