---
title: "Python"
---

# Python

### Rename

Syntax:

Following is the syntax for rename() method:

```
os.rename(src, dst)
```

Parameters:

* src -- This is the actual name of the file or directory.
* dst -- This is the new name of the file or directory.

```python
# !/usr/bin/python

import os, sys

# listing directories
print "The dir is: %s"%os.listdir(os.getcwd())

# renaming directory ''tutorialsdir"
os.rename("tutorialsdir","tutorialsdirectory")

print "Successfully renamed."

# listing directories after renaming "tutorialsdir"
print "the dir is: %s" %os.listdir(os.getcwd())
```

### Readlines

Parameters: 
+ sizehint -- This is the number of bytes to be read from the file.

```python

# Open a file
fo = open("foo.txt", "rw+")
print "Name of the file: ", fo.name

# Assuming file has following 5 lines
# This is 1st line
# This is 2nd line
# This is 3rd line
# This is 4th line
# This is 5th line

line = fo.readlines()
print "Read Line: %s" % (line)

line = fo.readlines(2)
print "Read Line: %s" % (line)

# Close opend file
fo.close()
```

### Listdir

Parameters:
+ path -- This is the directory, which needs to be explored.

```python 
#!/usr/bin/python

import os, sys

# Open a file
path = "/var/www/html/"
dirs = os.listdir( path )

# This would print all the files and directories
for file in dirs:
   print file
```

### Basename

basename() returns a value equivalent to the second part of the split() value.

```python
import os.path

for path in [ '/one/two/three', 
              '/one/two/three/',
              '/',
              '.',
              '']:
    print '"%s" : "%s"' % (path, os.path.basename(path))
$ python ospath_basename.py

"/one/two/three" : "three"
"/one/two/three/" : ""
"/" : ""
"." : "."
"" : ""
```

### Dirname

dirname() returns the first part of the split path:

```python
import os.path

for path in [ '/one/two/three', 
              '/one/two/three/',
              '/',
              '.',
              '']:
    print '"%s" : "%s"' % (path, os.path.dirname(path))
$ python ospath_dirname.py

"/one/two/three" : "/one/two"
"/one/two/three/" : "/one/two/three"
"/" : "/"
"." : ""
"" : ""
```

### Traversing a Directory Tree

os.path.walk() traverses all of the directories in a tree and calls a function you provide passing the directory name and the names of the contents of that directory. This example produces a recursive directory listing, ignoring .svn directories.

```python
import os
import os.path
import pprint

def visit(arg, dirname, names):
    print dirname, arg
    for name in names:
        subname = os.path.join(dirname, name)
        if os.path.isdir(subname):
            print '  %s/' % name
        else:
            print '  %s' % name
    print

os.mkdir('example')
os.mkdir('example/one')
f = open('example/one/file.txt', 'wt')
f.write('contents')
f.close()
f = open('example/two.txt', 'wt')
f.write('contents')
f.close()
os.path.walk('example', visit, '(User data)')

```

```
$ python ospath_walk.py

example (User data)
  one/
  two.txt

example/one (User data)
  file.txt
```

## OS

Working with files

The built-in open function lets you create, open, and modify files. This module adds those extra functions you need to rename and remove files:

###Example: Using the os module to rename and remove files

```python
import os
import string

def replace(file, search_for, replace_with):
    # replace strings in a text file

    back = os.path.splitext(file)[0] + ".bak"
    temp = os.path.splitext(file)[0] + ".tmp"

    try:
        # remove old temp file, if any
        os.remove(temp)
    except os.error:
        pass

    fi = open(file)
    fo = open(temp, "w")

    for s in fi.readlines():
        fo.write(string.replace(s, search_for, replace_with))

    fi.close()
    fo.close()

    try:
        # remove old backup file, if any
        os.remove(back)
    except os.error:
        pass

    # rename original to backup...
    os.rename(file, back)

    # ...and temporary to original
    os.rename(temp, file)

#
# try it out!

file = "samples/sample.txt"

replace(file, "hello", "tjena")
replace(file, "tjena", "hello")
```

