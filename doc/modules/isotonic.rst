.. _isotonic:

===================
Isotonic regression
===================

.. currentmodule:: sklearn.isotonic

The class :class:`IsotonicRegression` fits a non-decreasing real function to
1-dimensional data. It solves the following problem:

.. math::
    \min \sum_i w_i (y_i - \hat{y}_i)^2

subject to :math:`\hat{y}_i \le \hat{y}_j` whenever :math:`X_i \le X_j`,
where the weights :math:`w_i` are strictly positive, and both `X` and `y` are
arbitrary real quantities.

The `increasing` parameter changes the constraint to
:math:`\hat{y}_i \ge \hat{y}_j` whenever :math:`X_i \le X_j`. Setting it to
'auto' will automatically choose the constraint based on `Spearman's rank
correlation coefficient
<https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient>`_.

:class:`IsotonicRegression` produces a series of predictions
:math:`\hat{y}_i` for the training data which are the closest to the targets
:math:`y` in terms of mean squared error. These predictions are interpolated
for predicting to unseen data. The predictions of :class:`IsotonicRegression`
thus form a function that is piecewise linear:

.. figure:: ../auto_examples/miscellaneous/images/sphx_glr_plot_isotonic_regression_001.png
   :target: ../auto_examples/miscellaneous/plot_isotonic_regression.html
   :align: center

Infinite values in `X` are allowed (but NaN values are not). An infinite
threshold makes the training domain unbounded on that side, and predictions
are constant between the largest finite threshold and infinity, except at
exactly infinity, where the value fitted at that infinite threshold is
returned. For instance, with thresholds ``[0, 1, +inf]`` and targets
``[0, 0.5, 1]``, an input of ``2`` is predicted as ``0.5`` while an input of
``+inf`` is predicted as ``1``. Symmetrically, with a smallest threshold of
``-inf``, finite inputs below the smallest finite threshold are predicted with
the fitted value at that finite threshold, while an input exactly equal to
``-inf`` is predicted with the fitted value at ``-inf``. As a consequence, when
a threshold is infinite, the corresponding bound (``X_min_`` or ``X_max_``) is
infinite too, so arbitrarily large finite inputs are no longer considered out
of bounds by `out_of_bounds`. Infinite query points are accepted as well: on a
model fitted with only finite thresholds, they fall outside the fitted domain
and give NaN under the default ``out_of_bounds="nan"``.

When `X` contains no finite threshold at all (for example ``[-inf, +inf]``),
the fitted domain consists only of the infinite thresholds: predictions at
those exact points return the fitted values, while finite query values are out
of domain and give NaN under ``out_of_bounds="nan"``. ``out_of_bounds="raise"``
raises for any finite query, and ``out_of_bounds="clip"`` still gives NaN
because the bounds themselves are infinite, so clipping cannot move a finite
query onto a threshold. Only when the infinite bounds coincide (a single
infinite threshold) does ``out_of_bounds="clip"`` map every query onto that
threshold.

.. rubric:: Examples

* :ref:`sphx_glr_auto_examples_miscellaneous_plot_isotonic_regression.py`
