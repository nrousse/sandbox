.. _api_get_software_muse_run_name_key:

===================================
GET software/muse/run/{name}/{key}
===================================

URL :: 

  http://127.0.0.1:8000/api/software/muse/run/{name}/{key}

Description
===========

Returns the run results when ready, according to parameters of the initial
:ref:`api_post_software_muse_run_name` request.

This request follows :
  - a :ref:`api_post_software_muse_run_name` request and its response
    containing *key* value.
  - a :ref:`api_get_software_muse_state_name_key` request and its response
    containing *cr* valuing 'OK' (into *report*).

This request allows to get the run results, after having previously verified
*cr* value into the response of the :ref:`api_get_software_muse_state_name_key`
request.

Request available only if *cr* is 'OK' into *report* of a previous
:ref:`api_get_software_muse_state_name_key` request.

This request is part of (:download:`a sequence <files/POST_muse_run.pdf>` to
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

  If *cr* is 'OK' into *report* of the response of the previous 
  :ref:`api_get_software_muse_state_name_key` request : ::

    Results according to parameters of the initial
    :ref:`api_post_software_muse_run_name_key` request.

  Else (*cr* is 'NOK' or 'WAIT', errors...) : ::

    {
      "error": error text, "more":[...], "type":error type
    }

Examples
========

Into the :ref:`api_post_software_muse_run_name` examples, see
the **GET software/muse/run/{name}/{key}** requests (to get the results).

