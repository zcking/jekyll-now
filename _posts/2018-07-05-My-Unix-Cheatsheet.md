---
layout: post
title: My Bash & Linux Cheatsheet
categories: programming
comments: true
---

I'll try to make this post brief. Earlier today, I was thinking of all
the things I learned during my internship
as a DevOps Engineer, specifically *nix-related things. During this bit of
nostalgia, I considered how useful it would've been having certain things
compiled into one handy list--or cheatsheet--which I could've refered to. So
without further ado, please enjoy *my* version of a Linux/Bash cheatsheet; I
hope you enjoy it!

{% include alert.html content="<b>Note</b> if you want, you can download this web page by right-clicking in your browser
and choosing `Save Page As`." level="info" %}

&nbsp;

---

## Common Terminology

| Term | Explanation |
| ---- | ----------- |
| directory | another word for a "folder" but really more appropriate; "folder" is the more visual innuendo for graphically-based operating systems (e.g. Windows) |
| path | the route to a file or directory on the machine (e.g. `/home/zcking/hello_file.txt`) |
| root (path) | the base directory the file system is mounted on; path denoted by starting with a slash (e.g. `/`); e.g. a file `hello.txt` in the root directory would have the full path of `/hello.txt` |
| stdout | standard output stream; used by programs to output text/data; can be redirected to files or other programs; normally directed to the active terminal session running the program |
| stderr | standard error stream; used by programs to output error text/data; can be redirected to files or other programs |
| stdin | standard input stream; used by programs to receive input text/data; can be redirected to come from other files or programs |
| process | running/active program; has a Process Identifier (PID) that acts as a handle |


## Very Common Bash Commands

| Command | Description |
| ------- | ----------- |
| `ls` | lists files in current directory |
| `cd <path>` | changes the current directory to `<path>` (e.g. "moves into a folder") |
| `less <file>` | outputs the contents of a file in a scrollable/interactive manner |
| `cat <file>` | outputs the contents of a file to the terminal's stdout (non-interactive) |
| `head <file>` | outputs the top 10 lines of a file to the stdout; use `head -n <num> <file>` to outuput the first `num` lines |
| `tail <file>` | outputs the last 10 lines of a file to the stdout; use `tail -n <num> <file>` to output the last `num` lines |
| `sudo <command>` | run `<command>` as the root user; necessary for commands requiring elevated privileges etc. take high precaution when doing this |
| `sudo su - ` | switch to the root user; take high precaution when doing this |
| `man <command>` | view the "man" page ("man" = "manual") for the `<command>`; useful for learning optional flags and command usages |
| `cp <file1> <file2>` | copy `file1` to `file2` |
| `mv <file1> <file2>` | move/rename `file1` to `file2` |
| `mkdir <name>` | make a new directory called `name` |
| `which <app>` | show which path will be used to an application/command by default |
| `df` | show disk usage |
| `du` | show directory space usage |
| `free` | show memory and swap usage |
| `top` | display all running processes |
| `ps` | display *your* currently-running processes |
| `wget <url>` | download a file over HTTP, HTTPS, or FTP |
| `curl <url>` | transfer a URL; supports *several* protocols including HTTP and FTP |
| `ssh -i <pem_key> <user>@<host>` | connect to `<host>` as user `<user>`, using a PEM key `<pem_key>` |
| `tar cf <file>.tar <files>` | create a tar (archive) |
| `tar xf <file>.tar` | extract the files from a tar |
| `tar czf <file>.tar.gz <files>` | create a tar with Gzip compression |
| `tar xzf <file>.tar.gz` | extract the files from a tar with Gzip compression |
| `gzip <file>` | compress the file `<file>` and rename it to `<file>.gz` |
| `gzip -d <file>.gz` | uncompresses a Gzip-compressed file and renames it to `<file>` |
| `vim <file>` | edit a file with the terminal text editor, **Vim** |

&nbsp;

---

## Variables
In Bash, variables are very common. To create a variable yourself:  
```bash
varname=value
```

Then to get the value of the variable:
```bash
echo $varname
```

{% include note.html content="Variables in bash are essentially *all* stored as *strings*. Be careful, knowing that." %}

### Special Variables
Here are some of the more unique global variables available in bash:
```bash
# The number of arguments given to the script/function
echo $#

# Example:
function numargs() { echo $#; }
numargs foo bar bang # outputs 3

---

# The exit code of the last command/program
echo $?

# Example:
sleep 1
echo $? # Outputs 1

---

# The process ID of the last command/program backgrounded
echo #!

# Example:
sleep 60 &
echo $! > myprocess.pid # Write the PID (e.g. '5170') to a file
```


---

## Access Permissions
Access to files and directories is controlled through their permissions.

### Viewing permissions
To view the permissions for files and directories in path, you can use
`ls -l` to list the files and directories in a *long* format, which includes the
permissions levels. An example output of this might be:  
```
-rw-rw-r--  1 zcking zcking   348 Jun 23 15:12 404.md
-rw-rw-r--  1 zcking zcking  1351 Jun 24 14:45 about.md
-rw-rw-r--  1 zcking zcking  1740 Jun 24 16:47 artwork.html
drwxrwxr-x  3 zcking zcking  4096 Jun 23 15:45 assets
```

The first column of the output is the permissions levels. This column's value
is actually broken up into three sections: the owner's permissions, the
group's permissions, and all other users' permissions. There are three character
slots for each of these three sections, denoted by `r`, `w`, and `x`. `r` means
the corresponding user(s)/group can *read* the file; `w` means they can write,
or modify the file; and `x` means they can execute the file as an executable program.

The third column is the owner of the file/directory, and the fourth column is the
group it belongs to. Therefore, if you look at the `404.md` file, you can see
the owner and group for it are both `zcking` (me); as the owner, the user `zcking`
can read and write from/to the file, and likewise for the group. However, because the
third section is `r--`, all other users can oly *read* the file.

### Changing permissions
To change the permissions on a file or directory, two commands are commonly used:  
```
chmod <code> <path>
```
AND
```
chown <user>:<group> <path>
```

With `chmod` you specify the permissions (`rwx`) for the owner, group, and other
users via an integer representation. See, the three characters `rwx` are actually
represented as binary, because each character/permission is just a toggle-able
flag. For example, `r-x` would equal `101` in binary, which is `5` in decimal.
Here's another example: `rw-` equals `110` in binary and `6` in decimal. So if
you wanted to grant the owner and group read, write, and execute permissions,
but all other users only read-access, you would write  
```
chmod 774 /path/to/my_file
```

{% include alert.html content="<b>Note</b> you might need elevated privileges to change the permissions on a file or
directory; to do this prefix the command with `sudo` like so: `sudo chmod <code> <path>`." level="info" %}

With `chown`, you can quickly change the ownership of a file or directory. If you
wanted to change the ownership of a file `my_file.txt`, you would run something
like the following:  
```
chmod zcking:zcking my_file.txt
```

There's a lot more you can learn regarding permissions, especially little
details that make matters more convenient (e.g. `chmod u+x my_file.sh`), but
I won't dive too deep into it here; if you're interested in this more, check
out [this wiki on file permissions and attributes](https://wiki.archlinux.org/index.php/File_permissions_and_attributes).

&nbsp;

---


## Processes
Processes are the handles for the applications themselves in \*nix systems,
and are very commonly handled directly when operating in the terminal.

### Start a process
Starting a process is ultimately done by running an executable. An executable is
any *executable* script, or program, on the machine. To run that, you simply
specify the path to the file; e.g. `./my_script.sh`.

### List running processes
To view the currently running processes you can use the `ps` command. This
command has a *lot* of options for changing what output you get from it, but
I personally prefer the `ps aux` usage. This looks something like so:  
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 119652  5676 ?        Ss   22:31   0:01 /sbin/init splash
root         2  0.0  0.0      0     0 ?        S    22:31   0:00 [kthreadd]
root         4  0.0  0.0      0     0 ?        S<   22:31   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S<   22:31   0:00 [mm_percpu_wq]
root         7  0.0  0.0      0     0 ?        S    22:31   0:00 [ksoftirqd/0]
root         8  0.0  0.0      0     0 ?        S    22:31   0:03 [rcu_sched]
root         9  0.0  0.0      0     0 ?        S    22:31   0:00 [rcu_bh]
root        10  0.0  0.0      0     0 ?        S    22:31   0:00 [migration/0]
root        11  0.0  0.0      0     0 ?        S    22:31   0:00 [watchdog/0]
```

The column headers are self-explanatory. The most important ones usually
are `PID` and `COMMAND`; that tells you what is actually running as well as
the PID which can then use to kill the process if needed.

A useful trick for finding the PID, or checking if *there is* a process running,
for something, is to list the processes and then search for the command using `grep`.
You can do this with  
```
ps aux | grep <command>
```

That lists the processes and searches for the word `<command>` in the output. For
example, I could run `ps aux | grep splash` and get the following output:  
```
root         1  0.0  0.0 119652  5676 ?        Ss   22:31   0:01 /sbin/init splash
zcking    9443  0.0  0.0  14228   988 pts/1    S+   23:48   0:00 grep --color=auto splash
```

{% include note.html content="the `grep` command is also listed here since it contains the search-term *splash*" %}


### Killing a process
If you wish to kill/halt/shutdown a process, you use the `kill` command.
This command's usage is `kill <PID>` but I prefer to use `kill -9 <PID>`. The
`-9` is an alternate kill signal, but it basically ensures the process
will **definitely** get stopped.

Another way to kill a process, is using the `pkill` command:  
```
pkill -f python
```
That will kill all the processes with the command `python`.

My preferred method, however, when spawning a process is to write the PID 
to a file upon starting the new process. Then you can easily kill the process 
*using* that PID file; to do that, you use the `pkill -F <PID file>` command.  Example:
```bash
sleep 600 & # Start a long-running process
echo $! > sleeper.pid # Write the PID to a file
pkill -F sleeper.pid # Kill the process using the PID file
```

*Note:* `sudo` may be needed to kill processes.

### Backgrounding a process
By default, when executing a program from the terminal, you'll have to
wait for it to complete; however, you can tell the process to run
in the background and not block up your workflow in the terminal.

To run a process in the background when executing it, just slap an ampersand on
the end of the command:  
```
python /home/zcking/hello.py &
```
This will run the command's process in the background and output the PID, then
immediately return the terminal's stdin, stdout, etc. to you.

If you're *already running* a program (e.g. you forgot to put the `&`), you
can hit Ctrl+Z (or Cmd+Z on Mac) to background the process. Doing this won't
give you the PID, but you can easily find that using the `ps aux | grep <command>`
method.

&nbsp;

---

## Redirecting `stdout` and `stderr`
Most often, your programs you run will have some form of output. This output
typically gets sent to the standard output stream or standard error stream,
called `stdout` and `stderr` respectively. For example, most programming languages
offer a `print()` function; here's some examples:

| Language | How to print to stdout |
| -------- | ---------------------- |
| C++ | `std::cout << "Hi."; ` |
| Python | `print("Hi.")` |
| Java | `System.out.println("Hi.");` |

When you do this, that output is actually going to `stdout`; you see it
appear in your terminal when running the program because by default
`stdout` is the buffer you're looking at for your terminal session.

However, sometimes we don't want the output appearing in our terminal. Perhaps
you'd like to redirect it to a file to act as a log file for the application.

To redirect stdout from a program to a file, you can use the `>` or the `>>`
operator.

### Overwriting the file you direct output to
The `>` operator will **overwrite** the file you redirect to. So if the
file exists prior to running the command, its contents will be replaced by
the new output. This may not be behavior you are looking for with
log-type files, since you probably want to persist the output.

Example:  
```bash
python my_script.py > output.txt
```

### Appending to the file you direct output to
The `>>` operator will **append** to the file, as opposed to overwriting it.
This is useful if you wish to keep using the same file without losing old
output.

Example:  
```bash
python my_script.py >> output.txt
```

### Redirect *both* `stdout` and `stderr` to a file
Using `>` or `>>` alone won't do much for `stderr`, which is where
your program *should* be outputting its error output. To redirect both
streams to the same file you would use `2>&1` in combination with the `>` or `>>`
operator; this tells the OS to redirect `stderr` (file descriptor = 2) to
the `stdout` stream (file descriptor = 1). For example:  
```bash
python my_script.py > output.txt 2>1&
```

{% include alert.html content="<b>Note</b> doing this still locks up your terminal until the program
finishes; therefore I personally prefer to background the process at the
same time: `python my_script.py > output.txt 2>&1 &`. If I want to
see the output of the program as it's running, I'll use `tail -fn 100 output.txt`
which shows the last 100 lines of the file and follows it so new output will
show up live as the program runs" level="info" %}

{% include note.html content="for more information on standard streams,
check out [this wiki](https://en.wikipedia.org/wiki/Standard_streams)" %}

&nbsp;

---

## Basic File Hierarchy
One of my favorite things about Linux is how it organizes files into
a standardized, well-organized hierarchy. Here is the basic, common directory
names used in Linux and what they *should* contain:

| Directory Name | Description |
| -------------- | ----------- |
| `bin/` | binary files; executables; scripts and programs |
| `lib/` | libraries for use by binaries |
| `src/` | source code |
| `tmp/` | temporary files; often not preserved between system reboots |
| `opt/` | optional application software packages |
| `etc/` | configuration files |
| `var/` | variable files; files expected to change during normal operations (e.g. log files) |


Furthermore, there are specific directories for certain things in the OS:

| Directory | Description |
| --------- | ----------- |
| `/bin/` | essential command binaries (e.g. `cat`, `less`, `ls`, etc.) |
| `/dev/` | device files (e.g. `/dev/null`) |
| `/home/` | users' home directories, containing personal files |
| `/root/` | the root user's home directory |
| `/var/log/` | log files |
| `/var/run/` | run-time variable data (e.g. PID files) |
| `/usr/` | secondary hierarchy for read-only user data; contains most (multi-)user utilities and applications |


{% include note.html content="for more information on unix file structure,
check out [this wiki](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)" %}