Working with directories

The os module also contains a number of functions that work on entire directories.

The listdir function returns a list of all filenames in a given directory. The current and parent directory markers used on Unix and Windows (. and ..) are not included in this list.

Example: Using the os module to list the files in a directory

```python
import os

for file in os.listdir("samples"):
    print file

sample.au
sample.jpg
sample.wav
...
```
The getcwd and chdir functions are used to get and set the current directory:

Example: Using the os module to change the working directory
```python
import os

# where are we?
cwd = os.getcwd()
print "1", cwd

# go down
os.chdir("samples")
print "2", os.getcwd()

# go back up
os.chdir(os.pardir)
print "3", os.getcwd()

1 /ematter/librarybook
2 /ematter/librarybook/samples
3 /ematter/librarybook
```
The makedirs and removedirs functions are used to create and remove directory hierarchies.

Example: Using the os module to create and remove multiple directory levels
```python
import os

os.makedirs("test/multiple/levels")

fp = open("test/multiple/levels/file", "w")
fp.write("inspector praline")
fp.close()

# remove the file
os.remove("test/multiple/levels/file")

# and all empty directories above it
os.removedirs("test/multiple/levels")
```

Note that removedirs removes all empty directories along the given path, starting with the last directory in the given path name. In contrast, the mkdir and rmdir functions can only handle a single directory level.

Example: Using the os module to create and remove directories
```python
import os

os.mkdir("test")
os.rmdir("test")

os.rmdir("samples") # this will fail

Traceback (innermost last):
  File "os-example-7", line 6, in ?
OSError: [Errno 41] Directory not empty: 'samples'
```
To remove non-empty directories, you can use the rmtree function in the shutil module.

###Working with file attributes

The stat function fetches information about an existing file. It returns a 9-tuple which contains the size, inode change timestamp, modification timestamp, and access privileges.

Using the os module to get information about a file
```python
import os
import time

file = "samples/sample.jpg"

def dump(st):
    mode, ino, dev, nlink, uid, gid, size, atime, mtime, ctime = st
    print "- size:", size, "bytes"
    print "- owner:", uid, gid
    print "- created:", time.ctime(ctime)
    print "- last accessed:", time.ctime(atime)
    print "- last modified:", time.ctime(mtime)
    print "- mode:", oct(mode)
    print "- inode/dev:", ino, dev

#
# get stats for a filename

st = os.stat(file)

print "stat", file
dump(st)
print

#
# get stats for an open file

fp = open(file)

st = os.fstat(fp.fileno())

print "fstat", file
dump(st)
```

```
stat samples/sample.jpg
- size: 4762 bytes
- owner: 0 0
- created: Tue Sep 07 22:45:58 1999
- last accessed: Sun Sep 19 00:00:00 1999
- last modified: Sun May 19 01:42:16 1996
- mode: 0100666
- inode/dev: 0 2

fstat samples/sample.jpg
- size: 4762 bytes
- owner: 0 0
- created: Tue Sep 07 22:45:58 1999
- last accessed: Sun Sep 19 00:00:00 1999
- last modified: Sun May 19 01:42:16 1996
- mode: 0100666
- inode/dev: 0 0
```

Some fields don’t make sense on non-Unix platforms; for example, the (inode, dev) tuple provides a unique identity for each file on Unix, but can contain arbitrary data on other platforms.

The stat module contains a number of useful constants and helper functions for dealing with the members of the stat tuple. Some of these are shown in the examples below.

You can modify the mode and time fields using the chmod and utime functions:


Using the os module to change a file’s privileges and timestamps

