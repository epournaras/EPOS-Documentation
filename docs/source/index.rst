.. EPOS documentation master file, created by
   sphinx-quickstart on Fri Aug 31 12:10:00 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

=================================================
EPOS - Economic Planning and Optimized Selections
=================================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   experimantal-setup
   command-line-arguments
   input
   global-cost-function
   local-cost-function
   tree-topology
   termination-criteria
   multi-objective-optimization
   logging-output
   LiveRun
   advanced-topics

   
Installation
------------

The project can be installed in 2 possible ways:

- A command line executable, that can be found under the project `github releases <https://github.com/epournaras/EPOS/releases>`_.  Once the :code:`release-x.x.x.zip` is downloaded and extracted the user can navigate in the extracted folder and follow the guideline at :ref:`command-line-setup`. For this setup a `Java 8 or higher <https://www.java.com/en/download/>`_ should be installed.
- IDE imported source code, that can be found under the `tutorial branch <https://github.com/epournaras/EPOS/tree/tutorial/>`_ of the `project github repository <https://github.com/epournaras/EPOS/>`_. Detailed instructions for getting and importing the source code to an IDE can be found at :ref:`ide-setup`. For this setup `jdk-8uxxx or higher <https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>`_ should be installed along with `Maven 3.3 or higher <https://maven.apache.org/>`_ and the latest version of `git <https://git-scm.com/>`_.

   
Open Datasets
-------------

**Energy**: http://epos-net.org/shared/datasets/EPOS-ENERGY-SUBSET.zip

**Electric vehicles**: http://epos-net.org/shared/datasets/EPOS-ELECTRIC-VEHICLES.zip

**Bike sharing**: http://epos-net.org/shared/datasets/EPOS-BICYCLES.zip


Software Artifact
-----------------

An earlier software artifact with a Graphical User Interface (GUI) can be acquired via `EPOS Exemplar <http://epos-net.org/software/exemplar/>`_. The website contains an executable and instructions about the latest artifact version of EPOS. It is advised to use the website software for demonstration purposes. Detailed usage guidelines and explanations regarding the system architectures can also be found in the website.

.. _installation-setup:


Contribute
----------

- `Issue Tracker <https://github.com/epournaras/EPOS/issues>`_
- `Source Code <https://github.com/epournaras/EPOS>`_

Support
-------

For any question please contact us: mail@epos-net.org

License
-------

The project is licensed under the GPL-2.0 license.

Publications
------------
Evangelos Pournaras, Peter Pilgerstorfer and Thomas Asikis, Decentralized Collective Learning for Self-managed Sharing Economies, ACM Transactions of Autonomous and Adaptive Systems,  2018, ACM, `url <http://evangelospournaras.com/wordpress/wp-content/uploads/2018/09/Decentralized-Collective-Learning-for-Self-managed-Sharing-Economies.pdf>`_



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
