.. _termination-criteria-chapter:

Termination Criteria
====================

Termination criteria in EPOS is defined at two levels, in the agent level, it is defined as the last iteration that changes the global cost of the result. At the simulation level, termination of the simulation is when all the iterations are done. The iterations are defined at ``config.Configuration`` as ``public static int numIterations = 40`` by default and can be also set by :ref:`arguments-chapter`. Termination is logged by the termination logger implemented in ``agent.logging.TerminationLogger``. More detailed on loggers can be found in :ref:`logging-chapter` page.
