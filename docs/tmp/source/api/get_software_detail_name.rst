.. _api_get_software_detail_name:

==========================
GET software/detail/{name}
==========================

URL :: 

  http://127.0.0.1:8000/api/software/detail/{name}

Description
===========

  *Under construction*

Request parameters
==================

.. _p_gsdn_name:
 
Parameter name (mandatory)
--------------------------

  - Description : software name
  - Type : string
  - Note : to list all available softwares, use :ref:`api_get_software_list`

Request response
================

  *Under construction*
  
Example
=======

With *name* = sw_example

  Request : ::

    curl -H "Content-Type: application/x-www-form-urlencoded" http://127.0.0.1:8000/api/software/detail/sw_example/

  Response : ::

    {
        "name": "sw_example",
        "info": {
            "sw": {
                "desc": "software example (addition, in python code)",
                "shortname": "sw_example",
                "urls": {
                    "language": "https://www.python.org",
                    "sw_example documentation": "http://127.0.0.1/softwares/sw_example/home.html"
                },
                "keywords": [
                    "addition",
                    "python",
                    "example"
                ]
            },
            "cont": {
                "name": "sw_example",
                "type": "docker",
                "image": "sw_example-docker:latest",
                "desc": "container for software example (addition, in python code)"
            }
        }
    }

