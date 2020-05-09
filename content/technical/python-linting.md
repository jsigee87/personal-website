---
title: "Linting Your Python Project"
date: 2020-05-07
toc: false
---

## Linting vs Testing

First I want to cover the difference between linting and testing so there is no confusion. Linting is a "static" test in the sense that it doesn't run or execute any code. It analyzes the file(s) looking for [code smells](https://martinfowler.com/bliki/CodeSmell.html) and what the linter considers poor style. 

On the other hand, test actually execute the code and are looking to assert that functions have the correct output given some predefined input. Unit tests typically test small functions and should be atomic and not dependent on each other. Unit tests are typically used when you want to provide a way to make sure you have not broken any logic in the system when you make changes or additions. Other common tests are acceptance tests, integration tests, and UI tests, and they are all out of the scope of this post.

It is quite common for a linter or code formatter to be installed in a project with many developers so that code remains homogenous. A project becomes much easier to maintain when there is only one style of code within it. Linters are typically either part of the test or build process, and developers can run them on their local workstation before committing their code to a build branch.  

## Pylint  

### Overview  

There are quite a few Python linters available. One of the more popular ones is pylint. Pylint can be a great way to start out but its main downfall is that it's extremely configurable. If you don't take the time to configure it it will spit out endless streams of error messages at you that you do not care about in the slightest. We are going to solve this by creating a `.pylintrc` for the project. I would recommend creating one for each project in case you want to configure things differently for different proects. This file will tell pylint which errors that you wish to ignore.   

### Installation  

You can install pylint with pip like so:  

```bash
pip install pylint
```

### Usage

When you want to run pylint you can point it to either a directory or a single file:

```
pylint my_directory/
# or
pylint my_module.py
```

Pylint will then spit out a report with all of your errors. It may look something like this:

```
$ pylint unit_tests.py
************* Module unit_tests
unit_tests.py:115:2: W0511: TODO read in 2D outputs from mfcc and pass them in. (fixme)
unit_tests.py:13:19: C0326: Exactly one space required after comma
INPUT_SIZE = (1, 98,32)
                   ^ (bad-whitespace)
unit_tests.py:44:0: C0303: Trailing whitespace (trailing-whitespace)
unit_tests.py:48:0: C0303: Trailing whitespace (trailing-whitespace)
unit_tests.py:77:0: C0303: Trailing whitespace (trailing-whitespace)
unit_tests.py:88:50: C0303: Trailing whitespace (trailing-whitespace)
unit_tests.py:93:0: C0303: Trailing whitespace (trailing-whitespace)
unit_tests.py:117:11: C0326: Exactly one space required around comparison
if __name__=="__main__":
           ^^ (bad-whitespace)
unit_tests.py:119:0: C0303: Trailing whitespace (trailing-whitespace)
unit_tests.py:1:0: C0114: Missing module docstring (missing-module-docstring)
unit_tests.py:9:0: E0401: Unable to import 'coremltools' (import-error)
unit_tests.py:10:0: E0401: Unable to import 'tensorflow' (import-error)
unit_tests.py:11:0: E0401: Unable to import 'tensorflow.core.framework' (import-error)
unit_tests.py:26:0: C0116: Missing function or method docstring (missing-function-docstring)
unit_tests.py:29:0: C0115: Missing class docstring (missing-class-docstring)
unit_tests.py:38:4: C0116: Missing function or method docstring (missing-function-docstring)
unit_tests.py:39:43: C0103: Variable name "f" doesn't conform to snake_case naming style (invalid-name)
unit_tests.py:45:4: C0116: Missing function or method docstring (missing-function-docstring)
unit_tests.py:56:0: C0115: Missing class docstring (missing-class-docstring)
unit_tests.py:85:4: C0116: Missing function or method docstring (missing-function-docstring)
unit_tests.py:96:8: C0200: Consider using enumerate instead of iterating with range and len (consider-using-enumerate)
unit_tests.py:4:0: W0611: Unused import os (unused-import)
unit_tests.py:11:0: W0611: Unused graph_pb2 imported from tensorflow.core.framework (unused-import)

-----------------------------------
Your code has been rated at 5.83/10
```

