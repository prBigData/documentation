########
Heatmaps
########

This section deals with some scripts we have built to generate different density maps of the vessels within the mediterranean sea.

============================
Data analysis - Introduction
============================

As you might have noticed, we have spent a lot of time (too much time) on figuring out a way to compose our datasets, and to technically implement our solutions leads. We have finally achieved to build the two main pieces of our projects : the scrapers, providing us with our datasets, and the Big Data architecture, allowing us to store our data and to manipulate it.

The next milestone was to perform outstanding analysis on these pieces of data, but due to some lack of time, we only built scripts to generate density maps of vessels within the mediterranean sea area.

We think that your main job will consist in performing rigorous and interesting analysis on this dataset and wish you good luck :).

=======
Scripts
=======

The scripts we've developed to generate our density maps are available at `https://github.com/prBigData/heatmaps <https://github.com/prBigData/heatmaps>`_.


Setup & use the scripts
-----------------------

We have tried to develop it as python modules you just have to download, then import from the pyspark shell.

.. code-block:: bash

    $ git clone https://github.com/prBigData/heatmaps.git
    Cloning into 'heatmaps'...
    remote: Counting objects: 44, done.
    remote: Compressing objects: 100% (35/35), done.
    remote: Total 44 (delta 15), reused 32 (delta 7), pack-reused 0
    Unpacking objects: 100% (44/44), done.
    Checking connectivity... done.

    $ cd heatmaps
    $ pyspark --conf spark.cassandra.connection.host=127.0.0.1 --packages datastax:spark-cassandra-connector:2.0.0-M2-s_2.10

    [...]

Then, the import (example) :

.. code-block:: python

    >>> from heatmap import *

That's all ! Every function from the module is now available.


=============================================================
Generating a density map of any boat in the mediterranean sea
=============================================================

Script : `https://github.com/prBigData/heatmaps/blob/master/heatmap.py <https://github.com/prBigData/heatmaps/blob/master/heatmap.py>`_

.. code-block:: python

    >>> from heatmap import *
    >>> before = datetime.datetime(2016, 12, 10, 22, 54)
    >>> after = datetime.datetime(2016, 12, 10, 22, 53)
    >>> filename = "plot"
    >>> file_path = "./plots/"
    >>> plot_all(spark, sqlContext, before, after, filename, file_path)

Generates a density map of any recorded position between the 2016/12/10 at 22:53 and the 2016/12/10 at 22:54, and saves it into the ./plots/ folder, with the name plot_<before>_<after>_svm.pdf

.. warning::

    The spark and sqlContext variables are context variables available within the pyspark shell, that we need to pass as parameters as we use it within our methods.

===============================================================
Generating a density of boats going to a particular destination
===============================================================

Script : `https://github.com/prBigData/heatmaps/blob/master/recurrent_routes.py <https://github.com/prBigData/heatmaps/blob/master/recurrent_routes.py>`_

.. code-block:: python

    >>> from recurrent_routes import *
    >>> destination = "MARSEILLE"
    >>> filename = "plot"
    >>> file_path = "./plots/"
    >>> plot_all_to_dest(spark, sqlContext, destination, filename, file_path)

Generates a density map of any recorded position of vessels going to MARSEILLE, and saves it into the ./plots/ folder, with the name plot_<before>_<after>_svm.pdf

.. warning::

    The spark and sqlContext variables are context variables available within the pyspark shell, that we need to pass as parameters as we use it within our methods.


======================================================
Additional : Visualizing the trip of a particular boat
======================================================

Script : `https://github.com/prBigData/heatmaps/blob/master/single_vessel_trips.py <https://github.com/prBigData/heatmaps/blob/master/single_vessel_trips.py>`_

.. code-block:: python

    >>> from single_vessel_trips import *
    >>> mmsi = "305169000"
    >>> filename = "plot"
    >>> file_path = "./plots/"
    >>> plot_vessel_trip(spark, sqlContext, mmsi, filename, file_path)

Plots every recorded position for the vessel with mmsi 305169000.

=======================
Technical coniderations
=======================

The use of pyspark and python for data analysis purpose might not be the most optimised solution. We have decided to use that stack as we lacked time and it was based on the language we knew the best.

One of the first steps of your project should be, maybe, to re-consider these choices according to your analysis goals.


==========
Conclusion
==========

Good luck with your new algorithms study and implementation ;) .





