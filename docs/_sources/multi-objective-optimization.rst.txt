.. _multi-objective-optimization-chapter:

============================
Multi-Objective Optimization
============================

EPOS is capable of optimizing three different objectives:

1. The **global objective** is abstracted and expressed by a global cost function :math:`g(\cdot)` (Fore more information, see :ref:`global-cost-function-chapter`), that takes as an argument the global response, :math:`\mathbb{g}` (the sum of all selected plans of the agents, see :ref:`input-plans`). The value of the global objective is called *global cost* and is computed as follows:

.. math::
   :label: equation-global-cost

   G^{(t)} = g(\mathbb{g}^{(t)}), \quad G^{(t)} \in \mathbb{R}.


2. The **local objective** is formulated as minimizing the average cost of all selected plans. The value of the local objective is called *local cost* and its value is computed as follows:

.. math::
   :label: equation-local-cost

   L^{(t)} = \frac{1}{N} \sum_{i = 1}^{N} l(s_{i}^{(t)}), \quad L^{(t)} \in \mathbb{R},

where :math:`l(\cdot)` is the local cost function (see :ref:`local-cost-function-chapter`) that extracts the cost of the selected plan :math:`s_{i}` of the agent :math:`i` at iteration :math:`t`. The number of agents in the hierarchy is denoted with :math:`N`.

3. The **unfairness** objective is formulated as minimizing the standard deviation of the costs of the selected plans. Note that the system is considered fair if all agents select the plans with approximately equal cost. It is formulated as follows:

.. math::
   :label: equation-unfairness

   U^{(t)} = \sqrt{\frac{1}{N} \sum_{i = 1}^{N} \left(l(s_{i}^{(t)}) - L^{(t)}\right)^{2}},  \quad U^{(t)} \in \mathbb{R},

where :math:`l(s_{i}^{(t)})` denotes the cost of the plan :math:`s_{i}^{(t)}` selected by agent :math:`i` at iteration :math:`t`.

EPOS linearly combines the three objectives in a weighted sum of objectives, formulated as follows:

.. math::
   :label: equation-scalarized-objective

   \gamma \cdot G^{(t)} + \alpha \cdot U^{(t)} + \beta \cdot L^{(t)}

where :math:`\gamma, \alpha, \beta \in [0, 1]` and it *must* always hold :math:`\gamma + \alpha + \beta = 1`. The higher value of the weight expresses stronger preference towards minimizing the corresponding objective. When the value of the weight is 0, the corresponding objective is not optimized. The scalarization of the objectives before combining them in the weighted sum is subject of future work. Depending on the data, users are encouraged to implement their own objective-scalarization scheme(s).

By default :math:`\alpha = \beta = 0`, which means that only the global objective is optimized. The weights can be set in one of the following two ways (the example shows setting :math:`\alpha=0.2` and :math:`\beta=0.3`):

1. either setting ``alpha=0.2`` and ``beta=0.3``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: alpha-beta-configuration-java

      public double alpha = 0.2;
      public double beta = 0.3;

Note that :math:`\gamma` is always computed as :math:`\gamma = 1 - \alpha - \beta`. This example can be interpreted as follows: *the global objective is optimized with preference 0.5, the local objective is optimized with preference 0.3 and the unfairness is optimized with preference 0.2*.

The agent that is capable of optimizing multiple objectives is implemented in class ``agent.MultiObjectiveIEPOSAgent<V extends DataType<V>>``. 
   

