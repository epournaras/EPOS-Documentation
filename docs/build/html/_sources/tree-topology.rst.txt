.. _tree-topology-chapter:

=============
Tree Topology
=============

EPOS organises the agents in a tree structure. In ``config.Configuration`` class, the specifics of the tree is by default defined using the following:

.. code-block:: java
      :caption: config.Configuration.java
      :name: tree-initiation

      public static RankPriority priority = RankPriority.HIGH_RANK;
      public static DescriptorType rank = DescriptorType.RANK;
      public static TreeType type = TreeType.SORTED_HtL;
      public static BalanceType balance = BalanceType.WEIGHT_BALANCED;
      public static int numChildren = 2;

This creates a balance binary tree which sorts the agents based on their rank. If you need to change the topology or modify certain aspects of it check out ``util.TreeArchitecture.java``. 

In addition, each agent in EPOS has two classes^* which take care of the tree communication and keep track of the parent and childern nodes addresses: ``TreeAgent extends Agent<V> implements TreeApplicationInterface`` and ``IterativeTreeAgent extends TreeAgent``. In ``TreeAgent`` the most important functions are the ``public void setTreeView(Finger parent, List<Finger> children)`` and ``public void setParent(Finger parent)`` and ``public void setChildren(List<Finger> list)`` which are called when the ``TreeArchitecture`` is finished bootstrapping and creating the tree. In ``IterativeTreeAgent`` the most important function is the ``public void handleIncomingMessage(Message message)`` which is called whenever a new message is receive by the agent. Here you can track the *to-down* and *bottom-up* phases of EPOS.

.. _tree-topology-reorganization:

Tree Reorganization
===================

EPOS is capable of randomly changing the relative position of agents in the tree hierarchy dynamically and on-the-fly during runtime. In other words, the reorganization means that each agent gets assigned a new set of children and a new parent. Note, however, that the overall tree topology is not changed, i.e. if the original hierarchy was balanced binary tree, EPOS ensures it remains balanced binary tree after the reorganization. The reorganization is employed as means of improving the learning capacity of the agents by allowing them to explore new combinations of plans.

When certain criterion is met, the reorganization is triggered. Consequently, a new random relative position of agents within the original tree topology is constructed and each agent receives a new set of children and a new parent. Implementation-wise, the criterion is always evaluated by the current root agent. The current root agent is the one that generates new random relative position of agents within the hierarchy and is the one that notifies the agents that the reorganization is taking place by first notifying its children which then propagate the notification down their subtrees. Note that the actual reorganization takes place during the top-down phase. When an agent receives a notification that the reorganziation is taking place, it sends a request message to the current root, and it responds with a message containing the adresses of new children and a new parent. The reorganization also clears all current and previous selections, responses from subtree and indicator variables.

The reproducability of experiments in terms of randomness in selecting new relative position of agents when the reorganization is triggered can be controlled via a *random generator seed*. It can be set via one of the following two ways (the example shows setting the seed to 0):

1. either setting ``strategy.reorganizationSeed=0``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: reorganization-seed-configuration-java

      public long reorganizationSeed = 0;

The class encapsulating the reorganization functionality is ``agent.ModifiableIeposAgent<V extends DataType<V>>``. This class handles request and response messages exchanged to obtain a new set of children and a new parent. It also handles various reorganization criteria: :ref:`tree-topology-criterion-periodically`, :ref:`tree-topology-criterion-convergence` and :ref:`tree-topology-criterion-reduction`. 

.. _tree-topology-criterion-periodically:

Reorganization Criterion: Periodically
--------------------------------------

The periodical criterion triggers the reorganization every X iterations. It can be enabled in two ways (the example shows the period of 3):

1. either setting ``strategy=periodically`` and ``periodically.reorganizationPeriod=3``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: reorganization-periodically-configuration-java

      public ReorganizationStrategyType reorganizationType = 
                                        ReorganizationStrategyType.PERIODICALLY;
      public int reorganizationPeriod = 3;