```python
import os
import stat, time

infile = "samples/sample.jpg"
outfile = "out.jpg"

# copy contents
fi = open(infile, "rb")
fo = open(outfile, "wb")

while 1:
    s = fi.read(10000)
    if not s:
        break
    fo.write(s)

fi.close()
fo.close()

# copy mode and timestamp
st = os.stat(infile)
os.chmod(outfile, stat.S_IMODE(st[stat.ST_MODE]))
os.utime(outfile, (st[stat.ST_ATIME], st[stat.ST_MTIME]))

print "original", "=>"
print "mode", oct(stat.S_IMODE(st[stat.ST_MODE]))
print "atime", time.ctime(st[stat.ST_ATIME])
print "mtime", time.ctime(st[stat.ST_MTIME])

print "copy", "=>"
st = os.stat(outfile)
print "mode", oct(stat.S_IMODE(st[stat.ST_MODE]))
print "atime", time.ctime(st[stat.ST_ATIME])
print "mtime", time.ctime(st[stat.ST_MTIME])
```

```
original =>
mode 0666
atime Thu Oct 14 15:15:50 1999
mtime Mon Nov 13 15:42:36 1995
copy =>
mode 0666
atime Thu Oct 14 15:15:50 1999
mtime Mon Nov 13 15:42:36 1995
```

Working with processes

The system function runs a new command under the current process, and waits for it to finish.

Using the os module to run an operating system command
```python
import os

if os.name == "nt":
    command = "dir"
else:
    command = "ls -l"

os.system(command)
```
```
-rwxrw-r--   1 effbot  effbot        76 Oct  9 14:17 README
-rwxrw-r--   1 effbot  effbot      1727 Oct  7 19:00 SimpleAsyncHTTP.py
-rwxrw-r--   1 effbot  effbot       314 Oct  7 20:29 aifc-example-1.py
-rwxrw-r--   1 effbot  effbot       259 Oct  7 20:38 anydbm-example-1.py
...
```
The command is run via the operating system’s standard shell, and returns the shell’s exit status. Under Windows 95/98, the shell is usually command.com whose exit status is always 0.

Warning: Since os.system passes the command on to the shell as is, it can be dangerous to use if you don’t check the arguments carefully (consider running os.system(“viewer %s” % file) with the file variable set to “sample.jpg; rm -rf $HOME”). When unsure, it’s usually better to use exec or spawn instead (see below).

The exec function starts a new process, replacing the current one (“go to process”, in other words). In the following example, note that the “goodbye” message is never printed:


Using the os module to start a new process
```python
import os
import sys

program = "python"
arguments = ["hello.py"]

print os.execvp(program, (program,) +  tuple(arguments))
print "goodbye"
```
```
hello again, and welcome to the show
```

Python provides a whole bunch of exec functions, with slightly varying behavior. The above example uses execvp, which searches for the program along the standard path, passes the contents of the second argument tuple as individual arguments to that program, and runs it with the current set of environment variables. See the Python Library Reference for more information on the other seven ways to call this function.

Under Unix, you can call other programs from the current one by combining exec with two other functions, fork and wait. The former makes a copy of the current process, the latter waits for a child process to finish.

Using the os module to run another program (Unix)

```python
import os
import sys

def run(program, *args):
    pid = os.fork()
    if not pid:
        os.execvp(program, (program,) +  args)
    return os.wait()[0]

run("python", "hello.py")

print "goodbye"
```
```
hello again, and welcome to the show
goodbye
```

The fork returns zero in the new process (the return from fork is the first thing that happens in that process!), and a non-zero process identifier in the original process. Or in other words, “not pid” is true only if we’re in the new process.

fork and wait are not available on Windows, but you can use the spawn function instead. Unfortunately, there’s no standard version of spawn that searches for an executable along the path, so you have to do that yourself:

Example: Using the os module to exit the current process
```python
import os
import sys

try:
    sys.exit(1)
except SystemExit, value:
    print "caught exit(%s)" % value

try:
    os._exit(2)
except SystemExit, value:
    print "caught exit(%s)" % value

print "bye!"
```
```
caught exit(1)
```

### CPU Usage
This Python script retrieves and prints the CPU usage percentage using the psutil.cpu_percent() function from the psutil module. 

```python
#!/usr/bin/env python3
import psutil

cpu_stats = psutil.cpu_percent(0.5)

print(cpu_stats)
```

