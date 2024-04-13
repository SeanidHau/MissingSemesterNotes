# Lecture 2 Shell Tools & Scripting

##  Official note

###  English version

[Shell Tools and Scripting · Missing Semester (mit.edu)](https://missing.csail.mit.edu/2020/shell-tools/)

###  Chinese version

[Shell 工具和脚本 · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/2020/shell-tools/)

###  Assignment

```bash
# sample
foo=bar
echo $foo
bar
```

"" & '' have the same meaning while facing pure string. 

```bash
# sample
echo "Value is $foo"
Value is bar
echo 'Value is $foo'
Value is $foo
```

In Bash, *$1*  is just like the *argv[1]*  in Python, and ***$0*** is the name of the file.***$#*** is the number of arguments that we giving to the command.

```bash
# sample
# use the input as the name of the d
mcd(){
	mkdir -p "$1"
	cd "$1"
}
```

"||" means "or" and "&&" means "and", which is similar to the C Language.

```bash
# sample
true || echo "Will be not be printed"

# sample
false && echo "This will not print"
```

***$?*** represents the exit status or return code of the last executed command. 

```bash
# sample
ls
echo $?
0
```

***$_*** holds the value of the last argument of the previous command. The erroe code of ***True***  is always ***0*** while ***False*** is always 1.

```bash
# sample
echo "Hello World"
Hello World
echo $_
Hello World
```

***!!*** represents the entire previous command. 

```bash
# sample
rm -rf /path/to/file
sudo !!
```

Use ";" to concatenate commands in the same line.

```  bash
# sample
false ; echo "This will always print"
This will always print
```

###  Pricess substitution

<(  ) treat the output of a command as a file-like object. 

```bash
# sample
cat <(ls) <(ls ..)
last-modified.txt
seanid
```

***"$$"***  is the process ID (also being called PID) of running command.

```bash
# sample
echo $$
61
```

***"$@"*** expands to a list of all the positional parameters, each parameter treated as a separate string.

```bash
# sample
#!/bin/bash
echo "The total number of arguments：$#"
echo "The arguments are：$@"
The total number of arguments：3
The arguments are：arg1 arg2 arg3
```

***/dev/null***  

- often referred to as the "null device" or "bit bucket".
- Any data written to it is discarded and ingored.
- Reading from `/dev/null` always returns an end-of-file (EOF) condition, indicating that there is no data to read.
- Redirecting output to `/dev/null` is a way to effectively discard the output of a command or prevent it from being displayed on the terminal.

***{ }***

- Curly braces can be used for brace expansion, which allows you to generate a range of values or permutations.
- Curly braces can be used to group commands together and treat them as a single unit.
- Curly braces can be used for variable expansion to explicitly specify the boundaries of a variable name.

```bash
# sample1
echo {1..5}
# Output: 1 2 3 4 5
echo {a,b,c}
# Output: a b c
echo {a..c}
# Output: a b c

# sample2
{ command1 ; command2 ; command3 ; }

# sample3
name="John"
echo "Hello, ${name}!"
# Output: Hello, John!

convert image.{jpg,png}
# output:image.jpg image.png

touch foo{,1,2,10}
# similar to
touch foo foo1 foo2 foo10

# or even
touch {foo,bar}/{a..j}
# which is going to expand to "foo/a", "foo/b" like all these combinations through "j".
```

```bash
# sample
compares the contents of the two directories and displays the differences

diff <(ls foo) <(ls bar)
11c11
< x
---
> y
```

`Ctrl+R` backward search

###  Shebang

also known as a hashbang or a sharp-bang, is a special sequence of characters at the beginning of a script file that specifies the interpreter to be used to execute the script. It consists of the characters "#!" followed by the path to the interpreter.

```python
# sample
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
	print(arg)
# foobar
```

If you forget the path fo interpreter, you can use `#!/bin/env python(or something else)`.

###  shellcheck



### regular expression

In Bash, regular expressions can be used in commands.

```bash
# sample
# list all the files which is end of .sh
ls *.sh
```

##  Basic Commands

1. `source` execute the content of a shell script file within the current shell environment.

```bash
# sample
source filename
# or
. filename
```

2. `grep` search for text patterns within files or input streams.

```bash
# sample
grep pattern file
```

3. `find ` search for files and directories in a given directory hierarchy based on various criteria. It allows users to search for files by name, size, modification time, permissions, and more.

```bash
# sample
find . -name src -type d
,/project1/src
./project2/src

# sample
find . -path '**/test/*.py' -type f

# sample
# files modified in the last day
find . -mtime -1

# sample
# find and delete
find . -name "*.py" -exec rm {} \;
```

4. `locate` find files and directories based on their names.

```bash
# sample
locate missing-semester
```

5. `rg` ripgrep. searching text within files and directories. It is designed to be fast and efficient, making it a popular alternative to the traditional "grep" command. 

   `ag` or `ack` are the similar tools.

```bash
# sample
# search the python file in scratch folder where uses the request library and print the context in the next 5 lines.
rg "import requests" -t py -C 5 ~/scratch

# sample
# find the shell file without shebang.
rg -u --files-without-matvh "^#\!" -t sh
```

##  Comparisons in Bash

##  Tools

###  tldr

tldr (Too Long; Didn't Read) is a command-line tool that provides concise and easy-to-understand command line guides and examples. Its main purpose is to offer quick help and usage examples for common commands, helping users quickly grasp the basic usage of a command.

###  fzf

fzf(fuzzy finder) is a command-line fuzzy finder that provides an interactive search and selection interface for files, directories, and other items. It allows users to quickly and efficiently search through a list of items using fuzzy matching, where you can type partial or misspelled patterns and "fzf" will intelligently match and display the closest matches.

```bash
# sample
cat example.sh | fzf
```

###  tree

displays the directory structure in a tree-like format. It provides a visual representation of directories and files, showing the hierarchical relationship between them.

`broot` is almost the same.

```bash
# sample
tree 
```



##  A simple script

```bash
#example.sh
#!/bin/bash

echo "Starting program at $(date)"

echo "Running program $0 with $# argumrnts with pid $$"

for file in "$@"; do
	grep foobar "$file" > /dev/null 2> /dev/null
	if [[ "$?" -ne 0 ]]; then
		echo "File $file does not have any foobar, adding one."
	fi
done

./example.sh mcd.sh script.py example.sh

# find foobar in the following files, print date at the same time
```

-------

