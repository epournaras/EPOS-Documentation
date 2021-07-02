.. _arguments-chapter:

======================
Command Line Arguments
======================

EPOS has the following command line arguments:


  - ``h``: show helptext

  - ``dataset``: Dataset name to be used. This is the name of the directory located inside datasets/ directory thta contains all data. No default value, must be set

  - ``numSim``: Number of simulations. Default is 1.

  - ``numIterations``: Number of iterations. Default is 40.

  - ``lambda``: Lambda preference level. Lambda > 0. Used only for old I-EPOS. Default is 0.

  - ``alpha``: Weight of unfairness in multi-objective function. 0<= Alpha <= 1. Default is 0. For more information, see :ref:`multi-objective-optimization-chapter`.

  - ``beta``: Weight of local cost in multi-objective function. 0<= Beta <= 1. Default is 0. For more information, see :ref:`multi-objective-optimization-chapter`.

  - ``numAgents``: Number of participating agents. Default is 100.

  - ``numPlans``: The maximum number of possible plans per agent. Lower than this is possible. If more exist in the file, only first numPlan rows are read. Default is 16.

  - ``planDim``: Dimension of every possible plan of every agent. Must be equal across the agents and must correspond to the dataset. Default to -1 and must be set!

  - ``numChildren``: Number of children of each inner node. Default is 2.

  - ``shuffle``: The number of the shuffles to make before assigning agents to the tree hierarchy. Default is 0.

  - ``shuffleFile``: The path to a file containing already shuffled agents in one column, no header. Default is null.

  - ``reorganizationSeed``: The seed to be used in Random generator the shuffling is based on. Default is 0. For more information, see :ref:`tree-topology-reorganization`.

  - ``enableNEVERstrategy``: Enabling reorganization strategy NEVER. Works only for ModifiableIEPOSAgent. This is default. For more information, see :ref:`tree-topology-reorganization` and :ref:`tree-topology-criterion-never`.

  - ``enablePERIODICALLYstrategy``: Enabling reorganization strategy PERIODICALLY with indicated period. Default period is 3, but default strategy is to NEVER reorganize. For more information, see :ref:`tree-topology-reorganization` and :ref:`tree-topology-criterion-periodically`.

  - ``enableCONVERGENCEstrategy``: Enabling reorganization strategy ON_CONVERGENCE with indicated memorization offset. Default is 5, but default strategy is to NEVER reorganize. For more information, see :ref:`tree-topology-reorganization` and :ref:`tree-topology-criterion-convergence`.

  - ``enableGLOBALCOSTREDUCTIONstrategy``: Enabling reorganization strategy based on GLOBAL_COST_REDUCTION strategy with indicated tolerance level. 0 <= Tolerane Level <= 1. Default is 0, but default strategy is to NEVER reorganize. For more information, see :ref:`tree-topology-reorganization` and :ref:`tree-topology-criterion-reduction`.

  - ``goalSignalType``: The reference signal, paired with RSS, XCORR or RMSE global cost function, otherwise ignored. Options are: either an integer from [1, 19] to use predetermined signals, or path to a file with the signal in one column. The length of the signal from the file must correspond planDim option. Default is signal type 1. For more information, see :ref:`input-incentive-signals`.

  - ``setGlobalCostFunc``: The global cost function to be used. Options are (case-sensitive): VAR for :ref:`global-cost-function-variance`, XCORR for :ref:`global-cost-function-cross-correlation`, RSS for :ref:`global-cost-function-rss` and RMSE for :ref:`global-cost-function-rmse`. Default is VAR. XCORR uses :ref:`global-cost-function-scaling-standard` by definition, RMSE has its own way of scaling and RSS uses standard normalization by default, but can use others. For more information, see :ref:`global-cost-function-chapter`.

  - ``setScaling``: The scaling technique to be used with RSS function, ignored otherwise. Options are (case-sensitive): STD for :ref:`global-cost-function-scaling-standard`, MIN-MAX for :ref:`global-cost-function-scaling-min-max` and UNIT-LENGTH for :ref:`global-cost-function-scaling-unit-length`. Default is STD. For more information, see :ref:`global-cost-function-scaling`.

  - ``setLocalCostFunc``: The local cost function. Options are (case-sensitive): COST for :ref:`local-cost-function-discomfort`, PREF for :ref:`local-cost-function-preference`, which is converted to COST by 1 - PREF, INDEX for plan indicies to be used as costs (see :ref:`local-cost-function-index`). Default is COST. For more information, see :ref:`local-cost-function-chapter`.

  - ``logLevel``: The log level. Options are (case-sensitive): SEVERE, ALL, INFO, WARNING, FINE, FINER, FINEST