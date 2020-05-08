---
title: "Linting Your Python Project"
date: 2020-05-07T19:37:35-05:00
draft: true
toc: false
---

## Linting vs Testing

First I want to cover the difference between linting and testing so there is no confusion. Linting is a "static" test in the sense that it doesn't run or execute any code. It analyzes the file(s) looking for [code smells](https://martinfowler.com/bliki/CodeSmell.html) and what the linter considers poor style. On the other hand, test actually execute the code and are looking to assert that functions have the correct output given some predefined input. Unit tests typically test small functions and should be atomic and not dependent on each other. Unit tests are typically used when you want to provide a way to make sure you have not broken any logic in the system when you make changes or additions. Other common tests are acceptance tests, integration tests, and UI tests, and they are all out of the scope of this post.

It is quite common for a linter or code formatter to be installed in a project with many developers so that code remains homogenous. A project becomes much easier to maintain when there is only one style of code within it. Linters are typically either part of the test or build process, and developers can run them on their local workstation before committing their code to a build branch.  

## Pylint  

### Overview  

There are quite a few Python linters available. One of the more popular ones is pylint. Pylint can be a great way to start out but its main downfall is that it's extremely configurable. If you don't take the time to configure it it will spit out endless streams of error messages at you that you do not care about in the slightest. We are going to solve this by creating a `.pylintrc` for the project. I would recommend creating one for each project in case you want to configure things differently for different proects. This file will tell pylint which errors that you wish to ignore.   

### Installation  

You can install pylint with pip like so:  

```bash
pip install pylint
```

When you want to run pylint you can point it to either a directory or a single file:

```bash
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

This report will tell you which line is giving the error, and what the error is. The error codes identify the specific error associated with that line. For example, the error code `C0303` tells us that we have trailing whitespace. If you wish to disable an error for a specific line, but not project wide, you may do so by inserting a comment into the code to tell pylint to skip that error:

```python
# pylint: disable=C0303
foo = bar() # A line with the C0303 error
```

Disabling an error project wide is done by amending the `.pylintrc` file. For the same error as above, you would put the following inside your `.pylintrc`:

```
```

I would recommend putting the `.pylintrc` at the top of the project. However, this may require that you point to it from wherever you are running pylint.
```
```

Black

another linter to look into is called [Black](https://github.com/psf/black). Unlike pylint it is not very configurable and therefore highly opinionated. If you are okay with the opinions built into black then it may make a great alternative. Keep in mind that black is less of a linter and more of a code formatter. It does not warn you of code smells it simply changes your code to its preferred format.

Shellcheck

 I want to take this opportunity to also plug my favorite linter which is she'll check. She'll check is used for renting shell scripting files. It is extremely valuable and I learned quite a bit about Lenox style shell scripting from using it my files. the errors are not excessive they are helpful they provide explanations it is extremely well documented and extremely easy to use.  you can download it here  or by running:

```
```

 you can use it like this


