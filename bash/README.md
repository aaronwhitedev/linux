# Bash

## Declare Bash Script

```
#!/bin/bash
echo "Scripting"
```

[Bash variables](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#Bash-Variables)  
[Bourne Variables](https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Variables.html#Bourne-Shell-Variables)

## Bash script permissions

Bash scripts require execute AND read permissions (shell needs read access).

## Linux permissions

Read=4  
Write=2  
Execute=1

`chmod 744 file.sh`  
chmod can use an octal number (https://chmod-calculator.com)

## Permissions can be combined

Read+Write+Execute=7  
Read+Write=6  
Read+Execute=5  
Write+Execute=3

## Linux files/directories have three permission categories

Linux permissions  
`r` read  
`w` write  
`x` execute  
`-` no permissions

Every file and directory has three permission categories - `Owner`, `Group`, and `Other`. Permissions are displayed with three characters per category, not seperated with spaces.

The Linux command `ls -l` displays file and directory permissions  
`744` is displayed as `rwxr--r--`  
`750` is displayed as `rwxrx----`

For `744` the permissions are:

```
Owner = rwx
Group = r--
Other = r--

rwx r-- r-- # Combined with space
rwxr--r--   # Combined without space
```

## Terminology

IFS = Internal Field Separator  
Display the current separator(s) `echo ${IFS@Q}`

Parameter Expansion `${variable}`  
Command substitution `$(command)`

### Control Operators

Used to control how the command line is processed  
`Newline` `|` `||` `&` `&&` `;` `;;` `;&` `;;&` `|&` `(` `)`

### Redirection Operators

`<` `>` `<<` `>>` `<&` `>|` `<<-` `<>` `>&`

Control operators and Redirection characters only matter if they're unquoted.

### Outputs

`0` = standard input (stdin)  
`1` = standard output (stdout)  
`2` = standard error output (stderr)

`?$` displays last status code

```
ls /not/here
echo $?
```

Returns `2` (stderr)

Single `>` truncates existing data  
`cd /root > error.txt` (`stdout` to `error.txt` file won't work)  
`cd /root 2> error.txt` (`stderr` to `error.txt` works)  
`cd /root &> /dev/null` (`stdout` and `stderr` sends the response to the bit-bucket and immediately gets deleted)

`>>` appends to existing file

```
touch test.txt
echo `1` >> test.txt
```

`>> 2>>` appends to existing file  
`>> &>>` appends to existing file

# Arrays

### Arrays are separated with spaces

`numbers=(0 1 2 3 4)`

### Display all elements in an array

```
echo "${numbers[@]}"

# 0 1 2 3 4
```

### Display first element in array

`echo "${numbers}"`

### Display array index numbers

`echo "${!numbers[@]}"`

### Displays all elements including invisible new line

`echo “${numbers[@]@Q}”`

### Loop over array

```
# array.sh
numbers=(0 1 2 3 4 5)
for element in "${numbers[@]}"; do
	echo "${element}"
done
```

## Create file (with line breaks) and read into array

`printf "# file.sh\n1\n2\n3\n4\n5\n" > file.txt`

### Read into array -t removes trailing new lines

```
# files.sh
readarray -t files < file.txt
for file in “${files[@]}”; do
	touch $file
done
```

# Strings

```
name="John"
echo ${name,}  # convert first letter to lowercase
echo ${name,,} # convert string to lowercase
echo ${name^}  # convert first letter to uppercase
echo ${name^^} # convert string to uppercase
echo ${#name}  # length of string
echo ${name~}  # reverses the case for the first letter
echo ${name~~} # reverses the case for every letter
```

## Substring expansion / slicing

`${parameter:offset:length}`

```
numbers=0123456789
echo "${numbers:3}"
# 3456789

echo “${numbers:0:5”}
# 01234

echo “${numbers:4:2”}
# 45
```

Negative offset requires space

```
echo “${numbers: -3}"
# 789

echo “${numbers: -3:2}"
# 78
```

## Sequence Generation

```
echo {1..10}
# 1 2 3 4 5 6 7 8 9 10

# {start,end,increment}
echo {1..10..2} # 1 3 5 7 9
```

## Brace Expansion

```
echo month{01..12}
# results in month01 month02 month03 month04 month05 month06 month07 month08 month09 month10 month11 month12
```

`bc` is Basic Calculator command (scale displays number of spaces)

#### `5` divided by `2` with `10` decimal places

```
echo "scale=10; 5/2" | bc
# results in 2.5000000000
```

## Globbing

Globbing is only performed on words (not operators). Globbing patterns are words that contained unquoted Special Pattern Characters.  
`*` `?` `[` `]` `!`

Match files ending with a question mark

```
touch file?
ls *\?
```

Display files of one character in length

```
touch 1
ls ?
```

Display files with two characters in length

```
touch 11
ls ??
```

Display files with two chars, starting with `a`

```
touch ab.txt
ls a?.txt
```

Other ways to filter

```
ls c[eioau]t
ls c[ueoai]t
ls *[[:digit:]]
```

## Exclude exactly one character

```
[!aeiou]*
# baseball
# cricket
```

Matches all files starting with a, b, c, d, e, f, or g  
`ls [a-g]*`

Matches all files starting with 3, 4, 5, or 6  
`ls [3-6]`

# other filters

```
[[:alpha:]]
[[:alnum:]]
[[:digit:]]
[[:lower:]]
[[:space:]]
[[:upper:]]

```

# Positional Parameters

Special variable `$@` contains everything but first positional parameter (first parameter is the file name)

```
# special.sh
#/bin/bash
echo “$@”
```

Invoke script `./special.sh 1 2 3`
Returns `1 2 3`

Invoke script `./special.sh “test one” “test two”`

## Metacharaters:

`|` `&` `;` `(` `)` `<` `>` space tab newline

`"$*"` places first character of IFS variable between each positional parameter
Unquoted `$*` has no difference from `$@`.

```
#!/bin/bash
# special2.sh

IFS=,
echo “$\*”
```

Invoke script `./special2.sh 1 2 3`

Returns `1,2,3`

# Logical Operators

### Integer Test Operators

`arg1 -eq arg2` equal
`arg1 -ne arg2` not equal
`arg1 -lt arg2` less than
`arg1 -gt arg2` greater than
`arg1 -leq arg2` less than or equal
`arg1 -geq arg2` greater than or equal

`[ 2 -eq 2 ]; echo $?`

### String Test Operators

`=` equality  
`!=` inequality  
`-z` string is empty  
`-n` string is not empty

```
first=joe
last=smith
[[$first = $last]] ; echo $?
# 1
```

```
[[$first != $last]] ; echo $?
# 0
```

Logical operator AND `-a`
Logical operator OR `-o`

Check if both conditions are true
`if [ $a -gt 60 -a $b -lt 100]`

`PS1` variable contains the prompt string show in the terminal.
`PS4` controls expands/displays before each command during execution trace
`echo $PS1`
`echo $PS4`

### File Operators

`-d` is file is a directory
`-e` does file exists
`-f` does file exists and is a regular file
`-r` file is readable by you
`-s` file exists and is not empty
`-w` file is writable by you
`-x` file is executable by you

### Bash Process Steps

1. Tokenization
2. Command Identification
3. Expansions
4. Quote Removal
5. Redirection

### Loops

```
#!/bin/bash
for COLOR in red green blue
do
echo “COLOR: $COLOR”
done
```

### Select

```
#!/bin/bash
select day in mon tue wed th fri sat sun;
do
	echo "The day of the week is $day"
done
```

### Read Input

```
read -p “What’s your name?: ” USER
echo “User: $USER”
```

# Other Stuff

### Chain multiple commands with semicolon

`cp test.txt /tmp/bak ; cp test.txt /tmp`

```
HOST=”google.com”
ping -c 1 $HOST
# -c sends just one packet
```

Functions are NOT called with parentheses

```
#!/bin/bash
function hello() {
	echo “Hello”
	now
}

function now() {
	echo “It’s $(date +%r)”
}

hello
```

## Stages of Expansion

Stage 1. Brace Expansions  
Stage 2.

- Parameter Expansion
- Arithmetic Expansion
- Command Substition
- Tilde Expansion

Stage 3. Word Splitting  
Stage 4. Globbing

## Logging:

Facilities: `kern`, `user`, `mail`, `daemon`, `auth`, `local0`-`local7`  
Severities: `emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`

```
#!/bin/bash
while true
do
	command N
	sleep 1
done
```

```
#!/bin/bash
for i in {1..5}
do
	echo $1
done
```

```
#!/bin/bash
INDEX=1
while [ $INDEX -lt 6 ]
do
	echo “Creating project-${INDEX}”
	mkdir /usr/local/project-${INDEX}
	((INDEX++))
done
```

Read file line-by-line

```
LINE_NUM=1
while read LINE
do
	echo “${LINE_NUM}: ${LINE}”
	((LINE_NUM++))
done < /etc/fstab
```

#### Case statements

`*` represents default case
Double semicolon `;;` separates each block

```
case “$1” in
	start|START)
		/usr/sbin/sshd
		;;
	stop|STOP)
		kill $(cat /var/run/sshd.pid)
		;;
	*)
		echo “Usage: $0 start|stop” ; exit 1
		;;
esac
```

```
case “$ANSWER” in
	[yY]|[yY[eE][sS])
		echo “You answered yes”
		;;
	[nN]|[nN][oO])
		echo “You answered no”
		;;
	*)
		echo “Invalid"
		;;
esac
```

### Debugging

`-x` Prints commands and arguments as they execute values of variables displayed, expansion of `*` displayed
`#!/bin/bash -x`

`-e` Exit on error

`-v` Prints shell inputs as read in, before substitutions & expansions are applied does not provide variable and wildcard expansion

Can be combined

```

#!/bin/bash -ex

# error and debug

```

`$BASH_SOURCE` is script name  
`$LINENO` is the line number
