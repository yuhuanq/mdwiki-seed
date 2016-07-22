# Git Submodules

[Git Submodule Tutorial](http://https://git.wiki.kernel.org/index.php/GitSubmoduleTutorial#Removal)

TODO:
1. Clone .vim to ~/workplace/dot-files git repo and softlink back to ~.
2. Change plugins to git submodules

## Adding submodules

1. ``git submodule add repourl``

## To remove a submodule

1. Delete relevant section from the ``.gitmodules`` file in repo root
2. Stage the ``.gitmodules`` changes  ``git add .gitmodules``
3. Delete the relevant section from ``.git/config``
4. Run ``git rm --cached <path_to_submodule>`` (no trailing slash)
5. Run ``rm -rf .git/modules/path_to_submodule``
6. Commit ``git commit -m "Removed submodule <name>"``
7. Delete the now untracked submodule files. ``rm -rf <path_to_submodule>``


