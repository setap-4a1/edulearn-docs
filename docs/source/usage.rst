Usage
=====

.. _installation:

Installation
------------

To run Edulearn, clone the repo at https://github.com/setap-4a1/Edulearn, install dependencies, and run the Flask project:

.. code-block:: console

   $ git clone https://github.com/setap-4a1/Edulearn.git
   $ cd Edulearn
   $ pip install -r requirements.txt
   $ flask run

Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

