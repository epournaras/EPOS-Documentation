.. _global-cost-function-chapter:

====================
Global Cost Function
====================

.. _global-cost-function-variance:

Variance
========

.. _global-cost-function-cross-correlation:

Cross-Correlation
=================

The cross-correlation implemented in EPOS, between signals :math:`x \in \mathbb{R}^{d}` and :math:`y \in \mathbb{R}^{d}`, is defined as follows:

.. math::
   :label: equation-cross-correlation

   - \frac{1}{d} \sum_{i = 1}^{d} \frac{1}{\sigma\left(x\right) \sigma\left(y\right)} \left(x_{i} - \mu(x)\right) \left(y_{i} - \mu(y)\right)

where :math:`x_{i}` and :math:`y_{i}` represent the :math:`i^{th}` element of the two vectors, :math:`\mu(x)` and :math:`\mu(y)` their respective means and :math:`\sigma\left(x\right)` and :math:`\sigma\left(y\right)` their standard deviations. Both vectors are of the same length :math:`d`.

Intuitively, one of the vectors is *global response*, the sum of all selected plans in the agent network, and the other vector is the incentive signal. For more information about how to provide an incentive signal, see :ref:`input-incentive-signals`. Note that both vectors are of the same length. This function can be selected in two ways:

1. either setting ``-setGlobalCostFunc XCORR``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: cross-correlation-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc =	
                                                            new CrossCorrelationFunction();


Note the minus sign in the beginning of the formula. Two vectors have maximal matching when the cross-correlation is maximized, and vice versa, they are told to be *anti-correlated* when the cross-correlation is minimal. Given that EPOS is *minimization* algorithm, and that similarity is improved by *maximizing* the cross-correlation, EPOS actually *minimizes* the negative cross-correlation.

The definition of the function scales the input vectors according to :ref:`global-cost-function-scaling-standard`. If the scaling technique is forwarded via ``-setScaling`` argument, it is ignored.

The exact implementation of the function is given in ``func.CrossCorrelationFunction.java``. It is the subtype of ``DifferentiableCostFunction<Vector>``.

**Limitations:** Recall that the standard deviation of the vector that contains the same value :math:`\forall i \in \{1, 2, \ldots d\}`, is 0. Consequently, the denominator in Equation :eq:`equation-cross-correlation` is 0, and we have division by zero. To overcome this limitation, a small constant (:math:`10^{-9}`) is added to the denominator.

.. _global-cost-function-rss:

Residual Sum of Squares
=======================

The residual sum of squares implemented in EPOS, between signals :math:`x \in \mathbb{R}^{d}` and :math:`y \in \mathbb{R}^{d}`, is defined as follows:

.. math::
   :label: equation-rss

   RSS(x, y, s(\cdot)) = \left(s(x) - s(y)\right)^{T} \left(s(x) - s(y)\right)

where :math:`s(\cdot)` denotes the scaling technique employed (see :ref:`global-cost-function-scaling`), and :math:`T` denotes the transpose operator. Vectors :math:`s(x)` and :math:`s(y)` are referred to as *scaled vectors*, and the difference :math:`s(x) - s(y)` as the **residual** between scaled vectors. Note that both vectors :math:`x` and :math:`y` **must** be of the same length :math:`d`.

Intuitively, one of the vectors is *global response*, the sum of all selected plans in the agent network, and the other vector is the incentive signal. For more information about how to provide an incentive signal, see :ref:`input-incentive-signals`. Note that both vectors are of the same length. This function can be selected in two ways:

1. either setting ``-setGlobalCostFunc RSS``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: rss-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc =	
                                                            new RSSCostFunction();

On how to select a scaling technique, see :ref:`global-cost-function-scaling`.

The residual sum of squares measures dissimilarity between the shapes of the two vectors. In other words, the matching between the two signals is the highest when the value of the function is minimized. The range of values the function can have is only lower-bounded by 0, whihc inidicates the best possible matching.

The exact implementation of the function is given in ``func.RSSCostFunction.java``. It is the subtype of ``DifferentiableCostFunction<Vector>``.

.. _global-cost-function-rmse:

Root Mean Square Error
======================

This similarity measure function is specific because it uses its own scaling technique. If the incentive signal (see :ref:`input-incentive-signals`) is denoted as :math:`h \in \mathbb{R}^{d}` and the EPOS response is given as :math:`g \in \mathbb{R}^{d}`, then the scaled shape to match, :math:`x \in \mathbb{R}^{d}`, is constructed as follows:

.. math::
   :label: equation-rmse-scaling

   x = \frac{\sigma\left(g\right)}{\sigma\left(h\right)} \left(h - \mu(h)\right) + \mu(g)

where :math:`\sigma\left(g\right)` and :math:`\sigma\left(h\right)` denote standard deviations of the signals, and :math:`\mu(g)` and :math:`\mu(h)` their means. Intuitively, you can think of it as *scaling the signal* :math:`h` *to the scale of the signal* :math:`g`. Then, the root mean square error is computed as follows:

.. math::
   :label: equation-rmse

   \sqrt{\frac{1}{d} \left(g - x\right)^{T} \left(g - x\right)}

where :math:`T` is transpose operator.

For more information about how to provide an incentive signal, see :ref:`input-incentive-signals`. This function can be selected in two ways:

1. either setting ``-setGlobalCostFunc RMSE``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: rmse-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc =	
                                                            new RMSECostFunction();

Intuitively, this function measures the dissimilarity between the EPOS response and the scaled incentive signal. The values of the function are lower-bounded by 0, which also indicates the best matching. Also, given that it has its own scaling technique, if the scaling technique is forwarded via ``-setScaling`` argument, it is ignored.

The exact implementation of the function is given in ``func.RMSECostFunction.java``. It is the subtype of ``DifferentiableCostFunction<Vector>``.


.. _global-cost-function-scaling:

Scaling Techniques
==================

.. _global-cost-function-scaling-standard:

Standard Normalization
----------------------

.. _global-cost-function-scaling-min-max:

Min-Max Scaling
---------------

.. _global-cost-function-scaling-unit-length:

Unit-Length Scaling
-------------------