Note that the period of reorganizations must be an integer value.

.. _tree-topology-criterion-convergence:

Reorganization Criterion: Convergence
-------------------------------------

Convergence is detected when the value of the global cost remains unchanged in two consecutive iterations. Note that the current root agent is the one that can check if the system converged. When the system converges, new combinations of selected plans cannot be explored, so the reorganization is triggered. The idea is to revert the state of the system to some previous, memorized state, that was *far enough* from this local minimum. Memorizing a state means that every agent memorizes its own selected plan. Intuitively, reverting the state and the new relative position of agents should be able to explore a new set of plan combinations, hopefully finding a combination that results in a lower global cost. A parameter called *offset* controls when the state is memorized. More precisely, if the value of the global cost is equal at iterations :math:`t-1` and :math:`t`, then convergence is detected at iteration :math:`t`, the reorganization happens between iterations :math:`t` and :math:`t+1`, and at iteration :math:`t+1` the agents select the plans they have previously memorized. Finally, the new memorization occurs at iteration :math:`t+1+` *offset*. Note that reverting to the previously memorized state discards local minimum found. Also, note that the value of the global cost at the iteration in which plans were memorized and at the iteration after the reorganization are equal.

The convergence reorganization criterion can be enabled in two ways (the example shows the *offset* of 3):

1. either setting ``strategy=convergence`` and ``convergence.memorizationOffset=3``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: reorganization-convergence-configuration-java

      public ReorganizationStrategyType reorganizationType = 
                                        ReorganizationStrategyType.CONVERGENCE;
      public int reorganizationOffset = 3;

Note that the offset must be an integer value.

.. _tree-topology-criterion-reduction:

Reorganization Criterion: Global Cost Reduction
-----------------------------------------------

The :ref:`tree-topology-criterion-convergence` has one main limitation: all progress EPOS made between the iteration at which the selections are memorized and the iteration at which convergence is detected is discarded after the reorganization. This criterion addresses this problem by allowing the system to trigger reorganizations before converging, hence decreasing the runtime. The idea is that the reorganization should be triggered when the system is *too close* to the the local minimum, which is measured by the reduction in the global cost in two consecutive iterations:

.. math::
   :label: equation-global-cost-reduction

   \frac{G^{(t-1)} - G^{(t)}}{G^{(t-1)}},

where :math:`G^{(t-1)}` and :math:`G^{(t)}` represent the values of the global cost in two consecutive iterations :math:`t-1` and :math:`t`. Intuitively, when the relative global cost reduction is *low*, the system is close to reaching the local minimum, but if it is *high*, the system takes long steps towards it. More precisely, the reorganization is triggered when the relative global cost reduction in Equation :eq:`equation-global-cost-reduction` **drops** below a certain *threshold*. At the iteration immediately after the reorganization, the agents make the same selections as at the iteration prior to the reorganization. 

The reorganization criterion based on the global cost reduction can be enabled in two ways (the example shows the *threshold* of 0.5):

1. either setting ``strategy=globalCostReduction`` ``globalCost.reductionThreshold=0.5``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: reorganization-reduction-configuration-java

      public ReorganizationStrategyType reorganizationType = 
                                        ReorganizationStrategyType.GLOBAL_COST_REDUCTION;
      public double convergenceTolerance = 0.5;

Note that the threshold must be a double value between 0 and 1. If the *threshold* is 0, no reorganization happens, since the relative global cost reduction can never be strictly lower than 0. If the *threshold* is 1, then reorganizations are guaranteed to happen since the relative global cost reduction is always lower than 1.

.. _tree-topology-criterion-never:

Reorganization Criterion: Never
-------------------------------

By default, reorganizations are disabled. Externally, it can be done in one of the following two ways:

1. either setting ``strategy=never``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: reorganization-never-configuration-java

      public ReorganizationStrategyType reorganizationType = 
                                        ReorganizationStrategyType.NEVER;
