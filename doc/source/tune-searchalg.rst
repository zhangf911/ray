Tune Search Algorithms
======================

Tune provides various hyperparameter search algorithms to efficiently optimize your model. Tune allows you to use different search algorithms in combination with different trial schedulers. Tune will by default implicitly use the Variant Generation algorithm to create trials.

You can utilize these search algorithms as follows:

.. code-block:: python

    run_experiments(experiments, search_alg=SearchAlgorithm(...))

Currently, Tune offers the following search algorithms (and library integrations):

- `Grid Search and Random Search <tune-searchalg.html#variant-generation-grid-search-random-search>`__
- `BayesOpt <tune-searchalg.html#bayesopt-search>`__
- `HyperOpt <tune-searchalg.html#hyperopt-search-tree-structured-parzen-estimators>`__
- `SigOpt <tune-searchalg.html#sigopt-search>`__
- `Nevergrad <tune-searchalg.html#nevergrad-search>`__


Variant Generation (Grid Search/Random Search)
----------------------------------------------

By default, Tune uses the `default search space and variant generation process <tune-usage.html#tune-search-space-default>`__ to create and queue trials. This supports random search and grid search as specified by the ``config`` parameter of the Experiment.

.. autoclass:: ray.tune.suggest.BasicVariantGenerator
    :show-inheritance:
    :noindex:


Note that other search algorithms will not necessarily extend this class and may require a different search space declaration than the default Tune format.

BayesOpt Search
---------------

The ``BayesOptSearch`` is a SearchAlgorithm that is backed by the `bayesian-optimization <https://github.com/fmfn/BayesianOptimization>`__ package to perform sequential model-based hyperparameter optimization. Note that this class does not extend ``ray.tune.suggest.BasicVariantGenerator``, so you will not be able to use Tune's default variant generation/search space declaration when using BayesOptSearch.

In order to use this search algorithm, you will need to install Bayesian Optimization via the following command:

.. code-block:: bash

    $ pip install bayesian-optimization

This algorithm requires `setting a search space and defining a utility function <https://github.com/fmfn/BayesianOptimization/blob/master/examples/advanced-tour.ipynb>`__. You can use BayesOptSearch like follows:

.. code-block:: python

    run_experiments(experiment_config, search_alg=BayesOptSearch(bayesopt_space, utility_kwargs=utility_params, ... ))

An example of this can be found in `bayesopt_example.py <https://github.com/ray-project/ray/blob/master/python/ray/tune/examples/bayesopt_example.py>`__.

.. autoclass:: ray.tune.suggest.BayesOptSearch
    :show-inheritance:
    :noindex:

HyperOpt Search (Tree-structured Parzen Estimators)
---------------------------------------------------

The ``HyperOptSearch`` is a SearchAlgorithm that is backed by `HyperOpt <http://hyperopt.github.io/hyperopt>`__ to perform sequential model-based hyperparameter optimization. Note that this class does not extend ``ray.tune.suggest.BasicVariantGenerator``, so you will not be able to use Tune's default variant generation/search space declaration when using HyperOptSearch.

In order to use this search algorithm, you will need to install HyperOpt via the following command:

.. code-block:: bash

    $ pip install --upgrade git+git://github.com/hyperopt/hyperopt.git

This algorithm requires using the `HyperOpt search space specification <https://github.com/hyperopt/hyperopt/wiki/FMin>`__. You can use HyperOptSearch like follows:

.. code-block:: python

    run_experiments(experiment_config, search_alg=HyperOptSearch(hyperopt_space, ... ))

An example of this can be found in `hyperopt_example.py <https://github.com/ray-project/ray/blob/master/python/ray/tune/examples/hyperopt_example.py>`__.

.. autoclass:: ray.tune.suggest.HyperOptSearch
    :show-inheritance:
    :noindex:

SigOpt Search
-------------

The ``SigOptSearch`` is a SearchAlgorithm that is backed by `SigOpt <https://sigopt.com/>`__ to perform sequential model-based hyperparameter optimization. Note that this class does not extend ``ray.tune.suggest.BasicVariantGenerator``, so you will not be able to use Tune's default variant generation/search space declaration when using SigOptSearch.

