---
title: "Bash Tips"
date: 2020-04-26
slug: bash-tips
categories:
- development
- bash
---

#### Redirects

All those fun redirect operators are outlined really well in this [Stack Overflow post](https://unix.stackexchange.com/questions/159513/what-are-the-shells-control-and-redirection-operators).

#### Grep, Awk, and Sed

Grep is one of the most flexible and useful Linux utilities, Awk is actually a full blown programming language, and Sed isn't too bad either. This [9 page overview](https://www-users.york.ac.uk/~mijp1/teaching/2nd_year_Comp_Lab/guides/grep_awk_sed.pdf) gives some good tips to get started without being overly long and verbose.

#### Comparison Operators 

The cases of comparing strings versus comparing variables are handled a little differently in bash. [This](https://www.tldp.org/LDP/abs/html/comparison-ops.html) is an excellent guide. Note the whitespace! Bash is particular about whitespace.

#### Checking if a Variable is Empty

Example:

```bash
if [ -z "$var" ]
then
    # var is empty
else
    # var is not empty
fi
```

#### Checking Exit Codes

The proper way to check out an exit code is C-style, and can be a little confusing. Example stolen from [this](https://stackoverflow.com/questions/26675681/how-to-check-the-exit-status-using-an-if-statement-using-bash) Stack Overflow post.

```bash
if some_command
then
    # command returned true
else
    # command returned some error
fi
```

or inverted:

```bash
if ! some_command
then
    # command returned some error
else
    # command returned true
fi
```

#### Checking Apt Versions

Ex.

```bash
$ apt-cache policy libgtest-dev
libgtest-dev:
  Installed: 1.7.0-4ubuntu1
  Candidate: 1.7.0-4ubuntu1
  Version table:
 *** 1.7.0-4ubuntu1 500
        500 http://us.archive.ubuntu.com/ubuntu xenial/universe amd64 Packages
        100 /var/lib/dpkg/status
```

 Here the version is `1.7.0-4ubuntu1` and this version can be installed by running:

```bash
apt-get install libgtest-dev=1.7.0-4ubuntu1
```

#### Checking the Beginning of a Large File 

In a simple case, head is useful:

```bash
# This gives the first ten lines
head large_file 

# This gives the first 25 lines
head -25 large_file 
```

For more complicated cases head has other flags, or you can use cut:

```bash
# This gives the first 100 bytes
head -c 100 large_file 

# This gives the first 100 characters 
# (useful for debugging JSON loading errors!)
cut -c1-100 < large_file

# The above command is equivalent to
cat large_file | cut -c1-100
```

#### Custom logging statements

A practice I am in the habit of is placing a variable at the top of scripts called `MYFILENAME`. I have found it useful for logging and usage statements.

```bash
MYFILENAME="myscript.sh"

# LINENO is built in 
echo "[INFO: $MYFILENAME: $LINENO] Starting myscript.sh"
```

This becomes helpful when you have large and complicated bash scripts that need to work together.


#### Casing Arguments

This has worked well for me to case out command line args. You may need other checks to ensure the correct variables were passed in. If you pass in an extra arg thats not supported this does not catch it. This example is something I commonly have to parse out for deployment scripts. If the flag `--dev` is found, I set a `DEV` flag to true, etc.

```bash
# While there are still arguments to parse
while [[ $# -gt 0 ]]
do
key="$1"

# Case them out
case $key in
    --dev)
    DEV=true
    shift
    ;;
    --prod)
    PROD=true
    shift
    ;;
esac
done
```

#### Figuring Out Where Your File Is

You want someone to be able to run your script from multiple locations, but the script may need to move around within a repository to access different files. A simple and _relatively_ robust way to manage this is by figuring out the directory your script is located in, and `cd`-ing to that directory. This snippet should take care of that.

```bash
cd $(dirname "$0")
```

#### Using Shellcheck

If you have never used it, [shellcheck](https://github.com/koalaman/shellcheck) is a fantastic shell linter. Install and set up is easy, there is no reason not to use it. I found it to be accurate and helpful, and definitely not too picky.

#### Iterating Over an Array

I always forget the syntax for this, so here is an example:

```bash
SAMPLE_LIST=(
    "list item 1"
    "list item 2"
)


for item in "${SAMPLE_LIST[@]}"
do
    # Access the item like this
    echo "${item}"
done
```
