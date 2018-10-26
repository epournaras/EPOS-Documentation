.. _input-chapter:

=====
Input
=====

.. _input-plans:

Plans
=====

Plans are the representation of agents' possible actions. In EPOS, the plans are in form of n-dimensional vectors, which are read from the datasets. In each dataset, each agent has a certain number of plans, and each plan will have a certain of dimensions. A typical plan format is:

 *planScore* : *planDimension1*, ... , *planDimensionN*

in which the plan score can be:

1. discomfort (:ref:`local-cost-function-discomfort`) of the plan, or
2. preference (:ref:`local-cost-function-preference`) of the plan


The possible dataset formats are defined in ``agent.dataset`` package in the code. In addition, there is a Gaussian plan generator in ``agent.dataset.GaussianDataSet.java``. For this dataset, you can define the *number of agents*, *number of plans*, and *number of dimensions* using the configuration or the :ref:`arguments-chapter`.

In addition, at each iteration of EPOS, agents select their preliminary plans and report them to their parent. The implemented classes for this can be found at ``agent.planselection`` package. The main class is ``MultiObjectiveIeposPlanSelector implements PlanSelector<MultiObjectiveIEPOSAgent>`` and the main function is:

.. code-block:: java

      int id =  agent.getOptimization().argmin(agent.getGlobalCostFunction(), 
                                               agent.getLocalCostFunction(),
                                               agent.getPossiblePlans(), 
                                               otherResponse, 
                                               agent.getUnfairnessWeight(),
                                               agent.getLocalCostWeight(),
                                               currentDiscomfortSum,
                                               currentDiscomfortSumSqr,
                                               agent.getNumAgents(),
                                               agent);

which return the id of the selected local plan. There are different ways of selecting the local plan, however by default EPOS chooses the plan with the ``PlanSelectionOptimizationFunctionCollection.complexFunction1``. The function works as the following: :math:`(1 - \alpha - \beta) \cdot global\_cost + \alpha \cdot unfairness + \beta \cdot local\_cost`, the details of which can be found in :ref:`multi-objective-optimization-chapter`.

.. _input-incentive-signals:

Incentive Signals
=================

EPOS is able to guide the selections of agents towards a certain predefined goal. The goal is given as a reference incentive signal, and the agents make selections towards the best matching between their joint selections and the reference signal. As EPOS is a minimization algorithm, the objective is the minimization of *dissimilarity* between them. For more information on the dissimilarity measures, please see :ref:`global-cost-function-chapter`. The incentive signal is taken into account only if one of the following global cost functions are employed: :ref:`global-cost-function-cross-correlation`, :ref:`global-cost-function-rss` or :ref:`global-cost-function-rmse`.

The Incentive signal is used within EPOS via the parameter ``goalSignalPath`` by setting **a path** to a file containing the incentive signal. If the file path is not provided, a first ".target" file in *dataset* directory will be used as a goal signal. If none ".target" files are found, a *zero* goal signal is constructed (a signal filled with zeros).

Note that the incentive signal **must** be of the same length as plans. If it is shorter than the ``planDim`` parameter, it will be padded at the end in a Round-Robyn fashion, if longer, it will be cropped. The signal is given in a column, one value in each row. The file **cannot** have any headers or more than one column.

For example, the following are correct ways of providing the incentive signal to EPOS:

.. code-block:: java

   goalSignalPath="/home/user/Documents/EPOS-project/datasets/incentive-signal.csv"   // full path
   goalSignalPath="datasets/incentive-signal.csv"                                     // relative path

or in ``Configuration.java`` class:

.. code-block:: java
   :caption: Configuration.java
   :name: goal-signal-configuration-java

   public static String	goalSignalFilename = "/home/user/Documents/EPOS-project/datasets/incentive-signal.csv";  // or
   public static String	goalSignalFilename = "datasets/incentive-signal.csv";


And these are the incorrect ways:

.. code-block:: java

   goalSignalPath="datasets"                                                      // the name of the directory containing the file is invalid
   goalSignalPath="/home/user/Documents/EPOS-project/datasets/incentive-signal"   // the file extension must be specified



