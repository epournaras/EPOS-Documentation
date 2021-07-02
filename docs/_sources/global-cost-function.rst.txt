.. _global-cost-function-chapter:

====================
Global Cost Function
====================

The global cost function represents the performance of EPOS at the end of each iteration and it is typically defined as: global cost function :math:`g(\cdot)` as :math:`g \colon \mathbb{R}^{d} \rightarrow \mathbb{R}`. The classes of pre-implemented global cost functions can be found in ``func`` package in the code. Using :ref:`arguments-chapter`, ``globalCostFunction`` indicates your choice of the global cost function. In order to create your own global cost function, you can use the ``func.VarCostFunction`` class as the template. For more information about how to log the global cost, check :ref:`logging-chapter` page.

.. _global-cost-function-variance:

Variance
========

The variance implemented in EPOS, within signal :math:`x \in \mathbb{R}^{d}`, is defined as follows:

.. math::
   :label: equation-var

   \sigma^2 = \frac{\sum\limits_{i=1}^N (x -\mu)^2}{N}

where signal :math:`x` represents the global response in EPOS.

Intuitively, each vector is *global response*, the sum of all selected plans in the agent network and the idea of minimising the variance of said vector is to make the demand (in case of energy planning) as uniform / flat as possible. 

This function can be selected in two ways:

1. either setting ``globalCostFunction=VAR``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: var-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc = 
                                                            new VarCostFunction();

The exact implementation of the function is given in ``func.VarCostFunction.java``. It is the subtype of ``DifferentiableCostFunction<Vector>``.

.. _global-cost-function-cross-correlation:

Cross-Correlation
=================

The cross-correlation implemented in EPOS, between signals :math:`x \in \mathbb{R}^{d}` and :math:`y \in \mathbb{R}^{d}`, is defined as follows:

.. math::
   :label: equation-cross-correlation

   - \frac{1}{d} \sum_{i = 1}^{d} \frac{1}{\sigma\left(x\right) \sigma\left(y\right)} \left(x_{i} - \mu(x)\right) \left(y_{i} - \mu(y)\right)

where :math:`x_{i}` and :math:`y_{i}` represent the :math:`i^{th}` element of the two vectors, :math:`\mu(x)` and :math:`\mu(y)` their respective means and :math:`\sigma\left(x\right)` and :math:`\sigma\left(y\right)` their standard deviations. Both vectors are of the same length :math:`d`.

Intuitively, one of the vectors is *global response*, the sum of all selected plans in the agent network, and the other vector is the incentive signal. For more information about how to provide an incentive signal, see :ref:`input-incentive-signals`. Note that both vectors are of the same length. This function can be selected in two ways:

1. either setting ``globalCostFunction=XCORR``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: cross-correlation-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc =	
                                                            new CrossCorrelationFunction();


Note the minus sign in the beginning of the formula. Two vectors have maximal matching when the cross-correlation is maximized, and vice versa, they are told to be *anti-correlated* when the cross-correlation is minimal. Given that EPOS is *minimization* algorithm, and that similarity is improved by *maximizing* the cross-correlation, EPOS actually *minimizes* the negative cross-correlation.

The definition of the function scales the input vectors according to :ref:`global-cost-function-scaling-standard`. If the scaling technique is forwarded via ``scaling`` parameter in the configuration file, it is ignored.

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

1. either setting ``globalCostFunction=RSS``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: rss-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc =	
                                                            new RSSCostFunction();

On how to select a scaling technique, see :ref:`global-cost-function-scaling`.

The residual sum of squares measures dissimilarity between the shapes of the two vectors. In other words, the matching between the two signals is the highest when the value of the function is minimized. The range of values the function can have is only lower-bounded by 0, which indicates the best possible matching.

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

where :math:`T` is the transpose operator.

For more information about how to provide an incentive signal, see :ref:`input-incentive-signals`. This function can be selected in two ways:

1. either setting ``globalCostFunction=RMSE``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: rmse-configuration-java

      public static DifferentiableCostFunction<Vector> globalCostFunc =	
                                                            new RMSECostFunction();

Intuitively, this function measures the dissimilarity between the EPOS response and the scaled incentive signal. The values of the function are lower-bounded by 0, which also indicates the best matching. Also, given that it has its own scaling technique, if the scaling technique is forwarded via ``scaling`` parameter in the configuration file, it is ignored.

The exact implementation of the function is given in ``func.RMSECostFunction.java``. It is the subtype of ``DifferentiableCostFunction<Vector>``.


