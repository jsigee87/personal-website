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


when you run it you  should either point to a directory:

 or a single file:

 I would recommend creating an RC file for each projects as your requirements May differ with different projects. If this is the case you need to point pilot to your RC file like this:

```
```

 in some cases you may not want to ignore the pylons suggestion project wide. In these cases you can simply put a disabled command above the offending line like this:

```
```



Black

another linter to look into is called [Black](https://github.com/psf/black). Unlike pylint it is not very configurable and therefore highly opinionated. If you are okay with the opinions built into black then it may make a great alternative. Keep in mind that black is less of a linter and more of a code formatter. It does not warn you of code smells it simply changes your code to its preferred format.

Shellcheck

 I want to take this opportunity to also plug my favorite linter which is she'll check. She'll check is used for renting shell scripting files. It is extremely valuable and I learned quite a bit about Lenox style shell scripting from using it my files. the errors are not excessive they are helpful they provide explanations it is extremely well documented and extremely easy to use.  you can download it here  or by running:

```
```

 you can use it like this


