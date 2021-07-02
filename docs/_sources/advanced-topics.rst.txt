.. _advanced-topics-chapter:

===============
Advanced Topics
===============

.. _tree-topology-reorganization:

Tree Reorganization
===================

EPOS is capable of randomly changing the relative position of agents in the tree hierarchy dynamically and on-the-fly during runtime. In other words, the reorganization means that each agent gets assigned a new set of children and a new parent. Note, however, that the overall tree topology is not changed, i.e. if the original hierarchy was balanced binary tree, EPOS ensures it remains balanced binary tree after the reorganization. The reorganization is employed as means of improving the learning capacity of the agents by allowing them to explore new combinations of plans.

When certain criterion is met, the reorganization is triggered. Consequently, a new random relative position of agents within the original tree topology is constructed and each agent receives a new set of children and a new parent. Implementation-wise, the criterion is always evaluated by the current root agent. The current root agent is the one that generates new random relative position of agents within the hierarchy and is the one that notifies the agents that the reorganization is taking place by first notifying its children which then propagate the notification down their subtrees. Note that the actual reorganization takes place during the top-down phase. When an agent receives a notification that the reorganization is taking place, it sends a request message to the current root, and it responds with a message containing the addresses of new children and a new parent. The reorganization also clears all current and previous selections, responses from subtree and indicator variables.

The reproducibility of experiments in terms of randomness in selecting new relative position of agents when the reorganization is triggered can be controlled via a *random generator seed*. It can be set via one of the following two ways (the example shows setting the seed to 0):

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


How to run experiments with EPOS and COHDA
==========================================

How to write your own agent
===========================

EPOS Live with Dynamics (FGCS paper)
====================================

The section illustrates the steps needed to replicate the experiments in the paper **A self-integration testbed for decentralized socio-technical systems** publised at `Future Generation Computing Systems <https://www.sciencedirect.com/science/article/pii/S0167739X20303435?via%3Dihub>`_.

The sequence diagram for bootstrapping and managing the performed experiments is as follows:

.. figure:: EPOSLive/EPOSwithDynamics.png
   :scale: 100 %
   :alt: alternate text
   :align: center

Parameters for EPOS Live with Dynamics:
---------------------------------------

The EPOS agent capable of performing these experiment are created by the ``experiment.LivePeerTestbedExperiment`` class. The main differenece is the configuration file, which is the ``LivePeerTestbed.properties``. Within this configutation file, the following parameters are new:


.. code-block:: Java
      :caption: LivePeerTestbed.properties
      :name: LivePeerTestbed.properties

      userChange=false // enabling user join/leave dynamic
      joinLeaveRate=19 // maximum number of users that can join/leave at the end of 1 run: numUsersPerRun.get(currentRun)/joinLeaveRate
      userChangeProb=9 // probability of a user joins/leaves at the end of each run: SecureRandom().nextInt(userChangeProb)==0;

      minNumPeers=150 // given userChange == true, minimum number of active agents at any point in the system
      maxNumPeers=250 // given userChange == true, maximum number of active agents at any point in the system. Very important in case of computational resource restrictions.

      planChange=false // enabling plan change dynamic
      newPlanProb=9 // probability of changing plans for each user at the end of each run: (random.nextInt(newPlanProb) + 1) == 1

      weightChange=false // enabling weights change dynamic
      newWeightProb=9 // probability of changing plans for each user at the end of each run: (random.nextInt(newWeightProb) + 1) == 1

      GCFChangeProb=9 // probability of changing the globalCostFunction at the end of each run: (random.nextInt(GFCChangeProb) + 1) == 1). If `null` no changes are made.


Running EPOS Live with Dynamics:
--------------------------------

The only difference between running EPOS live with dynamics to the normal live version (:ref:`live_run-chapter`), is that the EPOS nodes should be created using the ``experiment.LivePeerTestbedExperiment`` class. To do so, few parameters in the code need to change in ``liveRunUtils.Entities.GatewayServer``:

.. code-block:: Java
      :caption: Running EPOS live with dynamics - parameter change in GatewayServer
      :name: LivePeerTestbedExperiment-GW

      // line 94
      String confPath = rootPath + File.separator + "conf" + File.separator + "LivePeerTestbed.properties";

      // line 170
      String command = "screen -S peer" + UsersStatus.get(0).index + " -d -m java -Xmx2048m -jar LivePeerNode.jar " + UsersStatus.get(0).index +
                                    " " + (bootstrapPort + UsersStatus.get(0).index) + " " + numUsersPerRun.get(currentRun) + " " + 0 + " " + currentSim;


      // line 469
      String command = "screen -S peer" + UsersStatus.get(j).index + " -d -m java -Xmx2048m -jar LivePeerNode.jar " + UsersStatus.get(j).index +
                    " " + peerPort + " " + numUsersPerRun.get(currentRun) + " " + initRun + " " + currentSim;

Additionally, in ``liveRunUtils.Entities.User``:

.. code-block:: Java
      :caption: Running EPOS live with dynamics - parameter change in User
      :name: LivePeerTestbedExperiment-User

      // line 85
      String confPath = rootPath + File.separator + "conf" + File.separator + "LivePeerTestbed.properties";

After these two change, the ``Gateway.jar`` and ``User.jar`` files need to be rebuilt, and then the following command executed: ``java -jar EPOSRequester.jar``. Everything else remains the same as :ref:`live_run-chapter`.




