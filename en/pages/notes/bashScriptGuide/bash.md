# Greg's Wiki Bash Guide

source: mywiki.wooledge.org/BashGuide/

notes

## guide.bash.academy

### 01

Bash = Bourne again shell

a binary is an exec prog that is exec dir by the system's kernel

linux console terminal acts as a medium for input and output operations for system. Terminal provides text interface for running a shell i.e. bash. Command line interface.

note: bash prog is only on eof many that can run in a terminal. Terminal program is what's making text appear, providing the text interface. It is only running a bash shell.


## Commands and arguments

### Review so far
* Alias
* Function - a name that is mapped to a set of commands. i.e. "cd", "echo"
* Builtin - certain commands built into bash. handled dir by bash executable
* keyboard - like builtins but have special parsing rules. i.e. [ is builtin while [[ is a keyword. Both used for testing stuff. [[ thus able to offer an extended test. i.e. `<` operator in [ and [[ parsed and treated differently.
* Executable - a program that can be exec by ref to its file path. or name if its loc is in PATH var


### Additionally

`type` command can be used to get descrip of command type
```bash
> type rm
rm is hashed (/bin/rm)
> type cd
cd is a shell builtin
```
### Scripts

sequence of commands. Bash reads and processes **in order**. Only moves onto next command when current one has **ended**. (exception being if command has been run asynchronously, in background.)

shebang: `#!/bin/bash`

shebang = *interpreter directive*. Specifies that /bin/bash is to be used as the interpretor when the file is used as the executable in a command. When the kernel execs a non-binary file, it reads first line if begins with `#!` kernel uses the line to determine the interpretor to relay the file to. i.e. bash or python.

could also use `#!/usr/bin/env bash`

`env` searches `"$PATH"` for the exec named by its first argument, in this case "bash."

**Note** `/bin/sh` **is not bash!** Bash however is a "sh-compatible" shell (can run most sh scripts and much of the same syntax), however, opposite is not true.

Also don't give scripts a .sh extension. no purpose. misleading.

`$ bash myscript` to execute a script. In this ex, `#!` is just a comment, bash does nothing with it, since we already passed the file to bash interpretor manually. if run by `./myscript` shebang is used.

Generally people like to keep scripts in a personally def dir; a few like to keep in an existent, executable PATH. To use personal dir,

```bash
$ mkdir -p "$HOME/bin"
$ echo 'PATH="$HOME/bin:$PATH"' >> "$HOME/.bashrc"
$ source "$HOME/.bashrc"
```

traditional for dir that contain commands to be named `bin`

### Additionally

* if system has mult ver of bash installed, specify interp by abs path may be disearble t oensure right version. `type -a bash` to print paths to all bash execs in PATH.
* interp can be followed by one word of interp's options. i.e., `#!/usr/bin/bash -xv` will turn on verbose debugging.
* After defining interp in header, it is recommended to summarize the scripts purpose.

```bash
#!/usr/bin/env bash
# script name - a short explanation of purpose
#
# scriptname [option] [argument] ...
```



## Special Chars

when eval by Bash have a *non-literal* meaning.

**To do later, in table mode vim**

#### Extra: Command Substition (extra=not in bashguide on this section)
is used to insert output of one command into a second commmand. e.g. with an assignment.

```bash
> today=$(date)
> echo $today
Mon Jul 26 ...
```

```bash
$ echo "Today is $(date + %A), it's $(date + %H:%M)"
Today is Monday, it's 13:21
```

see wordsplitting for more info on parsing of substition


**Examples**
```bash
> echo "I am $LOGNAME"
I am yuhuan qiu.
> echo 'I am $LOGNAME'
I am $LOGNAME
>...
```


## Parameters

Parameters are a sort of named space in memory you can use to retrieve or store info.

Come in two flavors: *vars* and *special params*. special params are read only and pre-est by BASH and used to communicate some type of internal status. Vars are parms that you can create and update yourself. Var names are bound by a rule:

*name*: only of letters,digits,udnerscores, and begin with letter or underscore. i.e. identifier

cant use spaces with `=` in assignments

To access data in var we use `parameter expansion`. which is substitution of a param by its value. Like a dereference operator * in *var in C++.

`>echo "foo is $foo"` becomes `> echo "foo is bar"`

```bash
> song="My song.mp3"
> rm $song
---> becomes 'rm My Song.mp3`
My: no such file or dir
song.mp3: no such file or dir
```

Bash replace `$song` with contents then performed word splitting. and THEN executed.

**As with all substition, command substition results will undergo word splitting**

To fix this. **put double quotes around every parameter expansion**. As double quotes protect the text inside them form being split into multiple words, so avoids word splitting problems after substition.



### Special params and vars

Vars are just one kind of param, params that are denoted by a name. Params that aren't vars are called special params.

ex
```bash
> echo "My shell is $0, and has herese options set: $-"
My shell is -bash, and has these options set: himB
```

** Insert table of special params **

The only good PE(param expansion) is a quoted PE.

### Var Types

although bash is not a typed language, does have a few diff types of vars.

* Array: `declare -a *variable*` the variable is a narray of strings
* Associative array: `delcare -A` var is n associative array of strings (bash 4.0 or higher) ?
* integer `declare -i`. Assigining values to this var auto triggers arithmetic evaluation
* Read Only `declare -r`. var can no longer be modified or unset
* Export `declare -x` the var is marked for export which means it will be inherited by any child process. Vars inherited this way are called *env vars*

in practice, `delcare -i` rare. Behav is strange for anyone to misses `decl` statement. Most prefer to use explicit arithmetic commands (with ((..)) or let) when they want to perform arithmetic.

Also rare for `declare -a` too. Sufficient to `array=(..)` Also rare for `declare -a` too. Sufficient to `array=(..)`

### Parameter Expansion

mywiki.wooledge.org/BashGuide/Parameters/ 5/6 way down

```bash
for file in *.JPG *.jpeg
do mv -- "$file" "${file%.*}.jpg"
done
```

Renaming al ljpeg files to have a normal .jpg extension. `${file%.*}` cuts off everything from the end starting with the last period (.). Then a new extension is appended to expansion result.

| syntax         | description                                                                    |
|----------------|--------------------------------------------------------------------------------|
| `${param:-word}` | *Use Default Val* if param is unset or null, word is substituted.              |
| ''=word        | *assign default val* if param is unest or null, word is asigned to param       |
| ''+word        | if param is null or unest, nothing is subsittued, otherwise word is substitued |
dd




## Patterns

Bash has 3 kinds of pattern matching.

On command line, most likely use *globs*.

Second is *extended globs*, which allows more complicated expressions.

Since 3.0, bash also supports *regular expression* patterns. Useful mainly in scripts to test user
input or parse data. (Can't use regex to select filenames; only globs and extended globs can do
that)

### Glob patterns

* `*` matches any string, including null str
* `?` maches any single char
* `[...]` matches any one of the enclosed chars

globs are implicitly anchored at both ends. So a glob must match a whole string. i.e. a glob of a*
will not match the string cat.

when used to match *filenames* the `*` and `?` chars cannot match a slash(/) char. i.e. the glob
`*/bin` might match foo/bin but will not match /usr/local/bin.

note: bash does filename expansions *after* word splitting so filenames generated by a glob will not
be split and are always handled directly.

```bash
$ touch "a b.txt"
$ ls
a b.txt
$ for file in `ls`; do rm "$file"; done
rm: cannot remove a; no such file...
rm: cannot remove b.txt; no such file
$ for file in *; do rm "$file"; done
$ ls
```

for command splits strings into wrds. so 'a b.txt' gets split. Bad. globe however expands in proper
form. It expands to string "a b.txt" which for takes as a single arg. no word slitting after for
globs.

**Always use globs instead of `ls` to enumerate files. globs expand safely and minimize risk for
bugs**

### Extended Globs

turned off by default. can be turned on with `shopt`.

`shopt -s extglob`

* ?(list) matches zero or one occur of given pattern
* *(list) matches zero or more occurences
* +(list) matches one or more occurance of given
* @(list) matches one of the given patterns
* !(list) matches anything but the given

list in parens is a list of globs/extended globs sperated by `|` char.

```bash
>ls
names.txt tokyo.jpg cali.bmp
>echo !(*jpg|*bmp)
names.txt
```

### Regular Expressions

similar to glob patterns, but only used for pattern matchingn, not for filename matching.

Bash uses the *extended regular expression (ERE) dialect*. links at source for more reading.

note: don't wrap regex patterns in quotes.

### Brace Expansion

technically not in cat of patterns, but is similar. Globs only expand to actual filenames, but brace
expansion will expand to any possible permutation of their contents.

```bash
> echo th{e,a}n
then than
> echo {/home/*,/root}/.*profile
/home/axxo/.bash_profile /home/lhunath/.profile /root/.bash_profile /root/.profile
> echo {1..9}
1 2 3 4 5 6 7 8 9
```

brace expansion happens before filename expansion. when using both brance expansion and globs, the
brace expansion goes first. Then the globs are expanded.

brace expansion can only be used to generate list of words. cannot be used for pattern matching.



### Tests and Conditionals

command return 0 if success. 1..255 exit code for error. `?` special param holds last exit code of
last foreground process that terminated.

**Good practice**: make sur ethat scripts always retur na nonzero exit code if something unexpected
happened in there execution. you can do thsi with the exit builtin:

`rm file || { echo 'Could not delete file!' >&2; exit 1; }`

`&` with `>` means what comes next is a file descriptor not a file.

`2` stderr file descriptor number. so redirect echo output to stderr stream.


### Control operators && and ||

in general, not a good idea to string together mult diff control ops in one command. useful in
simple but not in complex.


### Grouping Statements

Bash reads conditional ops from left to write. if `a && b && c || d`, even if a fails, bash skips
second, then skips third, finall sees `||` operator, (at this stage exit code is "failure") so bash
executes `d`. Problem.

**Solution**

```bash
grep -q goodword "$file" && ! grep -q badword "$file" && { rm "$file" || echo "couldn't delete:
"$file" > &2; }
```

rm and echo are one statement now.

another common use of grouping is error handling.

```bash
# Check if we can go into appdir.  If not, output an error and exit the script.
cd "$appdir" || { echo "Please create the appdir and try again" >&2; exit 1; }
```


# Conditional Blocks

```bash
$ if true
> then echo "It was true."
> else echo "it was flase."
> fi
it was true.
```

[[ pattern matching:
```bash
$ [[ $filename = *.png ]] && echo "$filename looks like a PNG file"
```

if else:
```bash
$ name=lhunath
$ if [[ $name = "George" ]]
> then echo "Bonjour, $name"
> elif [[ $name = "Hans" ]]
> then echo "Goeie dag, $name"
> elif [[ $name = "Jack" ]]
> then echo "Good day, $name"
> else
> echo "You're not George, Hans or Jack.  Who the hell are you, $name?"
> fi
```

Note thatthe comparison ops, `=, !=, >, <` treat args as strings. To string operands
as numbers, need to use diff set of ops: `-eq, -ne, -lt, -gt, -le, -ge`.

for `[,[[`
```
# Tests supported by [ (also known as test):

-e FILE: True if file exists.

-f FILE: True if file is a regular file.

-d FILE: True if file is a directory.

-h FILE: True if file is a symbolic link.

-p PIPE: True if pipe exists.

-r FILE: True if file is readable by you.

-s FILE: True if file exists and is not empty.

-t FD : True if FD is opened on a terminal.

-w FILE: True if the file is writable by you.

-x FILE: True if the file is executable by you.

-O FILE: True if the file is effectively owned by you.

-G FILE: True if the file is effectively owned by your group.

FILE -nt FILE: True if the first file is newer than the second.

FILE -ot FILE: True if the first file is older than the second.

-z STRING: True if the string is empty (it's length is zero).

-n STRING: True if the string is not empty (it's length is not zero).

# String operators:

STRING = STRING: True if the first string is identical to the second.

STRING != STRING: True if the first string is not identical to the second.

STRING < STRING: True if the first string sorts before the second.

STRING > STRING: True if the first string sorts after the second.



EXPR -a EXPR: True if both expressions are true (logical AND).

EXPR -o EXPR: True if either expression is true (logical OR).

! EXPR: Inverts the result of the expression (logical NOT).
Numeric operators:

INT -eq INT: True if both integers are identical.

INT -ne INT: True if the integers are not identical.

INT -lt INT: True if the first integer is less than the second.

INT -gt INT: True if the first integer is greater than the second.

INT -le INT: True if the first integer is less than or equal to the second.

INT -ge INT: True if the first integer is greater than or equal to the second.


# Additional tests supported only by [[:

STRING = (or ==) PATTERN: Not string comparison like with [ (or test), but pattern matching is performed. True if the string matches the glob pattern.

STRING != PATTERN: Not string comparison like with [ (or test), but pattern matching is performed. True if the string does not match the glob pattern.

STRING =~ REGEX: True if the string matches the regex pattern.

( EXPR ): Parentheses can be used to change the evaluation precedence.

EXPR && EXPR: Much like the '-a' operator of test, but does not evaluate the second expression if the first already turns out to be false.

EXPR || EXPR: Much like the '-o' operator of test, but does not evaluate the second expression if the first already turns out to be true.
```

if making bash script, always use `[[`.




### Conditional Loops (while, until, and for)

`while` command: Repeat so long as command is executed successfully (exit code is 0).

`until` command: Repeat so long as command is executed unsuccessfully (exit code is not 0).

`for` variable in words: Repeat the loop for each word, setting variable to each word in turn.

`for` (( expression; expression; expression )): Starts by evaluating the first arithmetic expression; repeats the loop so long as the second arithmetic expression is successful; and at the end of each loop evaluates the third arithmetic expression.

```bash
$ while true
> do echo "Infinite loop"
> done
$ (( i=10 )); while (( i > 0 ))
> do echo "$i empty cans of beer."
> (( i-- ))
> done
$ for (( i=10; i > 0; i-- ))
> do echo "$i empty cans of beer."
> done
$ for i in {10..1}
> do echo "$i empty cans of beer."
> done
```

```bash
$ for file in *.mp3
> do rm "$file"
> done
```

### Choices (case and select)

sometimes bit redundent to do if elif elif elif...

use `case`. enumerates several possible *glob patterns* and checks contents of your param against
these.

```
case $LANG in
    en*) echo 'Hello!' ;;
    fr*) echo 'Salut!' ;;
    de*) echo 'Guten Tag!' ;;
    nl*) echo 'Hallo!' ;;
    it*) echo 'Ciao!' ;;
    es*) echo 'Hola!' ;;
    C|POSIX) echo 'hello world' ;;
    *)   echo 'I do not speak your language.' ;;
esac
```

Each choice in a `case` consists of a pattern, a right paren, a block of code, and two semi colons
to denote end of block. `case` stops matching as soon as one is successful.


`select`. convenient for generating menu of choices that user can choose from.
```bash
$ echo "Which of these does not belong in the group?"; \
> select choice in Apples Pears Crisps Lemons Kiwis; do
> if [[ $choice = Crisps ]]
> then echo "Correct!  Crisps are not fruit."; break; fi
> echo "Errr... no.  Try again."
> done
```

```bash
$ PS3="Which of these does not belong in the group (#)? "; \
> select choice in Apples Pears Crisps Lemons Kiwis; do
> if [[ $choice = Crisps ]]
> then echo "Correct!  Crisps are not fruit."; break; fi
> echo "Errr... no.  Try again."
> done
```

## Arrays

probably end here, refer to official ref for more.

http://mywiki.wooledge.org/BashGuide/Arrays


