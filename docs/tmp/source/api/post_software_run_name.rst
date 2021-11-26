.. _api_post_software_run_name:

========================
POST software/run/{name}
========================

URL :: 

  http://127.0.0.1:8000/api/software/run/{name}

Access
======

:ref:`auth` *is required ; see also* :ref:`JWT<api_jwt>`.

.. include:: ui_token_right.rst

Description
===========

Runs the *name* software and returns the running result, according to the
request parameters.

**Local run**, always **as Singularity command** (if the software container
type is 'singularity' and even if the software container type is 'docker').

See :download:`diagram <files/POST_run.pdf>` (local run and get result).

Request parameters
==================

  About what to be executed :
  :ref:`name<p_psrn_name>` | :ref:`cmd<p_psrn_cmd>` |
  :ref:`cmd_option<p_psrn_cmd_option>`


  About access :
  :ref:`jwt<p_psrn_jwt>`

  About input datas :
  :ref:`file<p_psrn_file>` | :ref:`files_order<p_psrn_files_order>`

  About what to be returned :
  :ref:`returned_type<p_psrn_returned_type>` |
  :ref:`todownload<p_psrn_todownload>` | :ref:`format<p_psrn_format>`

.. _p_psrn_name:
 
Parameter name (mandatory)
--------------------------

  - Description : software name
  - Type : string
  - Note : to list all available softwares, use :ref:`api_get_software_list`

.. _p_psrn_cmd:

Parameter cmd
-------------

  - Description : command to run
  - Type : string
  - Default value : ''

.. _p_psrn_cmd_option:

Parameter cmd_option
--------------------

  - Description : command options as **singularity** container command

    The *cmd_option* content must follow the **Singularity syntax**.

    Fore more see : :ref:`singularity syntax for cmd_option<docker_container_post_software_run_name>`

  - Type : string
  - Default value : ''

For a software delivered as 'docker' or 'singularity' type, with a
corresponding singularity image named {image} *(that is or comes from the
software container image)*, this leads to a command *'such as'* : ::

    singularity exec {cmd_option} {image} {cmd}


.. _p_psrn_jwt:

Parameter jwt (mandatory)
-------------------------

  - Description : for authentication (JSON Web Token).

.. _p_psrn_file:

Parameter file
--------------

  - Description : some input files to be copied, and unzipped (in zip file case).
  - Type : file.

.. _p_psrn_files_order:

Parameter files_order
---------------------

  - Description : list of file names giving the order to treat them.
  - Type : list of strings.

=> So this leads to a files treatment such as ::

    For filename in files_order :
      1) copy filename file (overwrite)
      2) unzip filename file (if zip file)

.. _p_psrn_returned_type:

Parameter returned_type
-----------------------

  - Description : type of returned result.
  - Type : string.
  - Available values :

    ============== ==========================================================
     'run.zip'      for the run folder content to be returned, as a zip file
    -------------- ----------------------------------------------------------
     'stdout.txt'   for the txt stdout file to be returned
    -------------- ----------------------------------------------------------
     'stdout'       for the stdout file content to be returned, as a string
    ============== ==========================================================

  - Default value : 'stdout'

.. _p_psrn_todownload:

Parameter todownload
--------------------

  - Type : string.
  - *todownload* is available only if *returned_type* = 'run.zip' or
    'stdout.txt'
  - Available values :

    ======= ==================================================
     'yes'   for results to be downloaded later on
    ------- --------------------------------------------------
     'no'    for results to be immediatly returned/sent
    ======= ==================================================

  - Default value : 'no'

.. _p_psrn_format:

Parameter format
----------------

  - Description : response format.
  - Available values : 'json', 'api', ('yaml', 'xml').
  - Default value depending on request source.
      
Request response
================

The running result, according to request parameters : 
:ref:`returned_type<p_psrn_returned_type>`,
:ref:`todownload<p_psrn_todownload>`, :ref:`format<p_psrn_format>`.
  
Example A.
==========

With *name* = sw_example

