---
title: "JSON From The Command Line"
date: 2020-04-30
categories:
- development
- bash
- json
toc: false
---

I want to share a command line tool I have recently learned how to put to good use. `jq` provides a fantastic and fast way to manipulate and manage JSON files, especially when they become very large. I want to give a quick overview of how it works and then show some examples.

## Giving jq input and output

Jq works as a filter, which means that it requires input and will give you output. The handiest way to work with it is via redirects. In the simplest case if your input comes from a file there is no redirect necessary:

```bash
jq --some-jq-flag  myfile.json
```

This will take `myfile.json` as input and print your output to `stdout`. Say you want to redirect that output to a new file?

```bash
jq --some-jq-flag myfile.json > my_modified_file.json
```

(You may be tempted to `cat` the file into `jq` but this is a frowned upon pattern, called useless use of cat or UUOC. See [here](https://github.com/koalaman/shellcheck/wiki/SC2002) and [here](https://en.wikipedia.org/wiki/Cat_(Unix)#Useless_use_of_cat))

What if your input comes from a `grep` or `curl` command or some other process?

```bash
curl https://my-website | jq --some-jq-flag > my_modified_file.json
```

Finally, redirecting the `jq` output into another command (let’s see how many lines our output is) works as expected:

```bash
curl https://my-website | jq --some-jq-flag | wc -l
```

## But What Does It Do?

#### Pretty Print
Perhaps one of it’s most common and easy to see uses is to “pretty print” JSON data. We can tell `jq` to use the identity filter (`.`) (i.e. do not alter the data) and it will default to pretty print This is accomplished via the following:
```bash
some_input | jq '.' 
```

A useful construct to quickly pretty print from a e.g. Python script is to just “print” the data from the script and redirect the output to a file, through `jq`, e.g.:

```bash
my_script_that_prints_json.py > tmp.json
jq '.' < tmp.json
```

#### Array to Non-array and Back

Say someone has given you a JSON file that contains many objects, but they are not comma separated, and have no brackets to identify it as an array (this happens often.) You can read the file in “slurp” mode with the identity filter and `jq` will create an array out of your objects.

```bash
jq -s '.'  bracketless_file.json > array_of_objects.json
```

Now imagine the opposite- you have your array of JSON objects but you want to “remove the array.” Switching back and forth between these forms can be useful depending on the tool with which you want to manipulate the data. You can accomplish this by filtering on the array itself:

```bash
jq '.[]'  array.json > non_array.json
```

#### Converting to JSONL

Say you need to convert your array of JSON objects into [JSONL format](http://jsonlines.org/), or “new line delimited JSON.” A common use case for this for me is converting JSON into an appropriate format to be loaded into BigQuery (Google’s basic SQL database in GCP.) The appropriate flag here is the *compact* flag:

```bash
jq -c '.' non_compact.json > my_new_file.jsonl
```

(don`t forget file extensions are [“meaningless”](https://askubuntu.com/questions/803434/do-file-extensions-have-any-purpose-for-the-operating-system) in a UNIX-style system.)

#### Accessing Keys and Data

Say you are still getting a file with an array of JSON objects and want to know how many objects are in your array? We can use the identity filter and the built in `jq` length function:

```bash
# Spaces are allowed if you think it is more readable:
# jq '. | length' array.json
jq '.|length' array.json
```

Imagine you have the following data and want to know how many elements are in a specific list:


```bash
$ cat data.json
{
  “object”: [
    {...},
    ... 
    {...},
  ],
    “another object”:
     ...
}

# Get number of elements in “object”
jq '.object|length' data.json
```

Accessing the value in a specific key is simple as well. Taking the same example data as above:

```bash
jq '.object' data.json
```

This will print out the value associated with that key!

#### More information
I have barely scratched the surface of what you can do with this tool. Feel free to check out the excellent documentation on [the website](https://stedolan.github.io/jq/), and you can always run `jq --help` or `man jq`.





