Usage
=====

.. _installation:

Installation
------------

| To run Edulearn, clone the repo at https://github.com/setap-4a1/Edulearn, install dependencies, and run the Flask project:

.. code-block:: bash

   git clone https://github.com/setap-4a1/Edulearn.git
   cd Edulearn
   pip install -r requirements.txt
   flask run

Testing
-------

| To run the test suite, make sure you have installed the requirements and then run pytest.
| A running local instance will be required for Selenium tests.

.. code-block:: bash
   pytest
   pytest --cov  # Generates a coverage report

