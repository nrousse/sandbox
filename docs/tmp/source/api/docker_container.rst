.. _docker_container:

=====================
Docker container case
=====================

Software delivered as a Docker container : ::

  software container :
      type : 'docker' , image : {image}

.. _docker_container_post_software_run_name:

Running the software as a Singularity container
===============================================

Case of requests : :ref:`api_post_software_run_name`, 
:ref:`api_post_software_muse_run_name`

Description
-----------

  If the software container type is 'docker', then a Singularity container
  has been built (while installing the software) from its original delivered 
  Docker container. In :ref:`api_post_software_run_name` and
  :ref:`api_post_software_muse_run_name` cases, that is this built
  Singularity container that will be run, as Singularity command. ::

   software container :
       type : 'docker' , image : {image}
   Prerequisite :
       {singularity_image_file} has been built from docker {image}
   Command 'such as' :
       singularity exec {cmd_option} {singularity_image_file} {cmd}

  This explains why, in :ref:`api_post_software_run_name` and
  :ref:`api_post_software_muse_run_name` cases, the **cmd_option** content
  must follow the **Singularity syntax**, even if the software container type
  is 'docker'.

From Docker syntax to Singularity syntax
========================================

Bind
----

  - Docker :

    Into the Docker command option : ::

       -v <host_path>:<container_path>

  - Singularity :

    Into the request cmd_option : ::

      --bind <host_path>:<container_path>

    For Singularity, it may be required to previously create some {host_path}
    folders if they do not already exist : create and zip into f.zip the
    required folders and send f.zip into the request : ::

      file=f.zip

Set variables
-------------

  - Docker :

    Into the Docker command option : ::

      -e "VAR=value"
  
  - Singularity with a script file :

    Instead of request with : cmd={cmd} 

    1) Build a script file my_file.sh (in order to set VAR value) : ::

         #!/bin/bash
         VAR=value
         {cmd}

    2) And then use request with : ::

         cmd=my_file.sh and file=my_file.sh