This report will tell you which line is giving the error, and what the error is. The error codes identify the specific error associated with that line. For example, the error code `C0303` tells the user that there is trailing whitespace. If you wish to disable an error for a specific line, but not project wide, you may do so by inserting a comment into the code to tell pylint to skip that error:

```python
# pylint: disable=C0303
foo = bar() # A line with the C0303 error
```

### `.pylintrc`

Disabling an error project wide is done by amending the `.pylintrc` file. First you need to create a `.pylintrc` file for your project:

```bash
pylint --generate-rcfile > <project home>/.pylintrc
```

This creates a file with all the options present. For the same error as above, you would navigate to the correct section of the file, which is `[MESSAGES CONTROL]` in this case. Put the following line there:

```
disable=C0303
```

That`s it! You can do more complicated regex to allow and disallow specific things as needed. If you want some more info on using the `.pylintrc` the documentation is [here](http://pylint.pycqa.org/en/latest/user_guide/options.html).

Now that you have your `.pylintrc` working, you need to point to it when you run pylint:

```
pylint --rcfile=/path/to/rcfile module.py
```

Pylint is extremely popular and flexible, but there are certainly other options out there: [pyflakes](https://github.com/PyCQA/pyflakes), [Flake8](https://flake8.pycqa.org/en/latest/), and [Pylama](https://pylama.readthedocs.io/en/latest/) are a few other popular ones.

## Black

[Black](https://github.com/psf/black) is another linter that is so different from the other linters that it deserves a mention. Black is less linter and more _code formatter_. This means that unlike pylint, it is not configurable and therefore highly opinionated. If you are okay with the opinions built into Black then it is a great alternative and is much simpler to use. 

To install Black you can run:

```
pip install black
```

It is worth noting that Python 3.6.0+ is required to run Black.

Usage is straightforward:

```
black module.py
# or
black directory/
```

I would love to hear opinions on Black if anyone is using it! Feel free to email me (johnsigmon@gmail.com).

## ShellCheck

I can't write anything about linting and not take the opportunity to plug my favorite linter which is [ShellCheck](https://github.com/koalaman/shellcheck). ShellCheck is used for linting shell scripts. I have found it extremely valuable and I learned quite a bit about Unix-style shell scripting from using it. The errors are not excessive, they are helpful and provide explanations. It is extremely well documented and extremely easy to use. It can be helpful for shell script authors of all skill levels. 

You can install it with your standard Unix package manager:

```
# For Ubuntu
apt-get install shellcheck

# For Mac
brew install shellcheck
```

Usage is straightforward as well:

```
shellcheck script.sh
```

It gives an error report similiar to pylint:

```
$ shellcheck deploy.sh
In deploy.sh line 29:
 --trigger-resource $TIRGGERSOURCE \
                    ^-- SC2086: Double quote to prevent globbing and word splitting.


In deploy.sh line 33:
 --env-vars-file $ENVFILENAME
                 ^-- SC2086: Double quote to prevent globbing and word splitting.

```

Each error has its own **fantastic** page in the shellcheck documentation (it usually pulls up by putting the error code in Google.) For instance, `SC2086` has a full explanation [here](https://github.com/koalaman/shellcheck/wiki/SC2086a).

If you wish to disable errors, you can do so in the same manner as pylint:

```bash
# shellcheck disable=SC2016
offending line = $unquotedvar
```

## Searching for TODOs

Pylint will automatically find "TODO" statements and warn you about them. However ShellCheck will not, and pylint may miss some types of "TODO"s. I found it useful enough to check for this that I created a script to do so:


The script finds all files ending in `.py` and `.sh` in the directory you point to, while ignoring any `.env` directories. It then looks for any "TODO" or "FIXME" in a case insensitive manner. The `grep` output is then annotated with the word "Warning" (in case you want to add this as a compile time script in another project with compiled code) and printed to the terminal.
