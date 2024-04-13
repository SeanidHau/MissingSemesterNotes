# Lecture 1 Course Overview & The Shell

##  Official note

###  English version

[Course overview + the shell · Missing Semester (mit.edu)](https://missing.csail.mit.edu/2020/course-shell/)

###  Chinese version

[课程概览与 shell · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/2020/course-shell/)

##  Basic Commands

1.  `date`  get time

   ```bash
   # sample
   ~# date
   Tue Jul  4 12:15:57 HKT 2023
   ```

2.  `echo `  print text or the value

   ``` bash
   # sample
   ~# echo "hello world"
   hello world
   
   # sample
   ~# foo=bar
   ~# echo $foo
   bar
   ```

   ==notice== In Bash, some symbols may have some special meanings. So you should pay more effort to it. Read [Quoting (Bash Reference Manual) (gnu.org)](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) to know more about it.

   ==notice== You can use `echo $PATH` to show all the path.

3. `which` locate the path of an executable program in the system

   ``` bash
   # sample
   ~# which python
   /user/bin/python
   ```

4. `pwd` print working directory.

   ``` bash
   # sample
   ~# pwd
   /tmp/missing
   ```

5. `cd` change the current working directory

   ``` bash
   # sample
   cd tmp
   /tmp#
   
   # sample 
   # change to the home directory
   cd
   
   # sample
   # chenge to the parent directory
   cd ..
   
   # sample
   # change to the previous directory
   cd -
   /root
   ```
   
   ==notice== path is a unique location or address that identifies the location of a file or directory in a file system.
   
6. `ls`  list the files and directories in the current working directory.

   ``` bash
   # sample
   /# ls
   bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
   boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
   
   # sample
   # using -l to list files and directories in the long format
   ls -l
   total 1472
   lrwxrwxrwx     1 root root       7 May  2 05:34 bin -> usr/bin
   drwxr-xr-x     2 root root    4096 Apr 18  2022 boot
   drwxr-xr-x     8 root root    3020 Jul  4 12:15 dev
   drwxr-xr-x    73 root root    4096 Jul  4 12:15 etc
   drwxr-xr-x     3 root root    4096 Jul  2 00:56 home
   -rwxr-xr-x     3 root root 1440152 May  7  2022 init
   lrwxrwxrwx     1 root root       7 May  2 05:34 lib -> usr/lib
   lrwxrwxrwx     1 root root       9 May  2 05:34 lib32 -> usr/lib32
   lrwxrwxrwx     1 root root       9 May  2 05:34 lib64 -> usr/lib64
   lrwxrwxrwx     1 root root      10 May  2 05:34 libx32 -> usr/libx32
   drwx------     2 root root   16384 Apr 11  2019 lost+found
   drwxr-xr-x     2 root root    4096 May  2 05:34 media
   drwxr-xr-x     7 root root    4096 Jul  2 00:39 mnt
   drwxr-xr-x     2 root root    4096 May  2 05:34 opt
   dr-xr-xr-x 13073 root root       0 Jul  4 12:15 proc
   drwx------     2 root root    4096 Jul  2 00:46 root
   drwxr-xr-x     6 root root     140 Jul  4 12:15 run
   lrwxrwxrwx     1 root root       8 May  2 05:34 sbin -> usr/sbin
   drwxr-xr-x     8 root root    4096 May  2 05:36 snap
   drwxr-xr-x     2 root root    4096 May  2 05:34 srv
   dr-xr-xr-x    11 root root       0 Jul  4 09:15 sys
   drwxrwxrwt     3 root root    4096 Jul  2 12:52 tmp
   drwxr-xr-x    14 root root    4096 May  2 05:34 usr
   drwxr-xr-x    13 root root    4096 May  2 05:35 var
   ```

   ==notice== In the last list, ***"d"*** means ***directory***, ***"l"*** means a ***symbolic link***.  ***"r"*** means ***read permission***, ***"w"*** means ***write permission***, ***"x"*** means ***execute permission***. Every three letters are divided into one group, ***owner permissions***, ***group permissions*** and ***other permissions***. However, things will be different while the file is a directory, ***"r"***means allow listing the contents of the directory , ***"w"*** means allow creating, deleting, and renaming files or directories within the directory. ***"x"***  means  allow accessing and traversing into the directory.

7. `mv` move or rename files and directories in the command line interface.

   ``` bash
   # sample
   # move file to a new location
   mv source_file destination_directory
   
   # sample
   # Rename a file or directory
   mv old_name new_name
   
   # sample
   # Move multiple files to a directory
   mv file1 file2 file3 destination_directory
   
   # sample
   # Move and overwrite files
   mv -f source_file destination_file
   ```

8. `cp`  copy files and directories in the command line interface

   ``` bash
   # sample
   # Copy a file to a new location
   cp source_file destination_directory
   
   # sample
   # Copy multiple files to a directory
   cp file1 file2 file3 destination_directory
   
   # sample
   # Copy a directory and its contents recursively
   cp -r source_directory destination_directory
   
   # sample
   # Copy a file and preserve its attributes
   cp -p source_file destination_file
   
   # sample
   # Copy files and directories interactively
   cp -i source_file destination_file
   ```

9. `rm` remove or delete files and directories in the command line interface.

   ``` bash
   # sample
   # Remove a file
   rm file
   
   # sample
   # Remove multiple files
   rm file1 file2 file3
   
   # sample
   # Remove a directory and its contents
   rm -r directory
   
   # sample
   # Prompt for confirmation
   rm -i file
   ```

   ==notice== A directory can't be deleted unless uses the flag -r 

10. `rmdir` remove or delete empty directories in the command line interface.

    ``` bash
    # sample
    rmdir directory
    ```

11. `mkdir` create directories in the command line interface.

    ```bash
    # sample
    mkdir directory
    ```

12. `man` provides detailed information about the usage, options, and examples of a particular command.

    ```bash
    # sample
    man command
    ```

    ==notice== Press q to quit. And you can use `clear` or `Ctrl+L` to clear your terminal and back to the top.

13. `cat` display the contents of files in the command line interface. 

    ``` bash
    # sample
    cat file
    ```

14. `tail` display the last part of a file or continuously monitor a file for new data in the command line interface. It is often used to view the most recent entries in log files or to monitor real-time changes in files.

    ```bash
    # sample
    # displays the last 10 lines of the specified file
    tail file
    
    # sample
    # displays the last 5 lines of the specified file
    tail -n 5 file
    ```

15. `sudo` execute commands with administrative or superuser privileges.

    ```bash
    # sample
    sudo command
    ```

16. `tee` read from standard input and write to both standard output and one or more files simultaneously.

    ```bash
    # sample
    # takes the output of command (or a pipeline of commands) and displays it on the terminal. At the same time, it writes the output to the specified file. If the file already exists, it will be overwritten. If it doesn't exist, a new file will be created.
    command | tee file
    ```

    ```bash
    # A wrong sample
    sudo echo 500 > brightness
    brightness: permission denied
    
    # The right way
    echo 500 | sudo tee brightness
    ```

17. `xdg-open` open a file or URL with the default application associated with its file type or protocol. 

    ```bash
    # sample
    xdg-open file_or_url
    xdg-open document.pdf
    xdg-open directory
    xdg-open https://www.example.com
    ```

## Some knowledge

###  Stream

input stream & output stream

###  >, < & >>

\> redirect the output of a command to a file, overwriting the file if it already exists.

``` bash
# sample
command > output.txt
```

< redirect the input of a command from a file instead of the standard input.

```bash
# sample
command < input.txt
```

\>> redirect the output of a command and append it to the end of an existing file.

```bash
# sample
command >> output.txt
```

-------

