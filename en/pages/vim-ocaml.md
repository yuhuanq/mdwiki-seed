<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>OCaml+Vim+OS X</title>
  </head>
  <body>
    <h1>OCaml+Vim+OS X</h1>
    <h4>by <a href="http://www.cs.kent.ac.uk/~sao/">Scott Owens</a></h4>
    <h2>Modern language, ancient editors</h2>

    There are two editors with good support for <a
      href="http://ocaml.org">OCaml</a>: Emacs and Vim. Some other editors have
    support for OCaml syntax highlighting and automatic indenting &ndash; of
    sometimes dubious accuracy &ndash; and you can get by with that. In fact,
    everyone used to have to get by with mediocre editor support, and it wasn't
    that terrible; its only in the past few years that Emacs and Vim have done
    better. In the future, other editors will probably have good support too,
    but not at the present.

    <p>Good editor here support comes from two tools, <a
      href="http://www.typerex.org/ocp-indent.html">ocp-indent</a> and <a
      href="https://github.com/the-lambda-church/merlin">Merlin</a>.
    Ocp-indent is a utility for indenting OCaml files (the ocp is from OCaml
    Pro, the company that made it), and Merlin supplies modern IDE features
      (auto-completion and the like) for OCaml. For more on using Merlin with
      Vim also see <a
      href="https://github.com/the-lambda-church/merlin/wiki/vim-from-scratch">this
      page</a>. Because most OCaml programmers use either Emacs or Vim, the
    OCaml community has naturally added ocp-indent and Merlin support to them
    first.

    <p>This page is about getting Vim set up on a Unix-style system (especially
    OS X), and some rudimentary use.

    <h2>Getting set up</h2>

    The general plan is to first install the OCaml compiler, <a
      href="https://opam.ocaml.org">OPAM package manager</a>, and the Vim
    editor on your system, then to get ocp-indent, Merlin, and other useful
    OCaml libraries installed, and lastly to install <a
    href="https://github.com/scrooloose/syntastic">Syntastic</a> for Vim and
  build a good Vim configuration.

    <p>The following directions are not the only way to do this, the linked web
    pages for the various tools explain in detail the various options for
    installation. Nor are they guaranteed to work: depending on the state of
    your computer &ndash; which packages, and package managers it has
    installed, etc. &ndash; they might fail. So regard them as a guide to one
    easy-ish way to get everything up and running, but not as a foolproof
    recipe to unthinkingly follow.

    <p>I assume that you know how to use the command line and Vi or Vim at a
    basic level. The WWW is full of Vim tutorials, including <a
    href="http://www.openvim.com">an online interactive tutorial</a>.


  <ol>
    <li>Use your system's package manager to install OCaml, OPAM, and Vim if
      they aren't already there. <a
        href="https://ocaml.org/docs/install.html">This page</a> details the
      various ways to install OCaml and OPAM on various systems.
      On OS X you should use the <a
        href="http://brew.sh">Homebrew</a> package manager, which can be
      installed by running the following in a terminal.

      <p><code>ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"</code>

      <p>Use Homebrew to install the software.

      <p><code>brew install ocaml</code><br>
      <code>brew install opam</code><br>
      <code>brew install macvim</code>

      <p>OS X comes with a terminal-only Vim, which is fine, but the MacVim
      version is better: it acts like a Mac application with mouse support, cut
      and paste, etc.

      <p>Make sure that you have a recent OCaml, 4.02 or higher. 4.02.3 is
      current as of the writing of this page.
    </li>

    <li>If you just installed OPAM, initialise it:

      <p><code>opam init</code>

      <p>OPAM puts everything in the <code>.opam</code> subdirectory of your
      home directory.

      <p>If you already have OPAM, you can update it with <code>opam
      update</code> and upgrade all of the packages you've already installed
      with <code>opam upgrade</code>. In either case, you might want to do this
      in the future.
    </li>

    <li>Use OPAM to install Merlin and ocp-indent (or otherwise follow the
      installation directions on their web pages).

      <p><code>opam install merlin</code><br>
      <code>opam install ocp-indent</code>
    </li>

    <li>Install useful OCaml packages <a
        href="https://github.com/whitequark/ppx_deriving">ppx_deriving</a> and
      <a href="https://github.com/ygrek/ocaml-extlib">extlib</a>. ppx_deriving
      supplies automatic generation of boilerplate functions for printing and
      comparing values of user-defined types. extlib is a more complete
      standard library than the rather minimal one that comes with the OCaml
      compiler.

      <p><code>opam install ppx_deriving</code><br>
      <code>opam install extlib</code>
    </li>

    <li>Setup <a href="https://github.com/tpope/vim-pathogen">Pathogen</a>.
      Pathogen makes it easy to add extension to Vim, you just put the
      extension into ~/.vim/bundles.

      <p><code> mkdir -p ~/.vim/autoload ~/.vim/bundle</code>

      <p>Put <a
        href="https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim">this
        file</a> into the ~/.vim/autoload directory.
    </li>

    <li>Install useful Vim utilities into the ~/.vim/bundle directory that we
      just set up. All of these are hosted on Github.

      <p><code>cd ~/.vim/bundle</code><br>
      <code>git clone https://github.com/def-lkb/ocp-indent-vim.git</code><br>
      <code>git clone https://github.com/scrooloose/syntastic.git</code><br>
      <code>git clone https://github.com/tpope/vim-sensible.git</code><br>

      <p><a
        href="https://github.com/def-lkb/ocp-indent-vim.git">ocp-indent-vim</a>
      specialises ocp-indent to work with Vim. It is recommended over the
      default setup from OPAM in this <a
        href="https://opam.ocaml.org/blog/turn-your-editor-into-an-ocaml-ide/">generally
        useful blog post</a>. <a
        href="https://github.com/scrooloose/syntastic.git">Syntastic</a> checks
      for syntax errors whenever you save your file. <a
        href="https://github.com/tpope/vim-sensible.git">Vim-sensible</a> gives
      a modern, basic configuration for Vim; it gets Vim into a state where the
      other extensions will work.
    </li>

    <li>Setup a good ~/.vimrc file. <a
        href="http://www.cs.kent.ac.uk/~sao/article_files/.vimrc">This is mine</a>. At a
      minimum it needs the following four lines:

      <p><code>execute pathogen#infect()<br>
        let g:opamshare = substitute(system('opam config var share'),'\n$','','''')<br>
        execute "set rtp+=" . g:opamshare .  "/merlin/vim"<br>
        let g:syntastic_ocaml_checkers = ['merlin']</code>
    </li>

    <li>Check that everything worked. Make a new file test.ml and edit it with
      Vim.

      <p><code>touch test.ml<br>
        mvim test.ml</code>

      <p>Type a simple OCaml program into it.
      <p><code>let x = 1</code>

      <p>Move the cursor over x and type <code>\t</code>. At the bottom of the
      screen the type <code>int</code> of x should be displayed.

      <p>In a program with a type error (<code>let x = 1 1</code>), a red arrow
      should appear in the left column when the file is saved.  <p>Lastly,
      check which indenter is running.

      <p><code>:set indentexpr</code>

      <p>The result should be
      <code>indentexpr=ocpindent#OcpIndentLine()</code>. Any answer not
      referring to OcpIndent indicates that the (poor) Vim default OCaml
      indenter is running. This shouldn't happen, but it did when I was setting
      everything up for myself. Although I don't know how to fix it properly,
      you can hack around it be removing the default indenter from Vim.  Use
      the <code>:scriptnames</code> command in Vim to check which scripts are
      running. Look for the ones named <code>SOME_PATH/indent/ocaml.vim</code>.
      There should be the good one in
      <code>~/.vim/bundle/ocp-indent-vim/indent/ocaml.vim</code>, and a bad one
      somewhere else inside of your Vim installation (in my case in
      <code>/usr/local/Cellar/macvim/7.4-76/MacVim.app/Contents/Resources/vim/runtime/indent/ocaml.vim</code>).
      Simply move the bad one out of the way.


    </li>
  </ol>

  <h3>Changelog</h3>
  <ul>
    <li>2015-8-29: Initial</li>
  </ul>

  <p>
  <a href="http://validator.w3.org/check?uri=referer">Validate HTML</a>
  </p>

  </body>
</html>

