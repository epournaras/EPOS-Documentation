.. _logging-chapter:

================
Logging & Output
================

The logging functions in EPOS are expansive and they track, record, and report information about the system at each iteration and also at the end / termination of the simulation. The class implementations can be found at ``agent.logging`` package in the code. In order to initialise and add different loggers to the agnet, they need to be added in the ``experiment.MultiObjectiveIEPOSExperiment`` class:

.. code-block:: java

	LoggingProvider<MultiObjectiveIEPOSAgent<Vector>> loggingProvider = new LoggingProvider<MultiObjectiveIEPOSAgent<Vector>>();        
        GlobalCostLogger<Vector> GCLogger = new GlobalCostLogger<Vector>(Configuration.getGlobalCostPath());
        LocalCostMultiObjectiveLogger<Vector> LCLogger = new LocalCostMultiObjectiveLogger<Vector>(Configuration.getLocalCostPath());
        TerminationLogger<Vector> TLogger = new TerminationLogger<Vector>(Configuration.getTerminationPath());
        SelectedPlanLogger<Vector> SPLogger = new SelectedPlanLogger<Vector>(Configuration.getSelectedPlansPath(), config.numAgents);
        GlobalResponseVectorLogger<Vector> GRVLogger = new GlobalResponseVectorLogger<Vector>(Configuration.getGlobalResponsePath());
        PlanFrequencyLogger<Vector> DstLogger =	new PlanFrequencyLogger<Vector>(Configuration.getDistributionPath());
        UnfairnessLogger<Vector> ULogger = new UnfairnessLogger<Vector>(Configuration.getUnfairnessPath());
        GlobalComplexCostLogger<Vector> GCXLogger = new GlobalComplexCostLogger<Vector>(Configuration.getGlobalComplexCostPath());
        WeightsLogger<Vector> WLogger =	new WeightsLogger<Vector>(Configuration.getWeightsPath());
        
        
        GCLogger.setRun(Configuration.permutationID);
        LCLogger.setRun(Configuration.permutationID);
        TLogger.setRun(Configuration.permutationID);        
        SPLogger.setRun(Configuration.permutationID);
        GRVLogger.setRun(Configuration.permutationID);
        DstLogger.setRun(Configuration.permutationID);
        ULogger.setRun(Configuration.permutationID);
        GCXLogger.setRun(Configuration.permutationID);
        WLogger.setRun(Configuration.permutationID);
        
        loggingProvider.add(GCLogger);
        loggingProvider.add(LCLogger);
        loggingProvider.add(TLogger);
        loggingProvider.add(SPLogger);
        loggingProvider.add(GRVLogger);
        loggingProvider.add(DstLogger);
        loggingProvider.add(ULogger);
        loggingProvider.add(GCXLogger);
        loggingProvider.add(WLogger);

In order to add / develop new loggers to EPOS, the ``GlobalCostLogger`` template is recommended. In addition to this, each logger, when their ``print`` function is called, they write their data on output files with address defined in ``config.Configuration`` class.

Global Complex Cost Logger
==========================

The global compelex cost logger is implemented in ``agent.logging.GlobalComplexCostLogger.java`` and by default, it record the complex cost defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``global-complex-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **complex cost** of the whole system, at each iteration, and accross multiple runs. In addition, it shows the mean and standard deviation of the cost across different runs. By default, the *complex cost* is defined as:

.. math::

   \gamma \cdot G^{(t)} + \alpha \cdot U^{(t)} + \beta \cdot L^{(t)}

Global Cost Logger
==================

The global compelex cost logger is implemented in ``agent.logging.GlobalCostLogger.java`` and by default, it record the complex cost defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``global-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **ost of the whole system, at each iteration, and accross multiple runs**. In addition, it shows the mean and standard deviation of the cost across different runs. If no specific global cost logger is defined at the ``config.Configuration`` then EPOS assigns ``GlobalComplexCostLogger<Vector> GCXLogger`` as the global cost logger.

Global Response Logger
======================

The global compelex response logger is implemented in ``agent.logging.GlobalResponseCostLogger.java`` and by default, it record the global response defined in :ref:`global-cost-function-chapter`. The output file is named ``global-response-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **aggregated response** of the system at the root which is the sum of all plans. The matrix shows the aggregated response across all dimensions of the plan.

Indexed Histogram Logger
========================

The global compelex cost logger is implemented in ``agent.logging.PlanFrequencyLogger.java`` and by default, it record the plan frequency defined in :ref:`input-plans`. The output file is named ``indexes-histogram.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **how many times each plan was selected** at the end of each run / simulation.

Local Cost Logger
=================

The global compelex cost logger is implemented in ``agent.logging.LocalCostLogger.java`` and by default, it record the local cost defined in :ref:`local-cost-function-chapter`. The output file is named ``local-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **local cost of all agents at each iteration**. In addition, it shows the mean and standard deviation of the cost across different runs.

Selected Plans
==============

The global compelex cost logger is implemented in ``agent.logging.SelectedPlanLogger.java`` and by default, it record the selected plan index defined in :ref:`input-plans`. The output file is named ``selected-plan.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **selected plan index of each agent at each iteration**.

Termination Logger
==================

The global compelex cost logger is implemented in ``agent.logging.TerminationLogger.java`` and by default, it record the complex cost defined in :ref:`termination-criteria-chapter`. The output file is named ``termination.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **at which iteration EPOS terminated** depending on the termination criteria.

Unfairness Logger
=================

The global compelex cost logger is implemented in ``agent.logging.UnfairnessCostLogger.java`` and by default, it record the unfairness defined in **??**. The output file is named ``unfairness.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **unfairness of the whole system, at each iteration, and accross multiple runs**. In addition, it shows the mean and standard deviation of the cost across different runs.

Weights, Alpha, Beta Logger
===========================

The global compelex cost logger is implemented in ``agent.logging.WeightsLogger.java`` and by default, it record the complex cost defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``weights-alpha-beta.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **weights, alpha, and beta of each agent** of the whole system. In addition, it shows the mean and standard deviation of the cost across different runs.