Add *jwt* parameter **!!!**

  Request : ::

    curl -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 5", "returned_type":"stdout", "todownload":"no"}' http://127.0.0.1:8000/api/software/run/sw_example/

  Response : ::

    {
        "info": {
            "stdout":"9.0",
            "report": {
                "name":"sw_example",
                "request": {
                    "cmd":"python /add.py 4 5",
                    "cmd_option":"",
                    "ordered_filename_list":[],
                    "returned_type":"stdout",
                    "todownload":false
                },
                "cr":"OK"
            }
        }
    }

Example B.
==========

With *name* = sw_example

With *returned_type* = 'run.zip'

Add *jwt* parameter **!!!**

  Request : ::
   
    curl --output MYRETURNEDFILE.zip -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 55", "returned_type":"run.zip", "todownload":"no"}' http://127.0.0.1:8000/api/software/run/sw_example/

  Response : 
  :download:`MYRETURNEDFILE.zip<files/MYRETURNEDFILE.zip>`

Example C.
==========

With *name* = sw_example

With *returned_type* = 'stdout.txt'

Add *jwt* parameter **!!!**

  Request : ::

    curl --output MYRETURNEDFILE.txt -H "Content-Type: application/x-www-form-urlencoded" -d 'cmd=python /add.py 4 55&returned_type=stdout.txt&todownload=no' http://127.0.0.1:8000/api/software/run/sw_example/

  Response : 
  :download:`MYRETURNEDFILE.txt<files/MYRETURNEDFILE.txt>`

Example D.
==========

With *name* = sw_example

With *todownload* = 'yes'

Add *jwt* parameter **!!!**

  Request : ::
   
    curl -H "Content-Type: application/json" -d '{"cmd":"python /add.py 4 55", "returned_type":"run.zip", "todownload":"yes"}' http://127.0.0.1:8000/api/software/run/sw_example/

  Response : :: 

    {
        "msg":"The response content can be downloaded at : http://127.0.0.1:8000/api/download/?key=20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91 *** The resource to download the response content is 'GET download' with 'key' option (key value : 20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91) *** Keep this key value in order to be able to download the response content later on : key=20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91",

        "url":"http://127.0.0.1:8000/api/download/?key=20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91"
    }

Then to download the response :
  
  Request :ref:`api_get_download` : ::

    curl --output MYRETURNEDFILE1.zip http://127.0.0.1:8000/api/download/?key=20200806_163349_0591aaf8-8a12-4967-848a-1268784ffd91

  Response :
  :download:`MYRETURNEDFILE1.zip<files/MYRETURNEDFILE1.zip>`

Example E.
==========

With *name* = sw_example

With an input file :download:`stdin.txt<files/stdin.txt>` (containing the
input parameters of the software sw_example)

Without *files_order*

Add *jwt* parameter **!!!**

  Request : ::

    curl -F 'file=@stdin.txt' -F 'cmd="python /add.py"' -F 'returned_type=stdout' -F 'todownload=no' http://127.0.0.1:8000/api/software/run/sw_example/

  Response : ::

    {
        "info": {
            "stdout":"70.0",
            "report": {
                "name":"sw_example",
                "request": {
                    "cmd":"python /add.py",
                    "cmd_option":"",
                    "ordered_filename_list":["stdin.txt"],
                    "returned_type":"stdout",
                    "todownload":false
                },
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

Add *jwt* parameter **!!!**

  Request : ::

    curl -F 'file=@usrcmd.sh' -F 'file=@stdin.txt' -F 'cmd=usrcmd.sh' -F 'returned_type=stdout' -F 'todownload=no' -F 'files_order=["stdin.txt","usrcmd.sh"]' http://127.0.0.1:8000/api/software/run/sw_example/

  Response : ::

    {
        "info": {
            "stdout":"70.0",
            "report": {
                "name":"sw_example",
                "request": {
                    "cmd":"usrcmd.sh",
                    "cmd_option":"",
                    "ordered_filename_list":["stdin.txt","usrcmd.sh"],
                    "returned_type":"stdout",
                    "todownload":false
                },
                "cr":"OK"
            }
        }
    }

