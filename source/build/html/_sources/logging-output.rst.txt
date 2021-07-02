.. _logging-chapter:

================
Logging & Output
================

The logging functions in EPOS are extensive and they track, record, and report information about the system at each iteration and also at the end / termination of the simulation. The class implementations can be found at ``agent.logging`` package in the code. In order to initialise and add different loggers to the agent, they need to be added in the ``experiment.MultiObjectiveIEPOSExperiment`` class:

.. code-block:: java

  LoggingProvider<MultiObjectiveIEPOSAgent<Vector>> loggingProvider = new LoggingProvider<MultiObjectiveIEPOSAgent<Vector>>();        
  GlobalCostLogger<Vector>              GCLogger  = new GlobalCostLogger<Vector>(Configuration.getGlobalCostPath());
  LocalCostMultiObjectiveLogger<Vector> LCLogger  = new LocalCostMultiObjectiveLogger<Vector>(Configuration.getLocalCostPath());
  TerminationLogger<Vector>             TLogger   = new TerminationLogger<Vector>(Configuration.getTerminationPath());
  SelectedPlanLogger<Vector>            SPLogger  = new SelectedPlanLogger<Vector>(Configuration.getSelectedPlansPath(), config.numAgents);
  GlobalResponseVectorLogger<Vector>    GRVLogger = new GlobalResponseVectorLogger<Vector>(Configuration.getGlobalResponsePath());
  PlanFrequencyLogger<Vector>           DstLogger = new PlanFrequencyLogger<Vector>(Configuration.getDistributionPath());
  UnfairnessLogger<Vector>              ULogger   = new UnfairnessLogger<Vector>(Configuration.getUnfairnessPath());
  GlobalComplexCostLogger<Vector>       GCXLogger = new GlobalComplexCostLogger<Vector>(Configuration.getGlobalComplexCostPath());
  WeightsLogger<Vector>                 WLogger   = new WeightsLogger<Vector>(Configuration.getWeightsPath());
  ReorganizationLogger<Vector>          RLogger   = new ReorganizationLogger<Vector>(Configuration.getReorganizationPath());
        
  GCLogger.setRun(Configuration.permutationID);
  LCLogger.setRun(Configuration.permutationID);
  TLogger.setRun(Configuration.permutationID);        
  SPLogger.setRun(Configuration.permutationID);
  GRVLogger.setRun(Configuration.permutationID);
  DstLogger.setRun(Configuration.permutationID);
  ULogger.setRun(Configuration.permutationID);
  GCXLogger.setRun(Configuration.permutationID);
  WLogger.setRun(Configuration.permutationID);
  RLogger.setRun(Configuration.permutationID);

        
  loggingProvider.add(GCLogger);
  loggingProvider.add(LCLogger);
  loggingProvider.add(TLogger);
  loggingProvider.add(SPLogger);
  loggingProvider.add(GRVLogger);
  loggingProvider.add(DstLogger);
  loggingProvider.add(ULogger);
  loggingProvider.add(GCXLogger);
  loggingProvider.add(WLogger);
  loggingProvider.add(RLogger);


In order to add / develop new loggers to EPOS, the ``GlobalCostLogger`` template is recommended. In addition to this, each logger, when their ``print`` function is called, they write their data on output files with address defined in ``config.Configuration`` class.

Global Complex Cost Logger
==========================

The global complex cost logger is implemented in ``agent.logging.GlobalComplexCostLogger.java`` and by default, it record the complex cost defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``global-complex-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **complex cost** of the whole system, at each iteration, and across multiple runs. In addition, it shows the mean and standard deviation of the cost across different runs. By default, the *complex cost* (:ref:`multi-objective-optimization-chapter`) is defined as:

.. math::

   \gamma \cdot G^{(t)} + \alpha \cdot U^{(t)} + \beta \cdot L^{(t)}

.. figure:: GCC.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

Global Cost Logger
==================

