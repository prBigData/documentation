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

No dataset was supplied at the beginning of our PRDW. We have had to get data our own way. To do so, we've built two scrapers allowing us to get `https://www.vesselfinder.com/ <https://www.vesselfinder.com/>`_'s data for free.


************************************
Getting used to the technology stack
************************************

To learn quickly the required technologies, here are some useful links :

* **Python** : `https://www.tutorialspoint.com/python/ <https://www.tutorialspoint.com/python/>`_
* **Python's request module** : `http://docs.python-requests.org/en/master/ <http://docs.python-requests.org/en/master/>`_
* **XPath** : `http://lxml.de/xpathxslt.html <http://lxml.de/xpathxslt.html>`_
* **tmux** : `https://danielmiessler.com/study/tmux/ <https://danielmiessler.com/study/tmux/#gs.gpLb2M0>`_
* **linux crontab** : `http://kvz.io/blog/2007/07/29/schedule-tasks-on-linux-using-crontab/ <http://kvz.io/blog/2007/07/29/schedule-tasks-on-linux-using-crontab/>`_


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
        a. Make an HTTP request to get any vessel in a given zone, calling vesselfinder's servers
        b. Open the current vessels positions export file and get the currently saved positions list
        c. FOR line (= vessel) IN the HTTP response :
            i.  parse the line
            ii. format the vessel's data
            iii. store it into cassandra
            iv. append the vessel's data to the currently saved positions list
            v. append the current vessel's mmsi to the current mmsi list
        d. save the resulting position list into the positions export file
        e. read the daily mmsi list file and get the already detected mmsi
        f. merge the already detected mmsi list with the current mmsi list
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

When set up, we have launched and controlled the script within a tmux session. If you're not used with tmux, we advice you to read this really concise guide : `https://danielmiessler.com/study/tmux/ <https://danielmiessler.com/study/tmux/>`_

Launching the script
--------------------

First create a tmux session, then launch your "daemon" script within it, then detach from the tmux session :

.. code-block:: bash

    $ tmux new -s scraper1
    $ python get_data.py > get_data.log 2>&1
    $ ctrl-b d     (like any keyboard shortcut)
    [detached (from session scraper1)]


Killing the script
------------------

You can check any active tmux session by using :

.. code-block:: bash

    $ tmux ls
    scraper1: 1 windows (created Mon Feb  6 19:19:13 2017) [89x48]

Then you can re-attach to that session and kill your script before exiting the tmux session by using :


.. code-block:: bash

    $ tmux a -t scraper1
    $ ctrl-c
    $ ctrl-d


Monitoring the script
---------------------

You can monitor the scripts logs by simply following the tail of the logs :

.. code-block:: bash

    $ tail -f get_data.log

You can use the same command to check for the evolution of the export file while stuff is written in it.


===================
First scraper fails
===================

For the record, we have had difficulties building these two scrapers, here are some aborted tries (so that you don't do the same mistakes again):

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

