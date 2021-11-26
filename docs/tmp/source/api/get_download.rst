.. _api_get_download:

============
GET download
============

URL :: 

  http://127.0.0.1:8000/api/download/

Description
===========

  *Under construction*

Request parameters
==================

.. _p_gd_key:
 
Parameter key (mandatory)
-------------------------

  - Description : ...
  - Type : ...

Request response
================

  *Under construction*

Example
=======

With *key* = 20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91

  Request : ::

    curl --output MYRETURNEDFILE1.zip http://127.0.0.1:8000/api/download/?key=20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91

    Or from web browser, enter the URL : http://127.0.0.1:8000/api/download/?key=20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91

  Response :
  :download:`MYRETURNEDFILE1.zip<files/MYRETURNEDFILE1.zip>`

