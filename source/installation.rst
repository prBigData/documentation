##################
Installation Guide
##################


**********************
Dowloading the project
**********************

The project is available on Github or you can ask for the sources

===========
Github Fork
===========

The organisation's repo can be found on `github, PR Big Data`_.

.. _github, PR Big Data: https://github.com/prBigData

There, you can find the sources of the different parts of the project, more specifically :

* The scrapper's code : `github, aisdata`_
* The heatmaps generation scripts : `github, datatreatment`_

.. _github, aisdata: https://github.com/prBigData/aisdata

.. _github, datatreatment: https://github.com/prBigData/datatreatment

As for any public github project, you can fork the source code, and clone the repo locally, e.g.

.. code-block:: bash

    mkdir project
    cd project
    git clone https://github.com/prBigData/datatreatment.git


======================
Asking for source code
======================

As an alternative, you can also ask me for the full sources by email (available on my github profile)


***********************
Python and dependencies
***********************

The next thing to do is to setup your python environment.

==============
Python version
==============

The version we used to develop the project is python 2.7.11. Make sure your python version is the same if you to make sure the script work for you

================
Pip & Virtualenv
================

Python should be supplied with a working version of `pypi`_.

.. _pypi: https://pypi.python.org/pypi

What you may want to do now is to set up a virtual environment for working with python, in order to avoid messing up your other python projects. We strongly advice that you get to know python's `virtualenv module`_ if you don't know it yet.

.. _virtualenv module: http://docs.python-guide.org/en/latest/dev/virtualenvs/


.. warning::

    Tip : MAKE SURE YOUR VIRTUAL ENV IS ALWAYS ACTIVATED WHEN YOU WORK ON THE PROJECT


=============================
Dependencies and requirements
=============================

When your python environment is ready, you can install any python dependency required for a project by using

.. code-block:: bash

    pip install -r requirements.txt

