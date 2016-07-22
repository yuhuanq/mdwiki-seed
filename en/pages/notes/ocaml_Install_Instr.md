# Installing OCaml and Vim setup

These instructions are aimed at readers of Real World OCaml, though
much of what you find here will be useful for anyone who wants to get
a good working OCaml installation.

At the highest level, here's what you need to do:

- Get [OPAM](http://opam.ocaml.org) installed
- Build OCaml 4.01 or later using OPAM
- With OPAM, install the key libraries that you need to work through
  the examples in the book
- Install [Utop](https://github.com/diml/utop), a modern interactive
  toplevel with command history, tab completion, and defaults that are
  tuned to work with the examples in Real World OCaml,
- Set up your editing environment.
- Optionally install [merlin], a tool for providing IDE-like
  functionality like auto-completion, as well as [ocp-indent], a
  source code indenter.  Both work with emacs and vim, and can be
  adapted to other editors.

Note that Windows is not currently supported by the examples in Real
World OCaml or by OPAM, although it is being worked on.  Until that's
ready, we recommend using a virtual machine running Debian Linux on
your local machine.

Let's get started.

# Getting OPAM

The easiest way to install OPAM is through your OS's package manager.
In most cases, installing OPAM will also trigger the installation of
OCaml.  The version installed isn't important, since we can use OPAM
itself to install other versions of the compiler.

Here are platform-specific instructions.


## Ubuntu Linux

Ubuntu has have a PPA (Personal Package Archive) that you can use for installing OPAM as a binary package.

```shell
$ sudo add-apt-repository ppa:avsm/ppa
$ sudo apt-get update
$ sudo apt-get install curl build-essential m4 ocaml opam
```

### Without using PPA

If you don't want to use the PPA, Ubuntu 14.04 and later come with somewhat recent versions of OCaml and OPAM. (As of 16 Oct 2015, Ubuntu has OCaml 4.01.0, released 12 Sep 2013.)

```shell
$ sudo apt-get install m4 ocaml-native-compilers camlp4-extra opam
```

# Configuring OPAM

The entire OPAM package database is held in the `.opam` directory in
your home directory, including compiler installations. On Linux and
Mac OS X, this will be the `~/.opam` directory, which means you don't
need administrative privileges to configure it.  If you run into
problems configuring OPAM, just delete the whole `~/.opam` directory
and start over.

Let's begin by initialising the OPAM package database.  This will ask
you a few interactive questions at the end.  It's safe to answer yes
to these unless you want to manually control the configuration steps
yourself as an advanced user.

```shell
$ opam init
<...>
=-=-=-= Configuring OPAM =-=-=-=
Do you want to update your configuration to use OPAM ? [Y/n] y
[1/4] Do you want to update your shell configuration file ? [default: ~/.profile] y
[2/4] Do you want to update your ~/.ocamlinit ? [Y/n] y
[3/4] Do you want to install the auto-complete scripts ? [Y/n] y
[4/4] Do you want to install the `opam-switch-eval` script ? [Y/n] y
User configuration:
  ~/.ocamlinit is already up-to-date.
  ~/.profile is already up-to-date.
Gloabal configuration:
  Updating <root>/opam-init/init.sh
    auto-completion : [true]
    opam-switch-eval: [true]
  Updating <root>/opam-init/init.zsh
    auto-completion : [true]
    opam-switch-eval: [true]
  Updating <root>/opam-init/init.csh
    auto-completion : [true]
    opam-switch-eval: [true]
```

You only need to run this command once, and it will create the
`~/.opam` directory and sync with the latest package list from the
online OPAM database.

When the `init` command finishes, you'll see some instructions about
environment variables.  OPAM never installs files into your system
directories (which would require administrator privileges).  Instead,
it puts them into your home directory by default, and can output a set
of shell commands which configures your shell with the right `PATH`
variables so that packages will just work.

If you choose not to follow the OPAM instructions to add itself to
your shell profile, you can still configure it on-the-fly in your
current shell with just one command.

```shell
$ eval `opam config env`
```

This evaluates the results of running `opam config env` in your
current shell and sets the variables so that subsequent commands will
use them.  This _only_ works with your current shell and it can only
be automated for future shells by adding the line to your login
scripts.  On Mac OS X you should typically use `~/.profile`, and
`~/.bashrc` on most Linux setups.

OPAM isn't unusual in this approach; the SSH `ssh-agent` also works
similarly, so if you're having any problems just hunt around in your
configuration scripts to see how that's being invoked.

If you answered `yes` to the auto-complete scripts question during
`opam init`, this should have all been set up for you.  You can verify
this worked by seeing if the `OCAML_TOPLEVEL_PATH` environment
variable is set.  You can do this in `bash` as follows.

```shell
$ printenv OCAML_TOPLEVEL_PATH
/Users/myusername/.opam/4.01.0/lib/toplevel
```

The next step is to make sure we have the right compiler version
installed.

```shell
$ opam switch
system  C system                      System compiler (4.00.1)
--     -- 3.11.2                      Official 3.11.2 release
--     -- 3.12.1                      Official 3.12.1 release
--     -- 4.00.0                      Official 4.00.0 release
--     -- 4.00.1                      Official 4.00.1 release
--     -- 4.01.0                      Official 4.01.0 release
```

If, as in this case, your system compiler is older than `4.01.0`, you
should install a more up to date version of the compiler, as shown
below.

```shell
$ opam switch 4.01.0
$ eval `opam config env`
```

The `switch` itself will take around ten or fifteen minutes on a
modern machine, and will download and install the OCaml compiler
within the `~/.opam` directory).  You can install multiple compilers
at once, and switching between them once they're installed is fast.

The new compiler and all libraries associated with it will be
installed in `~/.opam/4.01.0`.

Finally, we're ready to install the libraries and tools we'll need for
the book.  The most important ones are `core`, which provides the
standard library that all the examples in the book are based on, and
`utop`, which is the interactive toplevel that you can use for working
through the examples.

```shell
$ opam install core utop
```

This will take about five or ten minutes to build, and will install
the requested packages along with a bunch of dependencies.  But there
are other libraries that will be useful as you go through the book
that you might as well install now.

```shell
$ opam install \
   async yojson core_extended core_bench \
   cohttp async_graphics cryptokit menhir
```

If some of the above don't work, don't worry too much.  Most of them
come up in only a few places in the book.  (There are some known
problems right now: in particular, `core_extended` doesn't install
cleanly on recent versions of OS X, due to an issue with the `re2`
library it depends on.  That is expected to be resolved soon.)

Note that if you've installed `utop` and can't launch it from the
shell, make sure you've run ```eval `opam config env` ``` in this
shell.  In general, you should just get this set up as part of
your shell init scripts, as described earlier.

# Setting up and using `utop`

When you first run `utop`, you'll find yourself at an interactive
prompt with a bar at the bottom of the screen.  The bottom bar
dynamically updates as you write text, and contains the possible names
of modules or variables that are valid at that point in the phrase you
are entering.  You can press the `<tab>` key to complete the phrase
with the first choice.

The `~/.ocamlinit` file in your home directory initializes `utop` with
common libraries and syntax extensions so you don't need to type them
in every time.  Given that you have Core installed, you should update
your `ocamlinit` to load Core every time you start `utop`, by adding
the following lines.

```ocaml
#use "topfind";;
#thread;;
#camlp4o;;
#require "core.top";;
#require "core.syntax";;
```

For `utop` only you could get away with just the last two lines, but
you need the whole thing if you want the traditional `ocaml` toplevel
to work too.

Note that `opam` will have already added some lines to the file.
Also, notice that `#` is used to mark a toplevel directive, not a
comment.

If you want to use Core as your primary standard library, then you
should open the `Core.Std` module by default, which you can do by
appending the following to the end of your `.ocamlinit`.

```ocaml
open Core.Std
```

When you run `utop` with these initialization rules, it should start
up with Core opened and ready to use.  If you don't open `Core.Std` by
default, then you must remember to open it before running any of the
interactive examples in the book.

# Editors

## Vim

Vim users can use the built-in style, and
[ocaml-annot](http://github.com/avsm/ocaml-annot) may also be useful.
For a more richly-featured setup, see the section on Merlin below.


## Cross-editor tools

There are a number of tools that are compatible with multiple editors
that can help you work more effectively with OCaml.  Two of the best
are `merlin` and `ocp-indent`, which we'll describe below.  Note that
these are very much optional.  You might want to get started with the
book before getting back to setting these up.

### Merlin

Merlin provides a number of advanced IDE-like features, including:

- context-sensitive auto-completion
- interactive type-querying (i.e., you can ask it, "what is the type
  of this expression")
- error reporting -- it will highlight parts of your code that don't
  compile as you go.

You can install merlin with OPAM:

```shell
$ opam install merlin
```

Then you will need to add configuration for your editor. The following
examples assume Merlin has been installed through OPAM as above.

#### Vim

A summary follows -- for more complete information, see [Merlin's Vim
documentation][merlin-vim] and `:h merlin.txt` after installing.

```vim
" Your vimrc
filetype plugin indent on
syntax enable

" Vim needs to be built with Python scripting support, and must be
" able to find Merlin's executable on PATH.
if executable('ocamlmerlin') && has('python')
  let s:ocamlmerlin = substitute(system('opam config var share'), '\n$', '', '''') . "/ocamlmerlin"
  execute "set rtp+=".s:ocamlmerlin."/vim"
  execute "set rtp+=".s:ocamlmerlin."/vimbufsync"
endif
```

As a one-time step after installation, index the documentation in
Vim's help system:

```vim
:execute "helptags " . substitute(system('opam config var share'), '\n$', '', '''') . "/merlin/vim/doc"
```

Some highlights of the functionality this enables:

- Autocompletion with Vim's omnicomplete system: `<C-x><C-o>`
- Quick type checking with keybindings -- the defaults use
  `LocalLeader`, which is normally backslash (`\`) by default:
    - `\t`: type of expression under the cursor (also works with a
      visual selection).
    - `\n` / `\p`: expand/contract the expression(s) to type check.
- `:Locate [identifier]`: go to the definition of given identifier or
  the one under the cursor.
- `:Reload`: if Merlin gets confused following build changes like new
  packages.
- `:GotoDotMerlin`: edit the `.merlin` project configuration (see
  below).

The plugin can integrate with some other popular Vim plugins such as
Syntastic and neocomplcache -- see `:help merlin.txt` for pointers.

#### Tell Merlin about your project layout

Regardless of which editor you use, you need to tell Merlin a bit
about the build environment you're working with in order for it to
work properly.  This is done by creating a `.merlin` file.  If you're
building applications based on Core and Async using the `corebuild`
script (which is built on top of `ocamlbuild`), then the following is
a good starting point.

```
S .
B _build
PKG core
PKG async
PKG core_extended
EXT nonrec
```

See [the Merlin README](https://github.com/the-lambda-church/merlin/blob/master/README.md#merlin-project) for complete details of the file format.

### ocp-indent

`ocp-indent` is a source-code indenter which includes a command-line
tool, library and integration with emacs and vi.  It does a better job
than most editor modes, including tuareg, and has the advantage that
it can be used as a command-line tool and be integrated into multiple
editors, so you get consistent indentation across tools.

You can install it with `opam`:

```shell
$ opam install ocp-indent
```

[Read this](http://https://www.typerex.org/ocp-indent.html)

#### Vim

After installing through OPAM, simply add this `autocmd` to your Vim
configuration:

```vim
" Your vimrc
autocmd FileType ocaml source substitute(system('opam config var share'), '\n$', '', '''') . "/typerex/ocp-indent/ocp-indent.vim"
```


[merlin]: https://github.com/the-lambda-church/merlin
[merlin-emacs]: https://github.com/the-lambda-church/merlin/wiki/emacs-from-scratch
[merlin-vim]: https://github.com/the-lambda-church/merlin/wiki/vim-from-scratch
[ocp-indent]: https://github.com/OCamlPro/ocp-indent


### Extra

check for compatability between ultrasnips and merlin
