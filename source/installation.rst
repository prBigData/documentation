##################
Installation Guide
##################

***********************
Project sources
***********************

The project is available on Github or you can ask for the sources

===========
Github Fork
===========

The organisation's repo can be found at `https://github.com/prBigData <https://github.com/prBigData>`_.

There, you can find the sources of the different parts of the project, more specifically :

* The scrapers code : `https://github.com/prBigData/scraper <https://github.com/prBigData/scraper>`_
* The heatmaps generation scripts : `https://github.com/prBigData/heatmaps <https://github.com/prBigData/heatmaps>`_


As for any public github project, you can fork the source code, and clone the repo locally, e.g.

.. code-block:: bash

    mkdir prdw
    cd prdw
    git clone https://github.com/prBigData/scraper.git

(Obviously the project url won't be the same, if you fork the repo)

=====================================
Asking for source code or permissions
=====================================

As an alternative, you can also ask for the rights on the repo, or the full sources (available on my github profile)


***********************
Python and dependencies
***********************

The next thing to do is to setup your python environment.

==============
Python version
==============

We used python 2.7.11. to develop the different scripts composing the project. Make sure your python version is the same if you want to make sure the scripts work for you

================
Pip & Virtualenv
================

Python should be supplied with a working version of `pypi <https://pypi.python.org/pypi>`_.

What you may want to do now is to set up a virtual environment for working with python, in order to avoid messing up your other python projects. We strongly advice that you get to know python's `virtualenv module <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_ if you don't know it yet.

.. warning::

    Tip : always check the status (activated or not) of your virtual environment when working on your project


=============================
Dependencies and requirements
=============================

When your python environment is ready, you can install dependencies of each project within each of your environment by simply using the "requirements.txt" files as follows :

.. code-block:: bash

    pip install -r requirements.txt

Otherwise you can use pip to install any new python module the usual way. We provide a list of dependencies for each part of the project in the corresponding section of the documentation.

