.. _tree-topology-chapter:

=============
Tree Topology
=============

EPOS organises the agents in a tree structure that is employed to guide the communication and information exchange up and down the tree. Aggregated knowlede is propagated in a bottom-up fashion, while feedback is propagated top-down. Consequently, the tree properties, such as the number of children of the agents, how balanced the tree is and the properties of the agents' plans, all play a significant role in the optimization process.

In ``config.Configuration`` class, the specifics of the tree is by default defined using the following:

.. code-block:: java
      :caption: config.Configuration.java
      :name: tree-initiation

      public static RankPriority priority = RankPriority.HIGH_RANK;
      public static DescriptorType rank = DescriptorType.RANK;
      public static TreeType type = TreeType.SORTED_HtL;
      public static BalanceType balance = BalanceType.WEIGHT_BALANCED;
      public static int numChildren = 2;

This creates a balanced binary tree which sorts the agents based on their rank. Note that the tree "structure" is generated from top to bottom, but the structure is **populated** by agents in a breadth-first bottom-up manner. In other words, the first agent in the sorted-by-rank agent-list will be associated to the left-most *leaf* (node without children), and the last one to the root spot. If you need to change the topology or modify certain aspects of it check out ``util.TreeArchitecture.java``. 

For the sake of statistical significance, the experiments should often be repeated multiple times with enabling randomness. If the tree "structure" remains the same, the randomness can be neabled by shuffling the sorted-by-rank list of agents which will consequently change the relative position of the agents within the tree structure and result in potentially different optimization process. The shuffling of this list is controlled by the `shuffle` parameter in the property file (for more info, check :ref:`arguments-chapter`), which indicates how many times the list should be randomly shuffled. The shuffling uses a ``Random Generator`` that is initialized by a seed given by the ``reorganizationSeed`` parameter in the property file. When EPOS is repeated with the same set of values for ``shuffle`` and ``reorganizationSeed`` parameters, the same solutions are generated, enabling reproducability of the simulations. The results of repeating EPOS with various ``reorganizationSeed`` values are shown below.

.. figure:: RSS-MIN-MAX-ENERGY-SIGNAL-5-BOUNDS.png
   :scale: 70 %
   :alt: alternate text
   :align: center

Moreover, EPOS can perform dynamic and on-the-fly agent shuffling when some criteria are met. For more info, see :ref:`tree-topology-reorganization`. 


In addition, each agent in EPOS has two classes which take care of the tree communication and keep track of the parent and children nodes addresses: ``TreeAgent extends Agent<V> implements TreeApplicationInterface`` and ``IterativeTreeAgent extends TreeAgent``. In ``TreeAgent`` the most important functions are the ``public void setTreeView(Finger parent, List<Finger> children)`` and ``public void setParent(Finger parent)`` and ``public void setChildren(List<Finger> list)`` which are called when the ``TreeArchitecture`` is finished bootstrapping and creating the tree. In ``IterativeTreeAgent`` the most important function is the ``public void handleIncomingMessage(Message message)`` which is called whenever a new message is receive by the agent. Here you can track the *to-down* and *bottom-up* phases of EPOS.

