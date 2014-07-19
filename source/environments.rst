Environments
============

Getting Started
---------------
Start the VM we'll be working with and connect to it

    vagrant up
    
    vagrant ssh

Next, we need to make sure packages are upto date so let's start by refreshing
the package list and then upgrading any out of date packages

    sudo apt-get update
    
    sudo apt-get upgrade


pyenv
-----

What is pyenv?
^^^^^^^^^^^^^^

It's a way to build and manage multiple python versions. For example, it will
let you run python 2.5, 2.7, 3.4, pypy, etc all happily on one system. It's
modeled after rbenv and works well with the rest of what we're going to talk
about today.

Setting up pyenv?
^^^^^^^^^^^^^^^^^

First, we need to install git:

    sudo apt-get install git

Next we need to clone down the pyenv repo:

    git clone git://github.com/yyuu/pyenv.git .pyenv

So that we have access to pyenv we need to hook it up to our shell. In order
for this to work properly, we have to tell bash, zsh, or whatever where to look
for it.

    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc

Now we'll tell the shell to add it to the execution PATH list

    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

Now that we can access the pyenv files, we're gonna call pyenv init

    echo 'eval "$(pyenv init -)"' >> ~/.bashrc

Okay let's restart the shell:

    eval $SHELL

so now we have access to pyenv

    pyenv

Now we need to install the build tools required to compile python
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm python-dev

Using pyenv?
^^^^^^^^^^^^

Let's see what python we are currently using.

    pyenv versions
    
    python --version

Let's see what we can install

    pyenv install -l

Let's install the new goodness

    pyenv install 3.4.1
    
    pyenv rehash

Okay check the versions again

    pyenv versions

Okay let's use python 3.4.1 for our user default python

    pyenv global 3.4.1
    
    pyenv versions

    python
    
    import asyncio

Crap back to normal work...

    pyenv install 2.7.8

How can I do per project versions?

    mkdir -p ~/dev/tests
    
    cd ~/dev/tests

    pyenv local 2.7.8
    
    ls -al
    
    cat .python-version

WAIT WAT!?!
yeah that's amazing

Extra Goodness
^^^^^^^^^^^^^^

Sets the root for our global version

    echo 'export PYVER_ROOT=`pyenv prefix`' >> ~/.bashrc

Set the executable path for our global version

    echo 'export PYVER_BIN="$PYVER_ROOT/bin"' >> ~/.bashrc

VirtualEnv
----------

Let's us seperate python packages into convenient environments that we can enable
and disable. This lets us do things like deal with dependancies and pin versions.

Install virtualenv
^^^^^^^^^^^^^^^^^^

    pip install virtualenv

Using virtualenv
^^^^^^^^^^^^^^^^

    virtualenv

WAIT WAT?!?

    pyenv which python
    pyenv which virtualenv
    pyenv rehash

Creating an environment
^^^^^^^^^^^^^^^^^^^^^^^

    virtualenv venv

Activating an environment
^^^^^^^^^^^^^^^^^^^^^^^^^

    source venv/bin/activate

notice the prompt change

    cat venv/bin/activate

Now we can install packages in this virtual environment that don't inteerfer
with our system python or any other python apps we're working on

Let's install another package

    pip install flask

Leaving the virtualenv
^^^^^^^^^^^^^^^^^^^^^^

    which python
    
    pip freeze
    
    deactivate

Notice the python and package listings

    which python
    
    pip freeze

So what is I don't wanna use the pyenv version of python I want a different one

    virtualenv --python=/opt/python-3.3/bin/python venv


VIRTUALENVWRAPPER
-----------------

Makes it easier to setup and use virtualenv in a consistent manner project to
project. It also provides some great hooks for us to tie into.

Install virtualenvwrapper
^^^^^^^^^^^^^^^^^^^^^^^^^

    pip install virtualenvwrapper

Tell virtualenvwrapper where to store virtualenvs

    echo 'export WORKON_HOME=$HOME/.virtualenv' >> ~/.bashrc

Tell virtualenvwrapper where to store projects

    echo 'export PROJECT_HOME=$HOME/dev' >> ~/.bashrc

Initialize virtualenvwrapper

    echo 'source $PYVER_BIN/virtualenvwrapper.sh' >> ~/.bashrc

reinit shell

    source ~/.bashrc

Using virtualenvwrapper
^^^^^^^^^^^^^^^^^^^^^^^

Listing available environments/projects
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    workon

Creating an environment
^^^^^^^^^^^^^^^^^^^^^^^

This creates and activates a new virtualenv but does not create a directory

    mkvirtualenv cookies

Deactivating doesn't change it's just

    deactivate

Removing an environment
^^^^^^^^^^^^^^^^^^^^^^^

    rmvirtualenv cookies

Creating a project
^^^^^^^^^^^^^^^^^^

This creates a new virtualenv and a project directory.

    mkproject cookies

Removing a project is a two step process
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    rm -rf $PROJECT_HOME/cookies
    
    rmvirtualenv cookies

Activating an environment or project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This will activate the environment and if a project switch to it's directory

    workon cookies

Hooks
^^^^^

let you add to the behavior of the virtualenvwrapper commands

    cd ~/.virtualenv
    
    ls

`An example <https://github.com/jasonamyers/dotfiles-linux/blob/master/virtualenv/postmkvirtualenv>`_


PIP Enhancements
----------------

Pip can be so much faster than it is, but it requires just a few things done
to it first

`Glyph's pip 2014 awesomeness <http://pip2014.com/>`_

tweet @glyph a HUGE THANK YOU ... RIGHT NOW from PYNASH!

The pain
^^^^^^^^

    pip install ipython[all]

Installing Packages
^^^^^^^^^^^^^^^^^^^

    pip install setuptools;
    
    pip install wheel
    
    pip wheel setuptools
    
    pip wheel virtualenv
    
    pip install virtualenv virtualenvwrapper

Setting up ENV
^^^^^^^^^^^^^^

    echo 'export STANDARD_CACHE_DIR="${XDG_CACHE_HOME:-${HOME}/.cache}/pip"' >> ~/.bashrc
    
    echo 'export WHEELHOUSE="${STANDARD_CACHE_DIR}/Wheelhouse"' >> ~/.bashrc
    
    echo 'export PIP_USE_WHEEL="yes"' >> ~/.bashrc
    
    echo 'export PIP_DOWNLOAD_CACHE="${STANDARD_CACHE_DIR}/Downloads"' >> ~/.bashrc
    
    echo 'export PIP_FIND_LINKS="file://${WHEELHOUSE}"' >> ~/.bashrc
    
    echo 'export PIP_WHEEL_DIR="${WHEELHOUSE}"' >> ~/.bashrc

Using it right
^^^^^^^^^^^^^^

    pip wheel ipython
    
    pip install ipython
