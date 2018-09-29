.. _input-chapter:

=====
Input
=====

.. _input-plans:

Plans
=====


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



