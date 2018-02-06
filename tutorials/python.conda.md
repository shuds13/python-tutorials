# Anaconda

## What is Conda/Anaconda/Miniconda

Conda is a Package Manager and virtual environment manager for installing packages from Conda compatible distributions. Anaconda is a conda package distribrution that includes many python packages and extensions. Miniconda includes a much smaller set of core packages along with Conda. Miniconda still has access to the Anaconda repository on-line, and other repositories of conda packages including the community driven conda-forge, and these can easily be installed at the command line. Intel also provides high performance variants of many packages accessible through Conda. This includes numpy/scipy based upon MKL.

While conda packages are a binary distribrution allowing very fast installation, other forms of installation are supported inside Conda environments, including pip. Any source installation can also be performed inside the Conda virtual environment. Each package installs along with a list of dependent packages by default.

Conda environments are an alternative to other python virtual enviornment managers such as virtualenv, and does not mix well with these. Virtual environments are extremely useful in python for enabling reproducibility and maintaining muliple sets of packages/dependencies.

Ananconda is provided on many major computing platforms includig ECP systems, generally requiring loading of an environment module or equivalent.

Miniconda is ideal for personal use on standalone systems and for using with on-line CI tools such as Travis. It's fast due to binary installation and usually quite simple (unless you have dependency issues – see below). libEnsneble has an [example of using Conda with Travis](https://github.com/Libensemble/libensemble/blob/master/.travis.yml)

Download Anaconda [here](https://www.anaconda.com/download)
Download Miniconda [here](https://conda.io/miniconda.html)

Conda has a good getting started tutorial [here](https://conda.io/docs/user-guide/getting-started.html)

A good discussion on the thinking behind Conda and some common misconceptions can be found here: 

https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/



## Issues

A downside of using a binary distribution can be inflexibility, which may include incompatible dependencies (due to version locked packages). This may result in a dependency for one packages being upgraded/downgraded when intalling a later package. To some extent this can be managed by use of the `--no-deps` or `--no-update-deps` option on the Conda install command. Furthermore, the current version (Conda 4.4) does not support virtual dependencies and packages may, for example, come with a given MPI implementation as a dependency. 

Lets say that you wish to install openMPI via:

    conda install openmpi

and you then try installing another MPI based package - you may find that comes with mpich as a dependency.

<!-- *check -->

    conda install petsc
or
    conda install mpi4py

One answer to this is to try to install packages in one line as follows:

    conda install petsc mpi4py openmpi

If this does not work, then sometimes combining source distributions with conda can be used to create customised builds, along with `--no-deps` flags.

When installing on a system with an existing MPI, such as a cluster, then it is highly recommended that mpi4py is installed ontop of the system. It is recommended to use pip install in this case. E.g:

    env MPICC=$(which cc) pip install mpi4py


## Combining with other dpendencies

Python will also install packages according to sys.path which can be checked by:


    $ python
    Python 3.6.3 |Intel Corporation| (default, Oct 16 2017, 15:28:36) ....
    >>> import sys
    >>> sys.path

To ensure isolation from external packages on your system set the enviornment variable `export PYTHONNOUSERSITE=1` before activating the environment (simlar to `--no-site-packages` in virtualenv). This should prevent paths for external python paths being included in the sys.path inside the conda environemnt. Selected directories can also be added using the  PYTHONPATH env variable as usual.


<!-- Cross compilation issues *** -->


## Examples

The Intel Python libraries are popular for you in high performance environments.

Using Intel python libraries:

    conda config --add channels intel

You can now access python packages (Note: channels can also be added to a .condarc file)

To add the set of intel core packaes for latest version of python3 when creating a new environment myenv:

    conda create --name myenv intelpython3_core python=3

or to add Intel's full distribrution (takes a while):

    conda create --name myenv intelpython3_full python=3

Intel conda packages include numpy and SciPy based on MKL. 

