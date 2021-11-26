.. _api_post_software_muse_run_name:

=============================
POST software/muse/run/{name}
=============================

URL :: 

  http://127.0.0.1:8000/api/software/muse/run/{name}

Access
======

:ref:`auth` *is required ; see also* :ref:`JWT<api_jwt>`.

.. include:: ui_token_right.rst

Description
===========

Launches on Muse cluster the *name* software, according to parameters.
Then returns information required to follow cluster process and get results
(*key* value).

**Cluster remote run**, always **as Singularity command** (if the software
container type is 'singularity' and even if the software container type is
'docker').

Sequence
--------

This request is part of :download:`a sequence<files/POST_muse_run.pdf>` from
from running a software under Muse cluster to getting the run results :

  1) :ref:`api_post_software_muse_run_name`

     Launches on Muse cluster the *name* software, according to parameters.
     Returns information required to follow cluster process and get results :
     *key* value.

  2) :ref:`api_get_software_muse_state_name_key`

     Returns cluster job information (IN_PROGRESS, ENDED_OK, ENDED_NOK...).

  If cr is 'OK' into *report* of the 
  :ref:`api_get_software_muse_state_name_key` response, then the run results
  are ready (cluster process ended), and the following request 
  :ref:`api_get_software_muse_run_name_key` can be done.
  Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
  should be done, until it (cr is 'OK') becomes the case (see also
  :ref:`api_get_software_muse_content_name_key`).

  3) :ref:`api_get_software_muse_run_name_key`

     Returns the run results, according to parameters of the initial request
     :ref:`api_post_software_muse_run_name`.

  Help while running a software under muse cluster :
    
    :ref:`api_get_software_muse_content_name_key` returns cluster job content
    (.zip), to be able to investigate if problems.

Request parameters
==================

Same as :ref:`api_post_software_run_name` request parameters

Plus - *Under construction* - :

  - sbatch_list : *Under construction* 

    - If *mail-user* is given into *sbatch_list* then, during process running,
      the user will receive notifications emails when cluster job state
      changes according to *mail-type* value ('ALL' by default, *mail-type*
      can also be given into *sbatch_list*).

  - module_list : *Under construction* 

Request response
================

  ::

    {
      "msg": text about how to go on (to get cluster job state, results)
      "url": URL value, for following get request
    }
  
Example A.
==========

  With *name* = sw_example

1) To ask for run :

    Add *jwt* parameter **!!!**

    Request :ref:`api_post_software_muse_run_name` : ::

      curl -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 5", "returned_type":"stdout", "todownload":"no"}' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/58ae6cea-df31-4059-9466-b817c337985d/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/58ae6cea-df31-4059-9466-b817c337985d/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=58ae6cea-df31-4059-9466-b817c337985d in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/58ae6cea-df31-4059-9466-b817c337985d/"

        "urls":{ ... }
      }

2) Then to know if run results are ready :

    Request :ref:`api_get_software_muse_state_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/state/sw_example/58ae6cea-df31-4059-9466-b817c337985d/

    Response : ::

      {
        "info": {

          "report": { 
              "request": {
                  "cmd":"python /add.py 4 5", "cmd_option":"",
                  "ordered_filename_list":[], "returned_type":"stdout",
                  "todownload":false,
                  "sbatch_list":[], "module_list":[]
              },
              "rsrc": { "identity":"muse"},
              process": {
                  "jobname":"114-ws-sw_example", "job_id":2459394,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"
              }, 
              name":"sw_example", 
              cr":"OK"
          }
        },
  
        "msg":"End of job. You can now get results by GET http://127.0.0.1:8000/api/software/muse/run/sw_example/58ae6cea-df31-4059-9466-b817c337985d/ request or ask for content (to investigate) by GET http://127.0.0.1:8000/api/software/muse/content/sw_example/58ae6cea-df31-4059-9466-b817c337985d/ request.",
  
        "url":"http://127.0.0.1:8000/api/software/muse/run/sw_example/58ae6cea-df31-4059-9466-b817c337985d/"
  
      }

    The run results are now ready (cluster process ended), since *cr* is 'OK'
    into *report* of response above.
    Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
    should be done, until it becomes the case (see also
    :ref:`api_get_software_muse_content_name_key`).

3) Then to get ready run results (according to parameters of initial POST
   request) :

    Request :ref:`api_get_software_muse_run_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/run/sw_example/58ae6cea-df31-4059-9466-b817c337985d/

    Response : ::

      {
        "info": {

          "stdout":"9.0",

          "report": {

              "request": {
                  "cmd":"python /add.py 4 5", "cmd_option":"",
                  "ordered_filename_list":[], "returned_type":"stdout",
                  "todownload":false,
                  "sbatch_list":[], "module_list":[]
              },

              "rsrc": { "identity":"muse" },

              "process": {
                  "jobname":"114-ws-sw_example", "job_id":2459394,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"
              },

              "name":"sw_example",
              "cr":"OK"
          }
        }
      }

