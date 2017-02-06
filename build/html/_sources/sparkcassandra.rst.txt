###################
Spark and Cassandra
###################

Here's everything to know about the use of Spark and Cassandra in the project.


********************
Setting up cassandra
********************

The current build is Cassandra 3.9, on top of Java 8 (JDK 1.8)

.. code-block:: bash

    java version "1.8.0_111"
    Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
    Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)

CQLSH version 5.0.1

To set up cassandra, follow the same instruction as you can find on the internet (and cross fingers, hopefully you won't get into any compatibility and versioning troubles) :

#. Go to the `apache project page`_, download and untar the ball file.
#. Add the CASSANDRA_HOME to the path.
#. Edit cassandra.yaml and put the right IP adresse(s) ('127.0.0.1' for localhost).
#. Launch Cassandra with the command 'cassandra' (Be sure that JAVA_JOME is setted)

.. _apache project page: https://cassandra.apache.org/download/

******************
Cassandra basics
******************


==================
Tables Definition
==================

======
CQLSH
======

****************
Setting up Spark
****************


***********************************************
Setting up Datastax's Spark-Cassandra connector
***********************************************




**********
Conclusion
**********

Your Big Data architecture is now all set, and fed in real time by your scrapers. Time to data mine ! The last section of that documentation, "Heatmaps", deals with some scripts we have written to generate a density map of the vessels within the mediterranean sea. See you there.


