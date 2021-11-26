.. _api_get_software_muse_content_name_key:

======================================
GET software/muse/content/{name}/{key}
======================================

URL :: 

  http://127.0.0.1:8000/api/software/muse/content/{name}/{key}

Description
===========

Returns cluster job information as file : the whole run folder (.zip).

This request follows a :ref:`api_post_software_muse_run_name` request and its 
response containing *key* value.

This request allows to investigate if problems.

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

    run_path folder that have been zipped (as is).

Example
=======

After this :ref:`api_post_software_muse_run_name` request to ask for run :

  With *name* = sw_example

    Request : ::

      curl -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 5", "returned_type":"stdout", "todownload":"no"}' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/58ae6cea-df31-4059-9466-b817c337985d/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/58ae6cea-df31-4059-9466-b817c337985d/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=58ae6cea-df31-4059-9466-b817c337985d in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/58ae6cea-df31-4059-9466-b817c337985d/"
      }

One can do this :ref:`api_get_software_muse_content_name_key` request to
ask for cluster job information as .zip (the whole run folder), for example
to investigate if problems :

    Request : ::

      curl --output MYRETURNEDFILE2_MUSE.zip http://127.0.0.1:8000/api/software/muse/content/sw_example/58ae6cea-df31-4059-9466-b817c337985d/

    Response :
    :download:`MYRETURNEDFILE2_MUSE.zip<files/MYRETURNEDFILE2_MUSE.zip>`