Example B.
==========

  With *name* = sw_example

  With *returned_type* = 'run.zip'

1) To ask for run :

    Add *jwt* parameter **!!!**

    Request :ref:`api_post_software_muse_run_name` : ::

      curl -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 55", "returned_type":"run.zip", "todownload":"no"}' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=fc3b822f-c62f-40fe-93be-417b1bcc38e9 in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/"
      }

2) Then to know if run results are ready :

    Request :ref:`api_get_software_muse_state_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/state/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/

    Response : ::

      {
        "info": {

          "report": {
              "request": {
                  "cmd":"python /add.py 4 55", "cmd_option":"",
                  "ordered_filename_list":[], "returned_type":"run.zip",
                  "todownload":false,
                  "sbatch_list":[], "module_list":[]
              },
              "rsrc": { "identity":"muse" },
              "process": {
                  "jobname":"115-ws-sw_example", "job_id":2460887,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"
              },
              "name":"sw_example",
              "cr":"OK"
          }
        },

        "msg":"End of job. You can now get results by GET http://127.0.0.1:8000/api/software/muse/run/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/ request or ask for content (to investigate) by GET http://127.0.0.1:8000/api/software/muse/content/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/ request.",

        "url":"http://127.0.0.1:8000/api/software/muse/run/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/"

      }

    The run results are now ready (cluster process ended), since *cr* is 'OK'
    into *report* of response above.
    Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
    should be done, until it becomes the case (see also
    :ref:`api_get_software_muse_content_name_key`).

3) Then to get ready run results (according to parameters of initial POST
   request) :

    Request :ref:`api_get_software_muse_run_name_key` : ::

      curl --output MYRETURNEDFILE_MUSE.zip http://127.0.0.1:8000/api/software/muse/run/sw_example/fc3b822f-c62f-40fe-93be-417b1bcc38e9/

    Response :
    :download:`MYRETURNEDFILE_MUSE.zip<files/MYRETURNEDFILE_MUSE.zip>`

Example C.
==========

  With *name* = sw_example

  With *returned_type* = 'stdout.txt'

1) To ask for run :

    Add *jwt* parameter **!!!**

    Request :ref:`api_post_software_muse_run_name` : ::

      curl -H "Content-Type: application/x-www-form-urlencoded" -d 'cmd=python /add.py 4 55&returned_type=stdout.txt&todownload=no' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=f3d078e0-7496-4f02-b9c4-2da924a24955 in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/"
      }

2) Then to know if run results are ready :

    Request :ref:`api_get_software_muse_state_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/state/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/

    Response : ::

      {
        "info": {

          "report": {
              "request": {
                  "cmd":"python /add.py 4 55", "cmd_option":"",
                  "ordered_filename_list":[], "returned_type":"stdout.txt",
                  "todownload":false,
                  "sbatch_list":[], "module_list":[]
              },
              "rsrc": { "identity":"muse" },
              "process": {
                  "jobname":"116-ws-sw_example", "job_id":2460934,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"
              },
              "name":"sw_example",
              "cr":"OK"
          }

        },

        "msg":"End of job. You can now get results by GET http://127.0.0.1:8000/api/software/muse/run/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/ request or ask for content (to investigate) by GET http://127.0.0.1:8000/api/software/muse/content/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/ request.",

        "url":"http://127.0.0.1:8000/api/software/muse/run/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/"
      }

    The run results are now ready (cluster process ended), since *cr* is 'OK'
    into *report* of response above.
    Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
    should be done, until it becomes the case (see also
    :ref:`api_get_software_muse_content_name_key`).

3) Then to get ready run results (according to parameters of initial POST
   request) :

    Request :ref:`api_get_software_muse_run_name_key` : ::

      curl --output MYRETURNEDFILE_MUSE.txt http://127.0.0.1:8000/api/software/muse/run/sw_example/f3d078e0-7496-4f02-b9c4-2da924a24955/

    Response :
    :download:`MYRETURNEDFILE_MUSE.txt<files/MYRETURNEDFILE_MUSE.txt>`
  
