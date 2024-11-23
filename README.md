# Linux Command Cheatsheet

## Package Managers

Debian (Ubuntu) uses `apt` | RedHat (CentOS) uses `yum`

## List processes

```
ps aux
# a = show processes for all users
# u = display the process's user/owner
# x = also show processes not attached to a terminal
```

## List processes by CPU usage

```
top
```

### Amount of free memory

```
free
```

### Memory, paging, processes, IO, CPU, and disk scheduling

```
vmstat
```

### List files

```
ls
```

## Long List, all files, sort by timestamp

```
ls -lat
# long list
# all files
# sort by timestamp
```

### List files in another directory

```
ls ~
ls /
lr -r
# recursive
```

# Shutdown Program

#### SIGNUP (SIGnal Hang Up)

```
sudo kill <processId>
sudo kill mongod
```

### Forcefully Terminate (SIGKILL)

```
sudo kill -9 <processId>
sudo kill mongod
```

### Make directories

```
mkdir one two three
```

### Make nested directories

```
mkdir -p one/two
```

### Remove directories

```
rm -r dir1 dir2
```

### Force deletion/surpress errors

```
rm -f dir1 dir2
```

### Create file

```
touch one.txt two.txt three.txt
```

### Remove files

```
rm file.txt file2.txt
```

### How long the system's been up for

```
uptime
```

### Network & IO

```
iostat
netstat -a
```

### Services (daemons)

```
sudo systemctl enable mongod
sudo systemctl start mongod
sudo service mongod start
systemctl status mongod
```

### List all environment variables

```
printenv
```

### Return last status code

```
echo $?
```

#### Install sysstat

Free swap

```
sar -S
```

### List drives/devices

```
sudo lsblk
sudo blkid
suod df -h
```

### Print Working Directory

```
pwd
```

### Determine type of file

```
file one.txt
```

### Display history of commands

```
history
```

### Search History

#### grep = Global Regular Expression Print

```
history | grep git
```

### Search History with spaces

```
history | grep 'git commit'
```

### Recall command (enter to execute)

```
history !5
```

### Clear history

```
history -c
```

### Get the word count from a file

`wc filename`

### Type of command

```
type ls
whatis cp
```

### Multiple commands/same line (semi-colon executes all valid cmds)

`mkdir folder1;touch file1;mv file1 folder1`

### `&&` short circuits remaining commands upon an invalid command

`cal && DATE && mkdir testing`

### Copies files starting with an alpha carchater to dir1

`cp f?l*.txt dir1`

```
apples, bananas, carrots, hundred, face, 123
cp [abc]* dir1
```

### Copies files not starting with alpha cars to dir2

```
apples, bananas, carrots, hundred, face, 123
cp [!abc]* dir2
```

### Copy filenames starting with an f to dir1

```
file1.txt, file2.txt, file3.txt, job.txt
cp f?l\*.txt dir1
```

### Copy filenames beginning with an uppercase letter

`cp [[:upper:]]* dir1`

### Copy files beginning with a lowercase character

`cp [[:lower:]]* dir1`

### Copy files beginning with an alpha character

`cp *[[:alpha:]] dir1`

### Copy files beginning with a digit

`cp *[[:digit:]] dir1`

### copy files beginning with alpha-numeric characters

`cp *[[:alnum:]] dir1`

### Copy files beginning with digit and ending with alpha character

`cp [[:digit::]]*[[:alpha:]] dir1`

### Copy files which do not begin with a digit

`cp [![::digit:]]* dir1`

### Add Alias

`vi ~/.bashrc`

### Create alias command

#### type `todo` - check if command exists, first

```
alias todo=”cd Desktop; vi file.txt”
alias rename=”mv”
```

### Delete an alias

```
unalias rename
```

`tar` = Tape Archive

Unpack a `tar` file

- x = extracts from zip file
- v = verbose
- z = decompress files
- f = filename you’re working with

`tar -xvf file.tar`

### Execute File System Check on Startup

`sudo touch /forcesck`

### Kernal Data

`uname -a`