The global cost logger is implemented in ``agent.logging.GlobalCostLogger.java`` and by default, it records the global cost defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``global-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing the **global cost of the whole system, at each iteration, and across multiple runs**. In addition, it shows the mean and the standard deviation of the global cost across different runs. If no specific global cost logger is defined at the ``config.Configuration`` then EPOS assigns ``GlobalComplexCostLogger<Vector> GCXLogger`` as the global cost logger.

.. figure:: GC.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

.. figure:: MULTI-OBJECTIVE-ALL-TOGETHER-ENERGY.png
   :scale: 70 %
   :alt: alternate text
   :align: center
   

Global Response Logger
======================

The global response logger is implemented in ``agent.logging.GlobalResponseCostLogger.java`` and by default, it records the global response defined in :ref:`global-cost-function-chapter`. The output file is named ``global-response-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing **global response** of the system, i.e. the sum of all selected plans, for every EPOS repetition and in every iteration.

.. figure:: GR.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

.. figure:: RSS-UNIT-LENGTH-PNW-EVENING-SHUFFLE-GENERATION-FAILURE-BOUNDS.png
   :scale: 70 %
   :alt: alternate text
   :align: center   

Indexed Histogram Logger
========================

The index histogram logger is implemented in ``agent.logging.PlanFrequencyLogger.java`` and by default, it records the plan frequency defined in :ref:`input-plans`. The output file is named ``indexes-histogram.csv`` and can be found in this path: ``~/output/``. The .csv file is an matrix showing **how many times each plan was selected** at the end of each run / simulation.

.. figure:: IH.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

Local Cost Logger
=================

The local cost logger is implemented in ``agent.logging.LocalCostLogger.java`` and by default, it records the local cost defined in :ref:`local-cost-function-chapter`. The output file is named ``local-cost.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing **local cost of all agents** at each iteration and for every EPOS repetition. In addition, it shows the mean and the standard deviation of the cost across different runs.

.. figure:: LC.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

Selected Plans
==============

The selected plan logger is implemented in ``agent.logging.SelectedPlanLogger.java`` and by default, it records the selected plan index defined in :ref:`input-plans`. The output file is named ``selected-plan.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing **selected plan index of each agent** at each iteration and in every repetition.


.. figure:: SP.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

Termination Logger
==================

The termination logger is implemented in ``agent.logging.TerminationLogger.java`` and by default, it records the terminal iteration defined in :ref:`termination-criteria-chapter`. The output file is named ``termination.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing **at which iteration EPOS terminated** depending on the termination criteria.


.. figure:: TR.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

Unfairness Logger
=================

The unfairness logger is implemented in ``agent.logging.UnfairnessCostLogger.java`` and by default, it record the unfairness defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``unfairness.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing the **unfairness of the whole system, at each iteration, and across multiple runs**. In addition, it shows the mean and standard deviation of the cost across different runs.


.. figure:: Un.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

Reorganization Logger
=====================
This logger is implemented in ``agent.logging.ReorganizationLogger.java``. For each EPOS run, the logger creates a new column that contains the cumulative number of reorganizations until the iteration index of the first column. More regarding reorganization and tree topology of EPOS can be found at :ref:`tree-topology-reorganization`.

.. figure:: NR.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center

.. figure:: TREE-RESTRUCTURING-REORGANIZATIONS-PER-THRESHOLD-GAUSSIAN.png
   :scale: 70 %
   :alt: alternate text
   :align: center   

Weights, Alpha, Beta Logger
===========================

The weights logger is implemented in ``agent.logging.WeightsLogger.java`` and by default, it records the weights defined in :ref:`multi-objective-optimization-chapter`. The output file is named ``weights-alpha-beta.csv`` and can be found in this path: ``~/output/``. The .csv file is a matrix showing :math:`\alpha`, :math:`\beta` and :math:`\gamma = 1 - \alpha - \beta` for every iteration.


.. figure:: WE.pdf.svg
   :scale: 100 %
   :alt: alternate text
   :align: center
