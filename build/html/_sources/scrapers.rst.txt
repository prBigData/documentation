########
Scrapers
########

TO CHECK :
tmux
illustration
cron and stuff scraper 2

************
Introduction
************

No dataset was supplied at the beginning of our PRDW. We have had to get data our own way. To do so, we've built two scrapers allowing us to get `vesselfinder`_'s data for free.


.. _vesselfinder: https://www.vesselfinder.com/


**********************************
FIRST SCRAPER : position's scraper
**********************************

First we've developed a scraper to get the vesselfinder's front page data. We reproduce our browser's AJAX calls, done every minute to vesselfinder's servers to update the vessels map.

=========
Algorithm
=========

The developed algorithm can be sumed up as :

.. code-block:: text

    1. WHILE (TRUE) :
        a. Make an HTTP request to get any vessel in a given zone calling vesselfinder's servers
        b. Open the current vessels positions export file and get the currently saved positions list
        c. FOR line (= vessel) IN the HTTP response :
            i.  parse the line
            ii. format the vessel's data
            iii. store it into cassandra
            iv. append the vessel's data to the currently saved positions list
            v. append the current vessel's mmsi to the current iteration mmsi list
        d. save the resulting position list into the export file
        e. read the daily mmsi list file and get the already detected mmsi
        f. merge the already detected mmsi list with the current iteration mmsi list
        g. SLEEP(60)


--
NB
--

* We use a new export file for the positions every hour
* We use a new export file for the mmsi lists every day
* This is achieved thanks to a strict naming convention you can observe within the code
* Understanding list comprehension in python is a plus, to understand our code


===============================
How to set the first scraper up
===============================

1. Download the "get_data.py" script and put it into a folder which has a "mmsi_lists" subfolder as well as a "mmsi_positions" subfolder. Create them if needed
2. Create the __init__.py files at least in the main folder
3. Install the python dependencies

The modules structure should be as follows :

.. code-block:: bash

    /scraper/
        /mmsi_positions/
        /mmsi_lists/
        __init__.py
        get_data.py

That's all

===============================
How to launch the first scraper
===============================

When set up, we have launched and controlled the script within a tmux session. If you're not used with tmux, we advice you to read this really concise guide : `tmux`_


.. _tmux: http://


--------------------
Launching the script
--------------------

This way you can launch the script connecting to your tmux session then launching it (don't forget to log :) ) :

.. code-block:: bash

    tmux truc
    python get_data.py > get_data.log 2>&1
    TO CHECK (both lines)


------------------
Killing the script
------------------

And you can kill your script, again, connecting to your tmux session and sending the kill signal :


.. code-block:: bash

    tmux truc
    ^C
    TO CHECK (both lines)


===================
First scraper fails
===================

We have had difficulties building these two scrapers, here are some aborted tries (so that you don't do the same mistakes again):

* Using a node.js crawler : we didn't have enough experience
* Using the scrapy python module : overkill



****************************************************
SECOND SCRAPER : The additionnal information scraper
****************************************************

Every information we needed wasn't available from the first scraper as there was no departure nor destination information on the front map of the vesselfinder website. Hence we've been compelled to build a second scraper, in order to get additionnal pieces of informations on every vessel detected by the first scraper.

==============
Main principle
==============

The main principle of that scraper is to try to get at day d+1 as much information as possible on every vessel detected by our first scraper at day d (hence the mmsi list exports !)

=========
Algorithm
=========

The developed algoithm can be sumed up as follows :

.. code-block:: text

    1. Open and read the mmsi list file exported the previous day
    2. Generate a list of URLs to query
    3. Spawn a "spider" that will request, parse and export additionnal information at each URLs of the list

This "spider" is a helper class we have handwritten (named after scrapy's spiders) providing us with several useful scraping and parsing methods. When calling the scrap method of the spider, it will proceed as follows :

.. code-block:: text

    1. FOR EACH url passed to the constructor
        a. make an HTTP request to get the additionnal info html page from vesselfinder
        b. parse the response page
        c. post treat the parsing results if necessary
        d. append the additional infos on that vessel to the additional info export file
        e. store it cassandra as well
        f. SLEEP(SLEEP_DELAY)


--
NB
--

Again, there might be some tricky parts to understand within our code.

* It is bit messy (we admit) : not so straightforward ("why the f... did they put that here ?") as we developed it iteratively, adding features step by step
* ABOUT THE SLEEP DELAY : why isn't it fixed ? why a sleep delay ? => Mainly because we obviously couldn't afford to request information several thousands vessels at the same time to vesselfinder's severs (which would have meant xxxx calls at the same time to their API). So we have decided to smoothly request it all along 22hours of a day. So the sleep delay is in fact 22hours divided by the number of detected vessels the previous day.

================================
How to set the second scraper up
================================

This second scraper uses two scripts : "cron.py" and "spider.py"


1. Download these two files and order it using the same structure as specified above (create the subfolders and __init__.py if needed)

.. code-block:: bash

    /scraper/
        /mmsi_lists/
        /mmsi_infos/
        /mmsi/
            __init__.py
            spider.py
        __init__.py
        cron.py

2. Edit the linux crontab so that it launches the "cron.py" script on a daily basis

TO DO here


That's all !


================
How to launch it
================


Linux crontab will automatically launch the cron script every day at the time you specified. Then the cron and the spider will work by themselves.



====================
Second scraper fails
====================

Once again, here are some of our aborted tries :

* Scraping `marine traffic`_'s website : we were detected as robots after 5 calls
* Scraping `aishub`_'s website : almost none of the vessels we detected from our first scraper was available in their database


.. _marine traffic: http://www.marinetraffic.com/

.. _aishub: http://www.aishub.net/vessels-database.php



**********
Conclusion
**********

Once you get both scrapers set up, your working dataset gets generating. You just reached the first milestone of your project and you're ready to go ahead to the second step before beginning the data analysis work :  building your big data architecture.

Before setting up Spark and Cassandra on your server (next step of that documentation) we strongly advice you to read our project report to understand the work of technological study and comparisons that lead us through our technology choices. Then you can follow the "spark and cassandra" section of that guide to set your big data environment up.