In order to use this search algorithm, you will need to install SigOpt via the following command:

.. code-block:: bash

    $ pip install sigopt

This algorithm requires the user to have a `SigOpt API key <https://app.sigopt.com/docs/overview/authentication>`__ to make requests to the API. Store the API token as an environment variable named ``SIGOPT_KEY`` like follows:

.. code-block:: bash

    $ export SIGOPT_KEY= ...

This algorithm requires using the `SigOpt experiment and space specification <https://app.sigopt.com/docs/overview/create>`__. You can use SigOptSearch like follows:

.. code-block:: python

    run_experiments(experiment_config, search_alg=SigOptSearch(sigopt_space, ... ))

An example of this can be found in `sigopt_example.py <https://github.com/ray-project/ray/blob/master/python/ray/tune/examples/sigopt_example.py>`__.

.. autoclass:: ray.tune.suggest.SigOptSearch
    :show-inheritance:
    :noindex:

Nevergrad Search
----------------

The ``NevergradSearch`` is a SearchAlgorithm that is backed by `Nevergrad <https://github.com/facebookresearch/nevergrad>`__ to perform sequential model-based hyperparameter optimization. Note that this class does not extend ``ray.tune.suggest.BasicVariantGenerator``, so you will not be able to use Tune's default variant generation/search space declaration when using NevergradSearch.

In order to use this search algorithm, you will need to install Nevergrad via the following command.:

.. code-block:: bash

    $ pip install nevergrad

Keep in mind that ``nevergrad`` is a Python 3.6+ library.

This algorithm requires using an optimizer provided by ``nevergrad``, of which there are many options. A good rundown can be found on their README's `Optimization <https://github.com/facebookresearch/nevergrad>`__ section. You can use ``NevergradSearch`` like follows:

.. code-block:: python

    run_experiments(experiment_config, search_alg=NevergradSearch(optimizer, parameter_names, ... ))

An example of this can be found in `nevergrad_example.py <https://github.com/ray-project/ray/blob/master/python/ray/tune/examples/nevergrad_example.py>`__.

.. autoclass:: ray.tune.suggest.NevergradSearch
    :show-inheritance:
    :noindex:

Scikit-Optimize Search
----------------------

The ``SkOptSearch`` is a SearchAlgorithm that is backed by `Scikit-Optimize <https://scikit-optimize.github.io>`__ to perform sequential model-based hyperparameter optimization. Note that this class does not extend ``ray.tune.suggest.BasicVariantGenerator``, so you will not be able to use Tune's default variant generation/search space declaration when using SkOptSearch.

In order to use this search algorithm, you will need to install Scikit-Optimize via the following command:

.. code-block:: bash

    $ pip install scikit-optimize

This algorithm requires using the `Scikit-Optimize ask and tell interface <https://scikit-optimize.github.io/notebooks/ask-and-tell.html>`__. This interface requires using the `Optimizer <https://scikit-optimize.github.io/#skopt.Optimizer>`__ provided by Scikit-Optimize. You can use SkOptSearch like follows:

.. code-block:: python

    optimizer = Optimizer(dimension, ...)
    run_experiments(experiment_config, search_alg=SkOptSearch(optimizer, parameter_names, ... ))

An example of this can be found in `skopt_example.py <https://github.com/ray-project/ray/blob/master/python/ray/tune/examples/skopt_example.py>`__.

.. autoclass:: ray.tune.suggest.SkOptSearch
    :show-inheritance:
    :noindex:

Contributing a New Algorithm
----------------------------

If you are interested in implementing or contributing a new Search Algorithm, the API is straightforward:

.. autoclass:: ray.tune.suggest.SearchAlgorithm
    :members:
    :noindex:

Model-Based Suggestion Algorithms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Often times, hyperparameter search algorithms are model-based and may be quite simple to implement. For this, one can extend the following abstract class and implement ``on_trial_result``, ``on_trial_complete``, and ``_suggest``. The abstract class will take care of Tune-specific boilerplate such as creating Trials and queuing trials:

.. autoclass:: ray.tune.suggest.SuggestionAlgorithm
    :show-inheritance:
    :noindex:

    .. automethod:: ray.tune.suggest.SuggestionAlgorithm._suggest
        :noindex:
