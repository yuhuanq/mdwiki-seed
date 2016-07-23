---
Title: Learn Vimscript

Subject: vimscript

Author: Yuhuan

Date: July 07, 2016

---

## Function arguments

### varargs

```vim
function Varg(..)
    echom a:0
    echom a:1
    echo a:000
endfunction

call Varg("a","b")
```

``...`` tells vim function can take any num args.

`a:0` displays number of extra args given.

`a:1` displays `a`. Displays first extra argument given.

When a function has varargs `a:000` will be set to a list containing all the extra arguments that
were passed. Note can't use `echom` with a list, hence `echo`.

Can use varargs together with regular arguments too.

```vim
function Varg(foo,...)
    echom a:foo
    echom a:0
    echom a:1
    echo a:000
endfunction
```

Remember that `a:` tells vim to use the argument scope.

Important: Cannot reassign argument variables.

For more information:
1. read `:help function-argument`
2. read `:help local-variables`

## Numbers

Different types of variables you can use.

vimscript has two types of num variables: Numebrs and Floats. A number is a 32 bit signed integer. A
float is a floating point number

### Number formats

Can specify numbers in hex notation by prefixing with `0x`. i.e. `echo 0xff`.

### Float formats

Can use exponential notation. i.e. `echo 5.43e+3`

--

vim will cast numbers to float through arithmatic or other operation with a float.

division is by int div.

`:help float`
`:help floating-point-precision`


## Strings

`echom "hello, " + "world"` displays `0` for osme reason!

Know that vim's `+` operator is *only* for Numbers. With string vim will try to coerce it to a
Number before performing the operation.

Must use concatenation operator.

`:echom "hello, " . "world"`

escape characters work as expected.

**Literal Strings** use to avoid excessive use of escape seqs.

`:echom '\n\\'` will display `\n\\`. Single quotes tells vim you wnat string exactly as is.

`:help expre-quote`
`:help i_CTRL-V`
`:help literal-string`

### String methods

`strlen("foo")`

`len("foo")`

Exists a difference. Will come back to.

`split("one two three")` splits a String into a list. default seperator is whitespace, can specify
with second arg.

`join(split("foo bar"), ";")` displays `foo;bar`.

`tolower(str)` and `toupper(str)`

`:help functions` to skim list of built in functions.



## Execute

`execute` command is used to eval string as if were a vimscript command.

`execute "echom 'hello, world'"` evals the string as a command and thus echoes.

`:help execute` `:help leftabove/rightbelow/split/vsplit`

## Normal

`:normal G` will move your cursol to the last line just like `G` in normal mode.

If remapped '`:normal _` will use remapped version. Thus, use `:normal! _` command to use default
functionality of that _ key.

In addition, `normal!` doesn't parse special chars so `:normal! /foo<cr>` doesn't work. Doesn't
recoginize the return statement. Will come back to later.

`:help normal`

`:execute "normal! gg/foo\<cr>dd"` will move to top, search for first occur of `foo` and delte the
line.

Combining `normal!` and `execute` fixes the special char parising error befor with just `normal!`.

**example**
```vim
:execute "normal! mqA;\<esc>`q"
```

`mq` stores current location in mark "q"

`` `q `` returns to the exact location of mark "q".


## 31. Basic Regular Expressions

` :execute "normal! gg/for .\\t in .\\+:\<cr>" `

`execute` takes string so double backslashes turn into single.

Vim has *four* different "modes" of parsing regex. Default requires `\` before `+` char to make it
mean 1 or more of preceding instead of a literal plus sign.

If you start all regex with `\v` tells vim to use its 'very magic' regex parsing mode, similar to
those of other languages.

`:execute "normal! gg" . '/\vfor .+ in .+:' . "\<cr>"`

`:help magic` `:help pattern-overview` `:help match`


## ...

tba
