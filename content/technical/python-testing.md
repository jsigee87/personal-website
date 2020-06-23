---
title: "Python Testing"
date: 2020-05-24T19:18:51-05:00
draft: true
---

## What is Testing
Unfortunately, I am not really going to answer this question. We can roughly break tests into dynamic and static. We can again further break up dynamic tests into unit tests, acceptance or integration tests, and other high level tests like fuzzing, UI tests, and more. I am going to cover `pytest`, which is typically used for low level testing like unit testing. Unit test typically execute some code and look to assert that functions have the correct output give him some predefined input. Unit tests are used to test individual functions and should be atomic and not dependent on each other. Unit tests are typically used when you want to provide a way to make sure you have not broken any logic in the system when you make changes or additions.

 Unit testing is a great tool but it is not always appropriate or feasible. Software like data pipelines are difficult to unit test. Often you must write acceptance tests or integration tests for programs like this. It's important to be aware of when unit testing is appropriate and when it's not. Some more resources to read on this are:
- [Thoughts on TDD by Kent Beck](https://medium.com/@kentbeck_7670/programmer-test-principles-d01c064d7934) (written 17 years after his landmark book [Test Driven Development](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
- [Integration tests by Martin Fowler](https://martinfowler.com/bliki/IntegrationTest.html)
- [The differenc between integration and unit tests](https://stackoverflow.com/questions/5357601/whats-the-difference-between-unit-tests-and-integration-tests)
- [An overview of how all the pieces fit together by Martin Fowler](https://martinfowler.com/articles/practical-test-pyramid.html)
- The book [Beautiful Testing](https://www.amazon.com/Beautiful-Testing-Professionals-Software-Practice/dp/0596159811)

## `pytest`
There are several tools for testing in Python but this post is about `pytest`. `pytest` is relatively easy-to-use out of the box, contains plenty of features, and is reasonably configurable. Other options include the `unit-test` module from the standard library and `nose`. We will talk about:
1. Installation 
2. Setting up your test directory structure
3. Writing simple tests
4. Running `pytest`
5. Running the coverage plug in
6. Configuring `pytest`

Quite a few people have trouble importing their modules from test code, and the way Python imports modules in general seems to confuse many developers (sometimes me included.) For this reason I spend a bit of time on different ways to set up the test directory structure.

## Installation

`pytest` is straightforward to install. you can install it with Pip my running:

```
```

Don’t forget to set up a virtual environment and activate it. If you don't know how to do that you can find that information here.(add link here)  In case you already know about virtual environments but forgot the command you can run the following:

```
cd <mydir>
virtualenv -p python3 .env
source .env/bin/activate
```

## Test Directory Structure

After activating your environment and installing `pytest` within it, you will need to put your tests somewhere. It is common in Python projects to collect them in a `tests/` directory. `pytest` supports two options for structuring your project.

Option 1: Keep the tests close to the code

```
my_project/    
 └── src/
      ├── my_module.py
      └── tests/
           └── test_module.py
```

Option 2: Import the code from afar

```
my_project/    
 ├── src/
 │    └── my_module.py
 └── tests/
      └── test_module.py
```

Talk about setup.py

Wrap up and link to repo

Talk about importing into `pytest` and using setup.py and __init__.py
b) create a setup.py file (at the same level as your README.md file), here's a minimal one:
from setuptools import setup
setup(name='apple', packages=['apple'])
Run the file like so:
python setup.py develop
now apple is globally available and you should never see a no module named apple problem again, i.e. you can run py.test from the root folder or the tests folder.
You can read more about setup.py in the Python Packaging User Guide at https://python-packaging-user-guide.readthedocs.org/en/latest/index.html
## Running `pytest`
pytest also provides shorter “`pytest`” and “py.test” command that may be run instead of the longer “python -m pytest” module form. However, the shorter commands do not append the current path to PYTHONPATH, meaning modules under test may not be importable. Make sure to update PYTHONPATH before using the shorter commands.
1
2
3
4
5
# Update the Python path
PYTHONPATH=$PYTHONPATH:.
 
# Discover and run tests using the shorter command
`pytest`


Fixtures

Using fixtures in `pytest`
https://www.guru99.com/pytest-tutorial.html#12


Test duration

`pytest-cov`

Installing 

```
pip install pytest-cov
```

Using



