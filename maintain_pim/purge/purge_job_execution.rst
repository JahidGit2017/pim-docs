How to Purge jobs executions
============================

This chapter gives details on how to purge jobs executions in order to decrease the database volume.

The batch job command
---------------------

Akeneo PIM provides a simple command to purge jobs executions:

.. code-block:: bash
    :linenos:

    bin/console akeneo:batch:purge-job-execution [-d|--days DAYS]

.. tip::
    Don't forget to add --env=prod to avoid memory leaks in dev environment (the default environment for commands)

You can also provide a custom configuration for the job:

.. code-block:: bash
    :linenos:

    bin/console akeneo:batch:purge-job-execution --days=30 --env=prod

For example, with the option --days=30, the command will remove all jobs executions and their archives when they were created over 30 days ago.

Scheduling the jobs
-------------------

To run a command periodically, you can use a cron_:

.. _cron: https://help.ubuntu.com/community/CronHowto

First, you need to install it (example in debian/ubuntu based distributions):

.. code-block:: bash
    :linenos:

    apt-get install cron

Then, you can edit your crontab:

.. code-block:: bash
    :linenos:

    crontab -e

You can now add a new line at the end of the opened file:

.. code-block:: bash
    :linenos:

    0 0 * * * /home/akeneo/pim/bin/console akeneo:batch:purge-job-execution --env=prod

With this cron configuration a purge of jobs executions older than 90 days, will be launched once a day.

.. note::

    This purge command removes the job executions from the database but does not remove files from app/archive.