.. _global-cost-function-scaling:

Scaling Techniques
==================

As means to improve the shape-matching between the signals with values of different magnitudes, three different scaling techniques are introduced. The scaling is considered only with the :ref:`global-cost-function-rss`, for any other global cost function it is ignored.

The scaling technique can be selected via ``scaling`` parameter in the configuration file. The available options are described below.

.. _global-cost-function-scaling-standard:

Standard Normalization
----------------------

The standard normalization :math:`n(\cdot)` of a vector :math:`x \in \mathbb{R}^{d}`, with the mean value :math:`\mu(x)` and the standard deviation :math:`\sigma(x)` is defined as follows:

.. math::
   :label: equation-std-normalization

   n(x) = \frac{x - \mu(x)}{\sigma(x)}

where subtraction and division are performed element-wise with constants :math:`\mu(x)` and :math:`\sigma(x)`, respectively. Note that vector :math:`n(x)` is also :math:`d`-dimensional vector. Moreover, the values in :math:`n(x)` have the mean of 0 and the standard deviation of 1. Note that values in :math:`n(x)` can be negative, but always between -1 and 1.

This standard normalization can be selected in one of the following two ways:

1. either setting ``scaling=STD``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: std-configuration-java

      public static UnaryOperator<Vector> normalizer = Vector.standard_normalization;

The implementation of the standard normalization can be found in class ``data.Vector`` and is implemented as ``UnaryOperator<Vector>`` function.

**Limitations:** The standard deviation of a vector whose all values are equal is 0, and if the standard normalization is applied on such vector, it will result in division by zero. As a solution to this, a small constant (:math:`10^{-9}`) is added in the denominator.

.. _global-cost-function-scaling-min-max:

Min-Max Scaling
---------------

The min-max scaling :math:`m(\cdot)` of a vector :math:`x \in \mathbb{R}^{d}`, where :math:`x_{min}` is the minimal value found in :math:`x`, and :math:`x_{max}` is the maximal value found in :math:`x`, is defined as follows:

.. math::
   :label: equation-min-max

   m(x) = \frac{x - x_{min}}{x_{max} - x_{min}}

where subtraction and division are performed element-wise with constants :math:`x_{min}` and :math:`x_{max} - x_{min}`, respectively. The values in :math:`m(x)` are always non-negative. More precisely, min-max scaling maps :math:`x_{min}` to 0 and :math:`x_{max}` to 1. 

This min-max scaling can be selected in one of the following two ways:

1. either setting ``scaling=MIN-MAX``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: min-max-configuration-java

      public static UnaryOperator<Vector> normalizer = Vector.min_max_normalization;

The implementation of the min-max scaling can be found in class ``data.Vector`` and is implemented as ``UnaryOperator<Vector>`` function.

**Limitations:** For a vector whose values are all equal, it holds that :math:`x_{min} = x_{max}`, hence if the min-max scaling is applied on such vector, it results in division by zero. As a solution to this, a small constant (:math:`10^{-9}`) is added in the denominator.

.. _global-cost-function-scaling-unit-length:

Unit-Length Scaling
-------------------

The unit-length scaling :math:`u(\cdot)` of a vector :math:`x \in \mathbb{R}^{d}`, whose Euclidean (:math:`L_{2}`) norm is :math:`\left\lVert x \right\rVert`, is defined as follows:

.. math::
   :label: equation-unit-length

   u(x) = \frac{x}{\left\lVert x \right\rVert}

where division is performed element-wise with constant :math:`\left\lVert x \right\rVert`. The Euclidean norm of the resulting vector :math:`u(x)` has the Euclidean norm of 1, :math:`\left\lVert u(x) \right\rVert = 1`. The absolute value of every element in :math:`u(x)` is between 0 and 1.

This unit-length scaling can be selected in one of the following two ways:

1. either setting ``scaling=UNIT-LENGTH``, or

2. .. code-block:: java
      :caption: config.Configuration.java
      :name: unit-length-configuration-java

      public static UnaryOperator<Vector> normalizer = Vector.unit_length_normalization;

The implementation of the unit-length scaling can be found in class ``data.Vector`` and is implemented as ``UnaryOperator<Vector>`` function.

**Limitations:** A vector whose all values are 0, has the Euclidean norm of 0, and if the unit-length scaling is applied on such vector, it results in division by zero. As a solution to this, a small constant (:math:`10^{-9}`) is added in the denominator.
