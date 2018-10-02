.. _input-chapter:

=====
Input
=====

.. _input-plans:

Plans
=====
Plans are the representation of agents' possible actions. In EPOS, the plans are in form of n-dimensional vectors, which are read from the datasets. In each dataset, each agent has a certain number of plans, and each plan will have a certain of dimensions. A typical plan format is ``_planScore:_planDimension1, ... ,_planDimensionN``, in which the plan score can be the local cost of the plan or the preference of the user when selecting plans. The possible dataset formats are defined in ``agent.dataset`` package in the code. In addition, there is a gauusian plan generator in ``agent.dataset.GaussianDataSet.java``. For this dataset, you can define the *number of agents*, *number of plans*, and *number of dimensions* using the configuration or the :ref:`arguments-chapter`.

In addition, at each iteration of EPOS, agents select their preliminary plans and report them to their parent. the implemented classes for this can be found at ``agent.planselection`` package. The main class is ``MultiObjectiveIeposPlanSelector implements PlanSelector<MultiObjectiveIEPOSAgent> and the main function is:

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

which return the id of the selected local plan. There are different ways of selecting the local plan, however by default EPOS choses the plan with the ``PlanSelectionOptimizationFunctionCollection.complexFunction1``. The function works as the following: :math:`(1 - alpha - beta) * global-cost + alpha * unfairness + beta * local-cost`, the details of which can be found in :ref:`multi-objective-optimization-chapter`.

.. _input-incentive-signals:

Incentive Signals
=================

EPOS is able to guide the selections of agents towards a certain predefined goal. The goal is given as a reference incentive signal, and the agents make selections towards the best matching between their joint selections and the reference signal. As EPOS is minimization algorithm, the objective is minimizing *dissimilarity* between them. For more informaiton on dissimilarity measures, please see :ref:`global-cost-function-chapter`. The incentive signal is taken into account only if one of the following global cost functions are employed: :ref:`global-cost-function-cross-correlation`, :ref:`global-cost-function-rss` or :ref:`global-cost-function-rmse`.

Incentive signal is forwarded to EPOS via command line argument ``-goalSignalType``. This can be followed by:

1. either an integer number between 1 and 19 specifying one of the **synthetic predefined signals**, or

2. **a path** to a file containing the incentive signal.

Note that the incentive signal **must** be of the same length as plans. Synthetic predefined incentive signals are constructed with respect to the length of the plans (also forwarded via ``-planDim`` argument). However, a user must ensure that the length of the incentive signal in the file corresponds to the length of the plans.

Synthetic incentive signals are given in the class ``GoalSignalsCollection`` in package ``func.goalsignals``, and include the following: 

- **sine signals** labeled with numbers 1, 2, and 3
- **single impulse signals** labeled with integers from 4 to 10
- **monotonically decreasing step-like signals** labeled with integers from 11 to 13
- **monotonically increasing step-like signals** labeled with integers from 14 to 17
- **double impulse signal** labeled with 18
- **mixture of two Gaussian curves signal** labeled with 19

User-defined incentive signals can be provided to EPOS in a file. The signal is given in a column, one value in each row. The file **cannot** have any headers or more than one column. The number of rows in a file **must** correspond to the length of the plans. If more values are provided, EPOS will throw ``IndexOutOfBoundsException``. The *full* path to the file containing the incentive signal is forwarded to EPOS via ``-goalSignalType`` command line argument.

For example, the following are correct ways of providing the incentive signal to EPOS:

.. code-block:: java

   -goalSignalType 19
   -goalSignaltype "/home/user/Documents/EPOS-project/datasets/incentive-signal.csv"   // full path
   -goalSignaltype "datasets/incentive-signal.csv"                                     // relative path

or in ``Configuration.java`` class:

.. code-block:: java
   :caption: Configuration.java
   :name: goal-signal-configuration-java

   public static String	goalSignalFilename = "/home/user/Documents/EPOS-project/datasets/incentive-signal.csv";  // or
   public static String	goalSignalFilename = "datasets/incentive-signal.csv";


And these are the incorrect ways:

.. code-block:: java

   -goalSignalType -5                                                              // negative number
   -goalSignalType 20                                                              // allowed values are integers between 1 and 19
   -goalSignaltype "datasets"                                                      // the name of the directory containing the file is invalid
   -goalSignaltype "/home/user/Documents/EPOS-project/datasets/incentive-signal"   // the file extension must be specified



