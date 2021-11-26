.. _api_get_software_list:

=================
GET software/list
=================

URL :: 

  http://127.0.0.1:8000/api/software/list

Description
===========

  *Under construction*

Request parameters
==================

  *Under construction*

Request response
================

  *Under construction*
  
Example
=======

  Request : ::

    curl -H "Content-Type: application/x-www-form-urlencoded" http://127.0.0.1:8000/api/software/list/

  Response : ::

    [

      {
        "name": "sw_example",
        "info": {
            "sw": {
                "desc": "software example (addition, in python code)",
                "shortname":"sw_example",
                "urls": {
                    "language": "https://www.python.org",
                    "sw_example documentation":"http://127.0.0.1/softwares/sw_example/home.html"
                },
                "keywords": [
                    "addition",
                    "python",
                    "example"
                ]
            }
        }
      },

      ...

      {
        "name": ...,
        "info": { ... }
      }
    ]