Example D.
==========

  With *name* = sw_example

  With *todownload* = 'yes'

1) To ask for run :

    Add *jwt* parameter **!!!**

    Request :ref:`api_post_software_muse_run_name` : ::

      curl -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 55", "returned_type":"run.zip", "todownload":"yes"}' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=d0a61955-bd83-4656-9444-6568af141b7a in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/"
      }

2) Then to know if run results are ready :

    Request :ref:`api_get_software_muse_state_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/state/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/

    Response : ::

      {
        "info": {

          "report": {
              "request": {
                  "cmd":"python /add.py 4 55", "cmd_option":"",
                  "ordered_filename_list":[], "returned_type":"run.zip",
                  "todownload":true,
                  "sbatch_list":[], "module_list":[]
              },
              "rsrc": {"identity":"muse"},
              "process": {
                  "jobname":"117-ws-sw_example", "job_id":2461147,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"
              },
              "name":"sw_example",
              "cr":"OK"
          }
        },

        "msg":"End of job. You can now get results by GET http://127.0.0.1:8000/api/software/muse/run/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/ request or ask for content (to investigate) by GET http://127.0.0.1:8000/api/software/muse/content/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/ request.",

        "url":"http://127.0.0.1:8000/api/software/muse/run/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/"
      }

    The run results are now ready (cluster process ended), since *cr* is 'OK'
    into *report* of response above.
    Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
    should be done, until it becomes the case (see also
    :ref:`api_get_software_muse_content_name_key`).

3) Then to get ready run results (according to parameters of initial POST
   request) :

    Request :ref:`api_get_software_muse_run_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/run/sw_example/d0a61955-bd83-4656-9444-6568af141b7a/

    Response : ::

      {
        "msg":"The response content can be downloaded at : http://127.0.0.1:8000/api/download/?key=20210119_130603_a20ce46e-849e-4674-ab2d-b3927a758024 *** The resource to download the response content is 'GET download' with 'key' option (key value : 20210119_130603_a20ce46e-849e-4674-ab2d-b3927a758024) *** Keep this key value in order to be able to download the response content later on : key=20210119_130603_a20ce46e-849e-4674-ab2d-b3927a758024",
        "url":"http://127.0.0.1:8000/api/download/?key=20210119_130603_a20ce46e-849e-4674-ab2d-b3927a758024"
      }

  Then to download the response :

    Request :ref:`api_get_download` : ::

      curl --output MYRETURNEDFILE1_MUSE.zip http://127.0.0.1:8000/api/download/?key=20210119_130603_a20ce46e-849e-4674-ab2d-b3927a758024

    Response :
    :download:`MYRETURNEDFILE1_MUSE.zip<files/MYRETURNEDFILE1_MUSE.zip>`

Example E.
==========

  With *name* = sw_example

  With an input file :download:`stdin.txt<files/stdin.txt>` (containing the
  input parameters of the software sw_example)
  
  Without *files_order*

1) To ask for run :

    Add *jwt* parameter **!!!**

    Request :ref:`api_post_software_muse_run_name` : ::

      curl -F 'file=@stdin.txt' -F 'cmd="python /add.py"' -F 'returned_type=stdout' -F 'todownload=no' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=0cfb698a-3a86-414f-8d5c-959dc4164f2e in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/"
      }

2) Then to know if run results are ready :

    Request :ref:`api_get_software_muse_state_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/state/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/

    Response : ::

      {
        "info": {

          "report": {
              "request": {
                  "cmd":"python /add.py","cmd_option":"",
                  "ordered_filename_list":["stdin.txt"],
                  "returned_type":"stdout","todownload":false,
                  "sbatch_list":[],"module_list":[]
              },
              "rsrc": { "identity":"muse" },
              "process": {
                  "jobname":"118-ws-sw_example","job_id":2461256,
                  "latest_cr":"ENDED_OK","latest_state":"COMPLETED"
              },
              "name":"sw_example",
              "cr":"OK"
          }
        },

        "msg":"End of job. You can now get results by GET http://127.0.0.1:8000/api/software/muse/run/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/ request or ask for content (to investigate) by GET http://127.0.0.1:8000/api/software/muse/content/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/ request.",

        "url":"http://127.0.0.1:8000/api/software/muse/run/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/"
      }

    The run results are now ready (cluster process ended), since *cr* is 'OK'
    into *report* of response above.
    Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
    should be done, until it becomes the case (see also
    :ref:`api_get_software_muse_content_name_key`).

3) Then to get ready run results (according to parameters of initial POST
   request) :

    Request :ref:`api_get_software_muse_run_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/run/sw_example/0cfb698a-3a86-414f-8d5c-959dc4164f2e/

  Response : ::

    {
      "info": {
          "stdout":"70.0",
          "report": {
              "request": {
                  "cmd":"python /add.py","cmd_option":"",
                  "ordered_filename_list":["stdin.txt"],
                  "returned_type":"stdout","todownload":false,
                  "sbatch_list":[],"module_list":[]
              },
              "rsrc":{"identity":"muse"},
              "process":{
                  "jobname":"118-ws-sw_example","job_id":2461256,
                  "latest_cr":"ENDED_OK","latest_state":"COMPLETED"},
              "name":"sw_example",
              "cr":"OK"
           }
      }
    }

