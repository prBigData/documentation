###################
Spark and Cassandra
###################

Here's everything to know about the use of Spark and Cassandra in the project.

============
Introduction
============

Before beginning any installation / configuration, we strongly advice you to read our report in order to understand what led us for our technology stack choices.

Then, obviously you can begin to set up your cluster (if you're not re-using our server).

=====
Spark
=====

Setting up Spark
-----------------

We installed Spark in a classical way, as described on any `web reference <http://spark.apache.org/docs/latest/spark-standalone.html>`_ .


Spark version
-------------

.. code-block:: bash

    Welcome to
          ____              __
         / __/__  ___ _____/ /__
        _\ \/ _ \/ _ `/ __/  '_/
       /__ / .__/\_,_/_/ /_/\_\   version 2.0.2
          /_/

    Using Python version 2.7.9 (default, Jun 29 2016 13:08:31)
    SparkSession available as 'spark'.
    >>>

.. note::

    We mainly used the pyspark interface, as we were more used to python than scala.


=========
Cassandra
=========

Setting up Cassandra
--------------------

Idem, we set up Cassandra in a classical way, as described on any `web reference <http://cassandra.apache.org/doc/latest/getting_started/index.html>`_.


Cassandra Version
-----------------
The current build is Cassandra 3.9, on top of Java 8 (JDK 1.8)

.. code-block:: bash

    java version "1.8.0_111"
    Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
    Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)


CQLSH Version
-------------

CQLSH is version 5.0.1


Tables Definition
-----------------

Once you are done setting up Cassandra, you can create your tables using the same syntax as in `https://github.com/prBigData/scraper/blob/master/cassandra/init_db.py <https://github.com/prBigData/scraper/blob/master/cassandra/init_db.py>`_ .


Inserting into Cassandra
------------------------

Within the scraper scripts, you can find examples on how to insert into Cassandra `https://github.com/prBigData/scraper/blob/master/get_data.py <https://github.com/prBigData/scraper/blob/master/get_data.py>`_ .


Important note on our tables structure
--------------------------------------

Again, you are adviced to read our report to understand what leds our tables structure thinking, what issue we encountered, and why we modified our structure from our original thoughts.


====================================
Datastax's Spark-Cassandra connector
====================================

In order to perform data analysis, Spark has to be connected to Cassandra through Datastax's open source connector.

Setting up the connector
------------------------

We followed `Datastax's documentation <https://github.com/datastax/spark-cassandra-connector>`_ to set up the connector.


Launching pyspark linking it to Cassandra
-----------------------------------------

Then to specify spark to connect to Cassandra while opening the pyspark prompt (for example), use :

.. code-block:: bash

    pyspark --conf spark.cassandra.connection.host=127.0.0.1 --packages datastax:spark-cassandra-connector:2.0.0-M2-s_2.10


==========
Conclusion
==========

Your Big Data architecture is now all set, and fed in real time by your scrapers. Time to data mine ! The last section of that documentation, "Heatmaps", deals with some scripts we have written to generate a density map of the vessels within the mediterranean sea. See you there.