### Disk Report

This Python script retrieves and prints disk usage information for the root ("/") directory using the shutil.disk_usage() function from the shutil module.

```python
#!/usr/bin/env python3
import shutil

du = shutil.disk_usage("/")

# Disk Space
print("Your Total Space ->", du)

# * Calculate amount of free disk space percentage
print("Free Disk Space % :", du.free / du.total * 100)
```

### Health Check

This Python script checks the disk usage and CPU usage of a system and prints an error message if either of the following conditions is met:

If the disk usage on the root ("/") partition is less than 20% free.
If the CPU usage is greater than or equal to 75%.
```python
#!/usr/bin/env python3

import shutil
import psutil


def check_disk_usage(disk):
    """Verifies that there's enough free space on disk"""
    du = shutil.disk_usage(disk)
    free = du.free / du.total * 100
    return free > 20


def check_cpu_usage():
    """Verifies that there's enough unused CPU"""
    usage = psutil.cpu_percent(1)
    return usage < 75


if not check_disk_usage("/") or not check_cpu_usage():
    print("Error!")
else:
    print("Everything is OK!", psutil.cpu_percent(1))
```

### Reading Environment Variables
```python
#!/usr/bin/env python3

import os

print('HOME: ' + os.environ.get('HOME', ''))
print('SHELL: ' + os.environ.get('SHELL', ''))
print('ROCK: ' + os.environ.get('ROCK', ''))
```

### Find Errors in Logs

This Python script is designed to search for specific errors in a log file and write the matching log entries to another file. 

```python
#!/usr/bin/env python3
import sys
import os
import re


def error_search(log_file):
    error = input("What is the error? ")
    returned_errors = []
    with open(log_file, mode='r', encoding='UTF-8') as file:
        for log in file.readlines():
            error_patterns = ["error"]
            for i in range(len(error.split(' '))):
                error_patterns.append(r"{}".format(
                    error.split(' ')[i].lower()))
            if all(re.search(error_pattern, log.lower()) for error_pattern in error_patterns):
                returned_errors.append(log)
        file.close()
    return returned_errors


def file_output(returned_errors):
    with open(os.path.expanduser('~') + '/data/errors_found.log', 'w') as file:
        for error in returned_errors:
            file.write(error)
        file.close()


if __name__ == "__main__":
    log_file = sys.argv[1]
    returned_errors = error_search(log_file)
    file_output(returned_errors)
    sys.exit(0)
```
### Logger

The logging module defines a standard API for reporting errors and status information from applications and libraries. The key benefit of having the logging API provided by a standard library module is that all Python modules can participate in logging, so an application’s log can include messages from third-party modules.

Logging in Applications

There are two perspectives for examining logging. Application developers set up the logging module, directing the messages to appropriate output channels. It is possible to log messages with different verbosity levels or to different destinations. Handlers for writing log messages to files, HTTP GET/POST locations, email via SMTP, generic sockets, or OS-specific logging mechanisms are all included, and it is possible to create custom log destination classes for special requirements not handled by any of the built-in classes.

Logging to a File

Most applications are probably going to want to log to a file. Use the basicConfig() function to set up the default handler so that debug messages are written to a file.

```python
import logging

LOG_FILENAME = 'logging_example.out'
logging.basicConfig(filename=LOG_FILENAME,
                    level=logging.DEBUG,
                    )

logging.debug('This message should go to the log file')

f = open(LOG_FILENAME, 'rt')
try:
    body = f.read()
finally:
    f.close()

print 'FILE:'
print body
```

After running the script, the log message is written to logging_example.out:

```python
$ python logging_file_example.py

FILE:
DEBUG:root:This message should go to the log file
```

Rotating Log Files

Running the script repeatedly causes more messages to be appended to the file. To create a new file each time the program runs, pass a filemode argument to basicConfig() with a value of 'w'. Rather than managing the creation of files this way, though, it is simpler to use a RotatingFileHandler:

