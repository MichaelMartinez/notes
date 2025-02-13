---
dg-show-inline-title: false
dg-publish: true
dg-home: false
dg-show-toc: false
dg-created: 11/25/2015
dg-updated: 
dg-show-tags: false
dg-link-preview: false
---

Title: Anaconda Primer
Date: 2015-11-25 15:56
Author: Michael
Category: Python
Tags: Development
Slug: anaconda-primer 
Status: published

This was taken from [Dave Behnke](http://davebehnke.com/using-python-anaconda-distribution.html) which is
no longer active. I used the way back machine to find it and reproduce
it.

### Introduction  
As a followup to my article about pyenv, I decided it was worth also
taking a look at Anaconda/Miniconda an alternate install for Python 2/3.
It is available for Windows, OSX, and Linux. I will walk you through
obtaining and installing using miniconda3 (a minimal install). I will
also introduce you to the conda tool for creating and using environments
and installing packages.

From the Continuum website it is described as, "Completely free
enterprise-ready Python distribution for large-scale data processing,
predictive analytics, and scientific computing"

While it's roots are designing a tool for science, math, engineering,
and data analysis, thanks to the miniconda installer you can customize
your install starting with a minimal root environment.

The key selling point, for myself anyways, is that you can install
Python in your home directory (or users directory on OSX and Windows)
and not touch your system installed Python which may be needed by your
OS for various things. You can also install on older production systems
that may be using Enterprise Linux and a very ancient version of Python.

Before you continue, please take a look at
https://store.continuum.io/cshop/anaconda/ for a more formal look at
Anaconda. We're going to get down to the nuts and bolts next!

Which one to use ?  
We have two ways to install.

Anaconda: [here](http://continuum.io/downloads)

Miniconda: [here](http://conda.pydata.org/miniconda.html)

This is mostly a matter of choice, if you like having a complete Python
environment, and don't mind a larger download, things you might not use,
or new to Python, pick the Anaconda installer. It includes 181+ common
packages preinstalled.

If you want a bootstrap or minimal install, choose miniconda. Miniconda
basically just installs the standard library and interpreter.

Next you will need to determine if you want to use Python 2 or 3 by
default. I personally choose Python 3 because I'm trying to discipline
myself to use Python 3 whenever possible. This article will demonstrate
an install with Python 3. Don't worry though you can have both 2 and 3
installed at the same time as we will show later on with creating
environments. For now, pick your starting point, download ONE of the
installers (Anaconda based on 2 or 3, or Miniconda based on 2 or 3)

### Installing  
The installation instructions are here:
http://docs.continuum.io/anaconda/install.html

Windows and OSX Installs are easy using the GUI based installers, so I
will focus on the Linux install in my example.

Locate your download copy of the installer, open up a terminal and run
the install script.
    :::bash
    bash /Downloads/Miniconda3-3.0.5-Linux-x86_64.sh
     
You should be greeted with the welcome message, asked to read and
approve the license agreement, asked where to install, and prepend your
path in your .bashrc
    
    :::bash
    Welcome to Miniconda3 3.0.5 (by Continuum Analytics, Inc.)

    Do you approve the license terms? [yes|no]  
    [no] >>> yes

    Miniconda3 will now be installed into this location:  
    /home/dbehnke/miniconda3

    - Press ENTER to confirm the location  
    - Press CTRL-C to abort the installation  
    - Or specify an different location below

    [/home/dbehnke/miniconda3] >>>

    Python 3.3.4 :: Continuum Analytics, Inc.  
    creating default environment...  
    installation finished.  
    Do you wish the installer to prepend the Miniconda3 install location  
    to PATH in your /home/dbehnke/.bashrc ? [yes|no]  
    [no] >>> yes  
    Restart your Terminal

### Updating conda  
Now that you have installed miniconda, you should open up a terminal
(OSX, Linux) or Command prompt (Windows). The remainder of this article
will show a bash prompt, but the commands are the same in Windows.

conda is a tool for managing environments and packages. If you are
familiar with virtualenv, environments are similar. With Anaconda we
usually don't use virtualenv or pip (they can be installed if you really
want to) for installing packages and creating enviornments. Instead
conda takes care of most of the work. We will start by updating conda
since it's the center of importance for using Anaconda.

The following command will determine if conda is installed and available
in your PATH:

    :::bash
    $ conda -V  
    conda 3.0.5  
Now lets update conda

    :::bash
    $ conda update conda

    Package plan for installation in environment /home/dbehnke/miniconda3:

    The following packages will be downloaded:

    package | build  
    ---------------------------|-----------------  
    conda-3.0.6 | py33_0 106 KB

    The following packages will be UN-linked:

    package | build  
    ---------------------------|-----------------  
    conda-3.0.5 | py33_0

    The following packages will be linked:

    package | build  
    ---------------------------|-----------------  
    conda-3.0.6 | py33_0 hard-link

    Proceed ([y]/n)? y  

### Environments  
By default you install in the root environment. Environments allow you
to sandbox or seperate your installed packages. You can even have
different python versions in each environment. They live in the envs
directory in your installation directory. Conda will also try to create
hard-links whenever possible to save disk space.

List installed environments (e.g. here I have 2 environments created
already from a previous install. The * indicates my current environment
is root)

    :::bash
    $ conda info -e  
    # conda environments:  
    #  
    oracle /home/dbehnke/miniconda3/envs/oracle  
    root * /home/dbehnke/miniconda3 
     
###Creating  
Here is the breakdown of creating a new environment.

For the full documentation, access the help.

    :::bash
    conda create -h  
    
We will look at a subset of the available options (taken from the -h
command)

    :::bash
    Description

    Create a new conda environment from a list of specified packages. To use
    the created environment, use 'source activate envname' look in that
    directory first. This command requires either the -n NAME or -p PREFIX
    option.

    conda create -n NAME [package_spec [package_spec] ]

    positional arguments::

    package_spec package versions to install into conda environment

    optional arguments:

    -n NAME, --name NAME name of environment  
    -p PATH, --prefix PATH full path to environment prefix  
I personally like to only use the -n so that it lives with my
installation in my home directory. However, if you installed as root or
somewhere your user account doesn't have access to you may want to use
-p $HOME/somedirectory instead of -n. This article will only use -n for
examples.


e.g. Create an environment with Default python (in this case Python 3).
Here I create an environment named "oracle" in the env directory of my
installation with just the minimal python.

    :::bash
    $ conda create -n oracle python

    Package plan for installation in environment
    /home/dbehnke/miniconda3/envs/oracle:

    The following packages will be linked:

    package | build  
    ---------------------------|-----------------  
    openssl-1.0.1c | 0 hard-link  
    python-3.3.4 | 0 hard-link  
    readline-6.2 | 2 hard-link  
    sqlite-3.7.13 | 0 hard-link  
    system-5.8 | 1 hard-link  
    tk-8.5.13 | 0 hard-link  
    zlib-1.2.7 | 0 hard-link

    Proceed ([y]/n)? y

    Linking packages ...  
    [ COMPLETE ]
    |######################################################################|
    100%  
    #  
    # To activate this environment, use:  
    # $ source activate oracle  
    #  
    # To deactivate this environment, use:  
    # $ source deactivate  
    #  
### Activating
    :::bash
    $ source activate oracle  
    prepending /home/dbehnke/miniconda3/envs/oracle/bin to PATH  
    Verifying environment is active

Notice the (oracle) in front of the bash prompt. This is an indicator
that the "oracle" environment is activated.

    :::bash
    (oracle) $ conda info -e

    # conda environments:  
    #  
    oracle * /home/dbehnke/miniconda3/envs/oracle  
    root /home/dbehnke/miniconda3

    (oracle) $ which python

    /home/dbehnke/miniconda3/envs/oracle/bin/python

    (oracle) $ python

    Python 3.3.4 |Continuum Analytics, Inc.| (default, Feb 10 2014,
    17:53:28)  
    [GCC 4.1.2 20080704 (Red Hat 4.1.2-54)] on linux  
    Type "help", "copyright", "credits" or "license" for more information.  
    >>>  
### Deactivating  
   
    :::bash
    (oracle) $ source deactivate

    discarding /home/dbehnke/miniconda3/envs/oracle/bin from PATH  

You'll notice the (oracle) will be removed as well from the prompt.

### Deleting  
You can use the conda remove -n nameofenvironment --all or delete the
directory the environment is in.

`conda remove -n oracle --all` 
or, simply

`rm -r -f /home/dbehnke/miniconda3/envs/oracle` 
Using a Different Version of Python  
To list available versions of Python to install you can use "conda
search". Here is an example.

    :::bash
    conda search "^python$"

    python 2.6.8 1 defaults  
    2.6.8 2 defaults  
    2.6.9 1 defaults  
    2.7.3 7 defaults  
    2.7.4 1 defaults  
    2.7.5 3 defaults  
    2.7.6 0 defaults  
    . 2.7.6 1 defaults  
    3.3.0 4 defaults  
    3.3.1 0 defaults  
    3.3.2 0 defaults  
    . 3.3.3 1 defaults  
    * 3.3.4 0 defaults  
The installed root version deterimines the default version of python
(indicated by the * in the conda search example above) used when
creating environments. To specify a different version of python, use the
environment variable PYTHON=x. (where x is version found in conda
search)

e.g. installing Python 2 in an environment called oracle. Here I simply
use 2 instead of specifying the complete version number which will take
the latest version of python 2.

    :::bash
    $ conda create -n oracle python=2

    Package plan for installation in environment
    /home/dbehnke/miniconda3/envs/oracle:

    The following packages will be downloaded:

    package | build  
    ---------------------------|-----------------  
    python-2.7.6 | 1 11.2 MB

    The following packages will be linked:

    package | build  
    ---------------------------|-----------------  
    openssl-1.0.1c | 0 hard-link  
    python-2.7.6 | 1 hard-link  
    readline-6.2 | 2 hard-link  
    sqlite-3.7.13 | 0 hard-link  
    system-5.8 | 1 hard-link  
    tk-8.5.13 | 0 hard-link  
    zlib-1.2.7 | 0 hard-link

    Proceed ([y]/n)?  
###Cloning an Environment  
If you want to make a snapshot of an already existing environment, you
can use the --clone keyword

    :::bash
    conda create -n nameofnew --clone nameofsource  
    Installing Packages  
    conda install -n nameofenvironment [package] [package[=x.x.x]]
    [package]  
The -n is names your environment, if you omit the -n then the install
will be installed in the root environment. It can't be stressed enough,
it is very important to use the -n flag when installing packages in a
environment other than the root. Get yourself in the habit of using the
-n parameter even if you are using root. From experience, failing to do
so you may install the module in root when you meant to install it in
another environment.

[package] is either simply the package name, for example flask, or
sqlalchemy. This will install the latest version of the package and any
dependencies it may need.

[package=x]

This will the version matching x. You can also use x.y, x.y.x, etc. To
match the version of the package. For example, as we saw above when we
installed Python 2, if we wanted to install the latest version of python
2, we would use python=2. For python 2.6.9, we would use python=2.6.9.
The same goes for other packages, flask 0.10, you would use flask=0.10

It's important to note, omitting the python=x will default to the
miniconda version, i.e. miniconda3 installs python 3 and miniconda2
installs python 2 by default.

Example #1. Installing an Anaconda environment similar to the full
installer

    :::bash
    $ conda install -n oracle anaconda

    Package plan for installation in environment
    /home/dbehnke/miniconda3/envs/oracle:

    The following packages will be downloaded:

    package | build  
    ---------------------------|-----------------  
    anaconda-1.9.1 | np18py33_0 3 KB  
    argcomplete-0.6.7 | py33_0 26 KB  
    astropy-0.3.0 | np18py33_0 5.9 MB  
    beautiful-soup-4.3.1 | py33_0 114 KB  
    bitarray-0.8.1 | py33_0 89 KB

    ...  
Example #2. Installing a couple of packages

e.g. installing flask and sqlalchemy

    :::bash
    $ conda install -n oracle flask sqlalchemy

    Package plan for installation in environment
    /home/dbehnke/miniconda3/envs/oracle:

    The following packages will be downloaded:

    package | build  
    ---------------------------|-----------------  
    flask-0.10.1 | py27_1 129 KB  
    itsdangerous-0.23 | py27_0 16 KB  
    jinja2-2.7.2 | py27_0 308 KB  
    markupsafe-0.18 | py27_0 26 KB  
    sqlalchemy-0.9.3 | py27_0 1.1 MB  
    werkzeug-0.9.4 | py27_0 385 KB  
    ------------------------------------------------------------  
    Total: 2.0 MB

    The following packages will be linked:

    package | build  
    ---------------------------|-----------------  
    flask-0.10.1 | py27_1 hard-link  
    itsdangerous-0.23 | py27_0 hard-link  
    jinja2-2.7.2 | py27_0 hard-link  
    markupsafe-0.18 | py27_0 hard-link  
    sqlalchemy-0.9.3 | py27_0 hard-link  
    werkzeug-0.9.4 | py27_0 hard-link

    Proceed ([y]/n)?  
### Removing Packages  
This is accomplished by using the "conda remove" command and is similar
to the "conda install" command. You can also save time if you have many
packages to remove, by simply deleting the environment and recreating
it.

### Advanced  
What if a package doesn't exist through conda install ?

If the package you want is not available for conda, another option is to
search http://binstar.org (make sure the package you find is built for
the platform you are on), and then add that person's binstar channel
with conda config --add channels binstar_username.

You should try to use "conda install" whenever possible. However, as a
last resort, you can solve this problem by installing pip into your
environment and pull in packages you need. You could also create your
own conda packages, but that's beyond the scope of this article.

e.g. installing pip

    :::bash
    $ conda install -n oracle pip

    Package plan for installation in environment
    /home/dbehnke/miniconda3/envs/oracle:

    The following packages will be downloaded:

    package | build  
    ---------------------------|-----------------  
    pip-1.5.4 | py27_0 1.5 MB  
    setuptools-2.2 | py27_0 458 KB  
    ------------------------------------------------------------  
    Total: 2.0 MB

    The following packages will be linked:

    package | build  
    ---------------------------|-----------------  
    pip-1.5.4 | py27_0 hard-link  
    setuptools-2.2 | py27_0 hard-link

    Proceed ([y]/n)?  
Then just install as normal using pip install xxxx.

### Uninstalling  
To remove Anaconda/Miniconda, just remove all the prepended path
variables in your .bashrc or your System Environment, and then remove
the installation directory.

Windows Users, I believe you just use the uninstaller in Add/Remove
programs.

### Conclusions  
Anaconda gives you a nice alternative to the default Python installer
on Windows.

Default packages installed in anaconda package include most of the
common packages most python developers use.

Consistent installs across Windows, Linux, OSX - you can use the same
conda commands.

It should work on older Linux distributions as well since it appears to
be compiled on a Red Hat 5 system.

Further Reading:

http://conda.pydata.org/docs/

http://docs.continuum.io/anaconda/
