---
layout: page
title: pyenv notes
math_support: mathjax
---


pyenv manages different python distributions

## 1. install

refer to official site: [official](https://github.com/yyuu/pyenv)

install via homebrew on osx or use [pyenv-installer](https://github.com/yyuu/pyenv-installer) on linux.

## 2. add to .bashrc

add the corresponding line into *.bash_profile*, making it proceed the \$PATH variable.

## 3. install different distributions

~~~ bash
pyenv install --list # show the possible distributions
pyenv install 2.7.11
pyenv install 3.5.1
pyenv install pypy5.1
~~~

## 4. choose corresponding env

different env affects the `python` / `pip` commands, in the order: 

~~~ bash
pyenv shell 2.7.11 # use the dist. for current shell session
pyenv local 2.7.11 # use the dist. for current dir and all sub dir
pyenv global 2.7.11 # use the dist. for all 
~~~

**internals**:

- shell: pyenv exports a shell variable
- local: pyenv write a *.python-version* file in the current dir
- global: pyenv writes a global version file *$PYENV_ROOT/version*
  - *~/.pyenv/version* for linux
  - */usr/local/var/pyenv/version* for osx
  
**to remove:**

~~~ bash
pyenv shell --unset 
pyenv local --unset
~~~

## 5. packages

after changed to every new dist., pip is new and packages will be installed into that dist., for example:   
*$PYENV_ROOT/versions/2.7.11/lib/python2.7/site-packages*

add to path search for local 2.7.11 packages (??)

## 6. virtualenv

[pyenv-virtualenv into](https://github.com/yyuu/pyenv-virtualenv)

use barely virtualenv is ok

while pyenv could help to activate or deactivate easily

~~~ bash
pyenv virtualenv 2.7.11 ./myenv # create
pyenv virtualenv ./env2 # create under the current dist.
pyenv activate env2
pyenv deactivate
pyenv uninstall ./myenv #remove
~~~







