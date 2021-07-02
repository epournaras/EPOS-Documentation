.. _arguments-chapter:

========================
Configuration Parameters
========================

EPOS has the following configuration parameters in ``epos.properties`` file within ``conf`` directory:

General parameters:

  - ``dataset``: Dataset name to be used. This is the name of the directory located inside ``datasets`` directory that contains all data.

  - ``numIterations``: Number of iterations. Default is 40.

  - ``numChildren``: Number of children of each inner node. Default is 2.

  - ``numAgents``: Number of participating agents. Default is 100.

  - ``numPlans``: The maximum number of possible plans per agent. Lower than this is possible. If more exist in the file, only first ``numPlans`` rows are read. Default is 16.

  - ``planDim``: Dimension of every possible plan of every agent. Must be equal across the agents and must correspond to the dataset. 

  - ``logLevel``: The log level. Options are (case-sensitive): SEVERE, ALL, INFO, WARNING, FINE, FINER, FINEST. The output is printed on the console.

Repetitions and randomness parameters:

  - ``numSim``: Number of repetitions, each with different relative position of the agents in the tree. Default is 1.

  - ``shuffle``: The number of agent-shuffling to make before assigning them to the tree hierarchy. More info on :ref:`tree-topology-chapter`. Default is 0.

  - ``shuffleFile``: The path to a file containing already shuffled agents in one column, no header. Default is null.

  - ``reorganizationSeed``: The seed on which the Random generator for shuffling is based on. Default is 0. For more information, see :ref:`tree-topology-reorganization`.

Parameters controlling the Objectives:

  - ``alpha``: Weight of unfairness in multi-objective function. 0<= ``alpha`` <= 1. Default is 0. For more information, see :ref:`multi-objective-optimization-chapter`.

  - ``beta``: Weight of local cost in multi-objective function. 0<= ``beta`` <= 1. Default is 0. For more information, see :ref:`multi-objective-optimization-chapter`.

  - ``globalCostFunction``: The global cost function to be used. Options are (case-sensitive): ``VAR`` for :ref:`global-cost-function-variance`, ``XCORR`` for :ref:`global-cost-function-cross-correlation`, ``RSS`` for :ref:`global-cost-function-rss` and ``RMSE`` for :ref:`global-cost-function-rmse`. Default is ``VAR``. ``XCORR`` uses :ref:`global-cost-function-scaling-standard` by definition, ``RMSE`` has its own way of scaling and ``RSS`` uses standard normalization by default, but can use others. For more information, see :ref:`global-cost-function-chapter`.

  - ``scaling``: The scaling technique to be used with ``RSS`` function, ignored otherwise. Options are (case-sensitive): ``STD`` for :ref:`global-cost-function-scaling-standard`, ``MIN-MAX`` for :ref:`global-cost-function-scaling-min-max` and ``UNIT-LENGTH`` for :ref:`global-cost-function-scaling-unit-length`. Default is ``STD``. For more information, see :ref:`global-cost-function-scaling`.

  - ``goalSignalPath``: The reference signal, paired with ``RSS``, ``XCORR`` or ``RMSE`` global cost function, otherwise ignored. A path to a file with the signal in one column. The length of the signal from the file must be of size ``planDim``. If shorter, it will be padded in a Round-Robyn manner, if longer, it will be cropped to the size given in ``planDim``. For more information, see :ref:`input-incentive-signals`.

  - ``localCostFunction``: The local cost function. Options are (case-sensitive): ``DISC`` for :ref:`local-cost-function-discomfort`, ``PREF`` for :ref:`local-cost-function-preference`, which is converted to ``DISC`` by 1 - ``PREF``, ``INDEX`` for plan indices to be used as costs (see :ref:`local-cost-function-index`). Default is ``DISC``. For more information, see :ref:`local-cost-function-chapter`.

Dynamic and on-the-fly reorganization parameters:

  - ``strategy``: The strategy for triggering reorganizations on-the-fly. The options include: ``never`` to disable reorganizations, ``periodically`` for :ref:`tree-topology-criterion-periodically`, ``convergence`` for :ref:`tree-topology-criterion-convergence` and ``globalCostReduction`` for :ref:`tree-topology-criterion-reduction`.

  - ``periodically.reorganizationPeriod``: Sets the period of the reorganizations when :ref:`tree-topology-criterion-periodically` strategy is used.

  - ``convergence.memorizationOffset``: Sets the memorization offset when :ref:`tree-topology-criterion-convergence` strategy is used.

  - ``globalCost.reductionThreshold``: Sets the threshold when :ref:`tree-topology-criterion-reduction`  strategy is used.

  - ``strategy.reorganizationSeed``: The random generator seed controlling the randomness in reorganizations. Default is 0.
