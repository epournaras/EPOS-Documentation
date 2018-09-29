.. EPOS documentation master file, created by
   sphinx-quickstart on Fri Aug 31 12:10:00 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

EPOS - Economic Planning and Optimized Selections
================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

Features
--------

- Experimental and simulation setup: determine the varied parameters. 

  - `h`: show helptext

  - `dataset`: Dataset name to be used. This is the name of the directory located inside datasets/ directory thta contains all data. No default value, must be set

  - `numSim`: Number of simulations. Default is 1.

  - `numIterations`: Number of iterations. Default is 40.

  - `lambda`: Lambda preference level. Lambda > 0. Used only for old I-EPOS. Default is 0.

  - `alpha`: Weight of unfairness in multi-objective function. 0<= Alpha <= 1. Default is 0.

  - `beta`: Weight of local cost in multi-objective function. 0<= Beta <= 1. Default is 0.

  - `numAgents`: Number of participating agents. Default is 100.

  - `numPlans`: The maximum number of possible plans per agent. Lower than this is possible. If more exist in the file, only first numPlan rows are read. Default is 16.

  - `planDim`: Dimension of every possible plan of every agent. Must be equal across the agents and must correspond to the dataset. Default to -1 and must be set!

  - `numChildren`: Number of children of each inner node. Default is 2.

  - `shuffle`: The number of the shuffles to make before assigning agents to the tree hierarchy. Default is 0.

  - `shuffleFile`: The path to a file containing already shuffled agents in one column, no header. Default is null.

  - `reorganizationSeed`: The seed to be used in Random generator the shuffling is based on. Default is 0.

  - `enableNEVERstrategy`: Enabling reorganization strategy NEVER. Works only for ModifiableIEPOSAgent. This is default.

  - `enablePERIODICALLYstrategy`: Enabling reorganization strategy PERIODICALLY with indicated period. Default period is 3, but default strategy is to NEVER reorganize.

  - `enableCONVERGENCEstrategy`: Enabling reorganization strategy ON_CONVERGENCE with indicated memorization offset. Default is 5, but default strategy is to NEVER reorganize.

  - `enablePREDEFINEDstrategy`: Enabling starting IEPOS with predefined selected plans, but default strategy is to NEVER reorganize.

  - `enableGLOBALCOSTREDUCTIONstrategy`: Enabling reorganization strategy based on GLOBAL_COST_REDUCTION strategy with indicated tolerance level. 0 <= Tolerane Level <= 1. Default is 0.5, but default strategy is to NEVER reorganize.

  - `goalSignalType`: The reference signal, paired with RSS, XCORR or RMSE global cost function, otherwise ignored. Options are: either an integer from [1, 19] to use predetermined signals, or path to a file with the signal in one column. The length of the signal from the file must correspond planDim option. Default is signal type 1.

  - `setGlobalCostFunc`: The global cost function to be used. Options are (case-sensitive): VAR for variance function, XCORR for negative cross-correlation, RSS for residual sum of squares and RMSE for residual mean square error. Default is VAR. XCORR uses standard normalization by definition, RMSE has its own way of scaling and RSS uses standard normalization by default.

  - `setScaling`: The scaling technique to be used with RSS function, ignored otherwise. Options are (case-sensitive): STD for standard normalization, MIN-MAX for min-max scaling and UNIT-lENGTH for unit-length scaling. Default is STD.

  - `setLocalCostFunc`: The local cost function. Options are (case-sensitive): COST for cost plan score, PREF for preference plan score, which is converted to COST by 1 - PREF, INDEX for plan indicies to be used as costs. Default is COST.

  - `logLevel`: The log level. Options are (case-sensitive): SEVERE, ALL, INFO, WARNING, FINE, FINER, FINEST		

- Explain input: plans, incentive signal, etc.
- How to use/implement a global cost function
- How to use/implement a local cost function
- How to setup and configure a tree topology
- How to collect global optimality results
- How to configure the alpha and beta parameters and what explain what they mean
- How to control termination criteria, 

Logging
--------

- Selected plans (with their local cost)
- Global cost, local cost, fairness over iterations


Advanced Topics:
--------

- How to run experiments with EPOS and COHDA
- How to run live
- How to write your own agent

Other
--------

- Links to datasets.

Software Artifact
--------

- Here you can copy paste the content from the following page: http://epos-net.org/software/exemplar/

Installation
------------

Install $project by running:

    install project

Contribute
----------

- Issue Tracker: github.com/$project/$project/issues
- Source Code: github.com/$project/$project

Support
-------

If you are having issues, please let us know.
We have a mailing list located at: project@google-groups.com

License
-------

The project is licensed under the BSD license.



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