```python
import glob
import logging
import logging.handlers

LOG_FILENAME = 'logging_rotatingfile_example.out'

# Set up a specific logger with our desired output level
my_logger = logging.getLogger('MyLogger')
my_logger.setLevel(logging.DEBUG)

# Add the log message handler to the logger
handler = logging.handlers.RotatingFileHandler(LOG_FILENAME,
                                               maxBytes=20,
                                               backupCount=5,
                                               )
my_logger.addHandler(handler)

# Log some messages
for i in range(20):
    my_logger.debug('i = %d' % i)

# See what files are created
logfiles = glob.glob('%s*' % LOG_FILENAME)
for filename in logfiles:
    print filename
```

The result should be six separate files, each with part of the log history for the application:
```bash
$ python logging_rotatingfile_example.py

logging_rotatingfile_example.out
logging_rotatingfile_example.out.1
logging_rotatingfile_example.out.2
logging_rotatingfile_example.out.3
logging_rotatingfile_example.out.4
logging_rotatingfile_example.out.5
```

The most current file is always logging_rotatingfile_example.out, and each time it reaches the size limit it is renamed with the suffix .1. Each of the existing backup files is renamed to increment the suffix (.1 becomes .2, etc.) and the .5 file is erased.

Verbosity Levels

Another useful feature of the logging API is the ability to produce different messages at different log levels. This code to be instrumented with debug messages, for example, while setting the log level down so that those debug messages are not written on a production system.

|Level	|Value
|--|--
|CRITICAL|	50
|ERROR|	40
|WARNING|	30
|INFO|	20
|DEBUG|	10
|UNSET|	0

The logger, handler, and log message call each specify a level. The log message is only emitted if the handler and logger are configured to emit messages of that level or higher. For example, if a message is CRITICAL, and the logger is set to ERROR, the message is emitted (50 > 40). If a message is a WARNING, and the logger is set to produce only messages set to ERROR, the message is not emitted (30 < 40).

```python
import logging
import sys

LEVELS = { 'debug':logging.DEBUG,
            'info':logging.INFO,
            'warning':logging.WARNING,
            'error':logging.ERROR,
            'critical':logging.CRITICAL,
            }

if len(sys.argv) > 1:
    level_name = sys.argv[1]
    level = LEVELS.get(level_name, logging.NOTSET)
    logging.basicConfig(level=level)

logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical error message')
```

Run the script with an argument like ‘debug’ or ‘warning’ to see which messages show up at different levels:

```python
$ python logging_level_example.py debug

DEBUG:root:This is a debug message
INFO:root:This is an info message
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical error message

$ python logging_level_example.py info

INFO:root:This is an info message
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical error message
```

Logging in Libraries

Developers of libraries, rather than applications, should also use logging. For them, there is even less work to do. Simply create a logger instance for each context, using an appropriate name, and then log messages using the stanard levels. As long as a library uses the logging API with consistent naming and level selections, the application can be configured to show or hide messages from the library, as desired.

Naming Logger Instances

All of the previous log messages all have ‘root’ embedded in them. The logging module supports a hierarchy of loggers with different names. An easy way to tell where a specific log message comes from is to use a separate logger object for each module. Every new logger inherits the configuration of its parent, and log messages sent to a logger include the name of that logger. Optionally, each logger can be configured differently, so that messages from different modules are handled in different ways. Below is an example of how to log from different modules so it is easy to trace the source of the message:

```python
import logging

logging.basicConfig(level=logging.WARNING)

logger1 = logging.getLogger('package1.module1')
logger2 = logging.getLogger('package2.module2')

logger1.warning('This message comes from one module')
logger2.warning('And this message comes from another module')
```

And the output:
```python
$ python logging_modules_example.py

WARNING:package1.module1:This message comes from one module
WARNING:package2.module2:And this message comes from another module
```

There are many, many, more options for configuring logging, including different log message formatting options, having messages delivered to multiple destinations, and changing the configuration of a long-running application on the fly using a socket interface. All of these options are covered in depth in the library module documentation.
