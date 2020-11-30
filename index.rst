.. okerr documentation master file, created by
   sphinx-quickstart on Thu Nov  5 22:21:13 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


`Okerr <https://okerr.com/>`_ - lightweight monitoring software/service for small and medium projects.

This is english wiki. (Есть `документация на русском языке <https://okerr.readthedocs.io/ru/latest/>`_)

If you have any questions - ask in our `freshdesk <https://okerr.freshdesk.com/support/home>`_ 
or open an `issue <https://github.com/yaroslaff/okerr-dev/issues>`_ on github or just `write me personally <mailto:yaroslaff@gmail.com>`_. 

Okerr is open-source project, and okerr documentation too. Please, feel free to edit this documentation as 
`okerr-doc-en <https://github.com/yaroslaff/okerr-doc-en>`_ public git project. We are not native english speakers, so 
if you can re-write part of text in better language - please do it. 


Okerr user guide
=================

.. toctree::
   :maxdepth: 1
   :titlesonly:

   User/Features
   User/Introduction
   User/Indicators
   cm/index
   User/Policies
   User/Eliminating-false-positives
   User/Failover
   

Okerr server admin guide
==========================

.. toctree::
   :maxdepth: 1
   
   Admin/Install
   Admin/Manual-install
   Admin/Configuration-files
   Admin/Bonus-codes
   Admin/cli/index
   Admin/cluster/index
   Admin/Error-codes.md
   Admin/Third-party-software-cheatsheet


Developer guide
===============

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Dev/RabbitMQ-queues.md
   Dev/Syntax-rules
   Dev/Update-indicators-over-HTTPs
   Dev/API
