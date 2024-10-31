Enos-kubernetes
===============

|License|

This is a fork of: https://gitlab.inria.fr/discovery/enos-kubernetes

The original project did not work correctly for us, we make some small changes
and gather the correct dependencies.

Deploys Kubernetes on various providers. Deployments are for
evaluation/experimental purpose (not production).

Behind the scenes this project uses:

* Kubespray (https://github.com/kubernetes-sigs/kubespray): It uses decent
  default values but this should be reasonnably customizable (through roles and
  vars in the configuration file)

* EnOSlib (https://gitlab.inria.fr/discovery/enoslib) for the framework part.

Command line overview
---------------------

.. code-block :: bash

    Usage: ek [OPTIONS] COMMAND [ARGS]...

    Options:
      --help  Show this message and exit.

    Commands:
      backup        Backup the deployed environment
      build         Preconfigure a machine with all the...
      deploy        Claim resources from a PROVIDER and configure...
      destroy       Destroy the deployed environment
      g5k           Claim resources on Grid'5000 (frontend).
      hints         Give some hints on the deployment
      inventory     Generate the Ansible inventory [after g5k or...
      post_install  Post install the deployement
      prepare       Configure available resources [after deploy,...
      reset         Resets Kubernetes (see kspray doc)
      vagrant       Claim resources on vagrant (localhost).


Usage overview
--------------

Install the project::

    conda create --name EnOSKubernetes python=3.8.12 ipython
    conda activate EnOSKubernetes
    conda install pip
    pip install -r requirements.txt

Configure the Grid5000 REST API access::

    echo '
    username: MYLOGIN
    password: MYPASSWORD
    ' > ~/.python-grid5000.yaml

For an example configuration file, see conf.yaml.example.

Deploy on g5k::

    python -m enos_kubernetes.cli deploy g5k

Deploy on g5k using virtual machines::

    python -m enos_kubernetes.cli deploy vmong5k

Build a base image on g5k::

    python -m enos_kubernetes.cli deploy g5k

Build a base image on vmong5k with an alternative cluster::

    python -m enos_kubernetes.cli deploy build vmong5k --cluster=chetemi

This also can be used from python directly using the provided API::

    # pseudo-code to deploy to g5k
    from enos_kubernetes import tasks

    ...
    tasks.g5k(...)
    tasks.inventory(...)
    tasks.prepare(...)
    ...

.. |License| image:: https://img.shields.io/badge/License-GPL%20v3-blue.svg
   :target: https://www.gnu.org/licenses/gpl-3.0