Example F.
==========

  With *name* = sw_example

  With an input file :download:`stdin.txt<files/stdin.txt>` (containing the
  input parameters of the software sw_example)
  
  With *files_order*
  
  With a script file :download:`usrcmd.sh<files/usrcmd.sh>` (calling the
  software)

1) To ask for run :

    Add *jwt* parameter **!!!**

    Request :ref:`api_post_software_muse_run_name` : ::

      curl -F 'file=@usrcmd.sh' -F 'file=@stdin.txt' -F 'cmd=usrcmd.sh' -F 'returned_type=stdout' -F 'todownload=no' -F 'files_order=["stdin.txt","usrcmd.sh"]' http://127.0.0.1:8000/api/software/muse/run/sw_example/

    Response : ::

      {
        "msg":"GET http://127.0.0.1:8000/api/software/muse/state/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/ request to get cluster job state, while waiting for results (the GET http://127.0.0.1:8000/api/software/muse/run/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/ request to get results will be possible once job finished). Keep the software name value=sw_example and the key value=de8a34be-dd3f-4f4e-9ea7-5ab820a26b00 in order to be able to follow your POST request. Notifications will be sent during process running if mail-user given into sbatch_list option. (__todo__: add information about temps imparti...)",

        "url":"http://127.0.0.1:8000/api/software/muse/state/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/"
      }

2) Then to know if run results are ready :

    Request :ref:`api_get_software_muse_state_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/state/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/

    Response : ::

      {
        "info": {

          "report": {
              "request": {
                  "cmd":"usrcmd.sh", "cmd_option":"",
                  "ordered_filename_list":["stdin.txt", "usrcmd.sh"],
                  "returned_type":"stdout", "todownload":false,
                  "sbatch_list":[], "module_list":[]
              },
              "rsrc":{"identity":"muse"},
              "process":{
                  "jobname":"119-ws-sw_example", "job_id":2461373,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"},
              "name":"sw_example",
              "cr":"OK"
          }
        },

        "msg":"End of job. You can now get results by GET http://127.0.0.1:8000/api/software/muse/run/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/ request or ask for content (to investigate) by GET http://127.0.0.1:8000/api/software/muse/content/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/ request.",

        "url":"http://127.0.0.1:8000/api/software/muse/run/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/"
      }

    The run results are now ready (cluster process ended), since *cr* is 'OK'
    into *report* of response above.
    Else, some other :ref:`api_get_software_muse_state_name_key` request(s)
    should be done, until it becomes the case (see also
    :ref:`api_get_software_muse_content_name_key`).

3) Then to get ready run results (according to parameters of initial POST
   request) :

    Request :ref:`api_get_software_muse_run_name_key` : ::

      curl http://127.0.0.1:8000/api/software/muse/run/sw_example/de8a34be-dd3f-4f4e-9ea7-5ab820a26b00/

    Response : ::

      {
        "info": {

          "stdout":"70.0",
          "report": {
              "request": {
                  "cmd":"usrcmd.sh", "cmd_option":"",
                  "ordered_filename_list":["stdin.txt","usrcmd.sh"],
                  "returned_type":"stdout", "todownload":false,
                  "sbatch_list":[], "module_list":[]
              },
              "rsrc": { "identity":"muse" },
              "process": {
                  "jobname":"119-ws-sw_example", "job_id":2461373,
                  "latest_cr":"ENDED_OK", "latest_state":"COMPLETED"
              },
              "name":"sw_example",
              "cr":"OK"
          }
        }
      }

