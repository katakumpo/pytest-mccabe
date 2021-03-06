pytest-mccabe
=============

.. image:: https://travis-ci.org/The-Compiler/pytest-mccabe.svg?branch=master
    :target: https://travis-ci.org/The-Compiler/pytest-mccabe

py.test plugin for checking cyclomatic complexity of python source with
`mccabe`_.

.. _mccabe: https://pypi.python.org/pypi/mccabe/

Usage
-----

install via::

    pip install pytest-mccabe

if you then type::

    py.test --mccabe

every file ending in ``.py`` will be discovered and run through mccabe,
starting from the command line arguments.

Simple usage example
--------------------

Consider you have this (deliberately bad and complex) code:

.. code-block:: python

    # module.py

    import random
    import os.path

    def some_function():
        num = random.random()
        if 0 <= num < 0.1:
            print("1")
        elif 0.1 <= num < 0.2:
            print("2")
        elif 0.2 <= num < 0.3:
            print("3")
        elif 0.3 <= num < 0.4:
            print("4")
        elif 0.4 <= num < 0.5:
            print("5")
        elif 0.5 <= num < 0.6:
            print("6")
        elif 0.6 <= num < 0.7:
            print("7")
        elif 0.7 <= num < 0.8:
            print("8")
        elif 0.8 <= num < 0.9:
            print("9")
        elif 0.9 <= num < 1:
            print("10")

Running py.test with pytest-mccabe installed shows you this function is
considered too complex::

   $ py.test -q --mccabe module.py
   F
   ============================== FAILURES ==============================
   ____________________________ mccabe-check ____________________________
   .../module.py:4: C901 'some_function' is too complex (11)


Configuring mccabe complexity per project and file
--------------------------------------------------

You may configure the maximum complexity for your project
by adding an ``mccabe-complexity`` entry to pytest config file (e.g.
``setup.cfg``) like this:

.. code-block:: ini

    [pytest]
    mccabe-complexity=15

Rerunning with the above example will now look better::

    $ py.test -q --mccabe foo.py
    .
    1 passed in 0.00 seconds

If you have some files where you want to set a higher complexity than the
project-wide one, you can start a mccabe-complexity line with a glob-pattern:

.. code-block:: ini

    [pytest]
    mccabe-complexity =
        *.py 7
        magic.py 10

Ignoring certain functions
--------------------------

You can exclude certain functions from the complexity check by adding comments
like this:

.. code-block:: python

    def some_function():  # noqa
        ...

    def another_function():  # pragma: no mccabe
        ...

(both will work - ``# noqa`` is mainly there for `flake8`_ compatibility)

.. _flake8: https://pypi.python.org/pypi/flake8


Running mccabe checks and no other tests
----------------------------------------

You can restrict your test run to only perform "mccabe" tests
and not any other tests by typing::

    py.test --mccabe -m mccabe

This will only run tests that are marked with the "mccabe" keyword
which is added for the mccabe test items added by this plugin.

If you are using pytest < 2.4, then use the following invocation
to the same effect::

    py.test --mccabe -k mccabe


Notes
-----

The repository of this plugin is at https://github.com/The-Compiler/pytest-mccabe

For more info on py.test see http://pytest.org

The code is based on Florian Schulze's excellent `pytest-flakes`_ - Thanks!

.. _pytest-flakes: https://pypi.python.org/pypi/pytest-flakes

Changes
=======

0.1
---

- Initial release.
