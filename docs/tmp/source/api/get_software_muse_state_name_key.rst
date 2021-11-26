.. _api_get_software_muse_state_name_key:

====================================
GET software/muse/state/{name}/{key}
====================================

URL :: 

  http://127.0.0.1:8000/api/software/muse/state/{name}/{key}

Description
===========

Returns cluster job information (IN_PROGRESS, ENDED_OK, ENDED_NOK...) as text,
during cluster process.

This request follows a :ref:`api_post_software_muse_run_name` request and its 
response containing *key* value.

This request allows to know when the run results are ready and can be asked
(by :ref:`api_get_software_muse_run_name_key` request) : see condition on *cr* 
into the "Request response" below.

This request is part of :download:`a sequence<files/POST_muse_run.pdf>` to
run a software under muse cluster and get the run result, implying requests :
:ref:`api_post_software_muse_run_name` |
:ref:`api_get_software_muse_state_name_key` |
:ref:`api_get_software_muse_run_name_key` |
:ref:`api_get_software_muse_content_name_key`.

Request parameters
==================

Parameter name (mandatory)
--------------------------

  - Description : software name
  - Type : string

Parameter key (mandatory)
-------------------------

  - Description : key value
  - Type : string
  - Note : comes from the response of the initial
    :ref:`api_post_software_muse_run_name` request.

Request response
================

  In error case : ::

    {
      "error": error text, "more":[...], "type":error type
    }

  Else : ::

    {  "info": { 
           "report": {
               "cr": value "OK" or "NOK" or "WAIT"
               "name": software name,
               "request", "rsrc", "process" information
           }
       }
       "msg": text where how to go on
       "url": URL value (probable following get request ; see also msg)
    }


When *cr* is 'OK' into *report* of the response, then the run results can
be asked (by :ref:`api_get_software_muse_run_name_key` request).

Examples
========

Into the :ref:`api_post_software_muse_run_name` examples, see
the **GET software/muse/state/{name}/{key}** requests.

