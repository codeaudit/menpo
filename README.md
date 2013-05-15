DEPENDENCIES
============

Python
------

See `dependences.py` for a full list. All of these bar those noted in the
installation section below will be installed for you by `pip install`.
Note however that these packages will have many of their own non-python
dependencies (such as BLAS, or a Fortran compiler) and you will be expected
to have these installed. If you are using Ubuntu, the quickest way of ensuring
you have these base dependencies is to use the `build-dep` feature of
`apt-get`, which grabs all build dependencies for a given Debian package.

For example, running

    sudo apt-get build-dep python-numpy

will install all libraries that numpy requires to build. The libraries will
definitely be sufficient to build the version of numpy that ships with your
version of Ubuntu - and hopefully will be sufficient for the (newer) version
of numpy pip will download for you when installing pybug.

External
--------

The following table lists the dependencies that need to be installed prior to
installing pybug.

<table>
  <tr>
    <th>dependency</th><th>ubuntu package</th><th>description</th>
  </tr>
  <tr>
    <td>Open Asset Import Library</td>
    <td>libassimp-dev</td>
    <td>Import large number of 3D file formats</td>
  </tr>
  <tr>
    <td>Python bindings for VTK</td>
    <td>python-vtk</td>
    <td>Required for Mayavi functionality</td>
  </tr>
</table>

INSTALLATION
============

In addition to the above dependencies, pybug requires numpy and cython be
installed via pip prior to installation.

    pip install numpy cython

After that, just run

    pip install -e git+https://github.com/YOURGITHUBACCOUNT/pybug.git#egg=pybug

to install. We highly recommend you do this inside a virtualenv. Take a look
at virtualenvwrapper to make your life easier. Note that we recommend that you
grab a copy of the code from a personal fork of the project onto
YOURGITHUBACCOUNT - this will make issuing changes back to the main repository
much easier.

Full example
------------

This explains a full installation process on a fresh version of Ubuntu 13.04.
If this doesn't work for you, file a issue so the readme can be updated!

First we get our core dependencies

    sudo apt-get install libassimp-dev python-vtk git

Note you will need to set up git, but this is out of the scope of this README.
Do that now and come back here.

Then we get the build requirements for the python packages that pybug will
install for us

    sudo apt-get build-dep python-numpy python-scipy python-matplotlib cython

We will use virtualenv to keep everything nice and neat:

    sudo apt-get install virtualenvwrapper

Now close this terminal down and open a new one. You should see the initial
setup of `virtualenvwrapper` happen.

We make a new virtualenv for pybug

    mkvirtualenv pybug

and install numpy and cython

    pip install numpy cython

finally, we want to enable global python packages so that this env can see
the vtk python bindings we installed earlier

    toggleglobalsitepackages

(it should notify you that this ENABLED the global site packages).

Now we are good to install pybug. It should take care of the rest

    pip install -e git+https://github.com/YOURGITHUBACCOUNT/pybug.git#egg=pybug

The code is installed in the virtualenv we made. First, `cd` to it

    cdvirtualenv

Then go to the source folder

    cd src/pybug

Here you will find all the git repository for the project.

DEVELOPMENT
===========

The above installation properly installs pybug just like any other package.
Afterwards you just need to enable the pybug virutalenv

    workon pybug

and then you can open up an IPython session and `import pybug` just as you
would expect to be able to `import numpy`. Note that you can start a python
session from _anywhere_ - so long as the pybug env is activated, you will
be able to use pybug.

As we installed pybug with the `-e` flag, the install is _editable_ - which
means that any changes you make to the source folder will actually change how
pybug works. Let's say I wanted to print a silly message every time I import
pybug. I could go the source folder:

    cdvirtualenv && cd src/pybug

edit the `pybug/__init__.py` to print something

    echo 'print "Hello world from pybug!"' >> pybug/__init__.py


then, after starting (or restarting) a python session, you should see the
effect of the print statement immediately on import (try it out!) This means
the general approach is to, iteratively,

1. Edit/add files in `src/pybug` either using a text editor/IDE like PyCharm
2. Restart an IPython session/open the PyCharm IPython to load the changes
3. Try out your changes

the only extra complication is when developing Cython modules (those that
bridge C/C++ and Python). If you are doing development with Cython, or
do a git fetch which includes a change to a Cython file, you will need to
trigger the recompilation of the C/C++ code. To make this easy, there is a
Makefile in `src/pybug` - just go there and

    make

to check everything is up to date. As a first port of call, if something
doesn't work as expected, I would run `make` here. Maybe you switched to a
different git branch or something with different Cython files - if so, this
would fix any problems.

