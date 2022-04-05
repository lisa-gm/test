Installation
============

.. _precompiled libSQUIC:

Precompiled libSQUIC
--------------------

The precompiled libSQUIC distributions are listed in the folder ``/precompiled/``. All distributions require OpenMP to be installed, which can be done via a standard package manager; e.g., for Ubuntu: ``sudo apt-get install libomp-dev``, or  Mac: ``brew install libomp``.  

.. note::

	The default location of libSQUIC for the different interface packages is the home directory; i.e., ``~/``. For further details, refer to the respective interface package.



.. _installation from source:

Installation from Source
------------------------

**Requirements**

For Mac:

- CMake (>3.9): For example using homebrew ``brew install cmake``

- :download:`Intel MKL libraries <https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit/download.html?operatingsystem=mac&distributions=webdownload&options=online>`

- OpenMP: For example using homebrew ``brew install libomp`` 

- C++/C Compilers (Clang): This is the default; nothing is required.

- Fortran Compiler (GNU): For example, using homebrew ``brew install gcc``

For Linux:

- CMake (>3.9)

- :download:`Intel MKL libraries: <https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit/download.html?operatingsystem=linux&distributions=webdownload&options=online>`

- OpenMP 

- C++/C/Fortran Compilers (GNU)


**Compile & Install**

Step 0: Git clone the repository.

Step 1: After installing Intel MKL, the evironment variable MKLROOT must be set via the command:

.. code-block:: console

	source /opt/intel/mkl/bin/mklvars.sh intel64  # or 
	source /opt/intel/oneapi/mkl/latest/env/vars.sh  intel64


Step 2: Compile CHOLMOD and related libraries from SuiteSparse (version 4.5.4 is provided) by the script:
	
.. code-block:: console
	
	sh make_cholmod.sh 


.. note::

	This script will copy the relevant files in the ``/extern/`` folder.

Step 3: Create the build directory and compile the project using cmake with the following commands:  

.. code-block:: console

	mkdir build
	cd build
	cmake ..
	make -j12

Step 4: Install the libSQUIC library in the default location (i.e., ``~/``) using the command:


.. code-block:: console

	make install 


.. _r package:


R Package
---------



.. _python package:


Python Package
--------------

# SQUIC Python3 Interface Package

SQUIC is a second-order, L1-regularized maximum likelihood method for performant large-scale sparse precision matrix estimation. This repository contains the source code for the Python(v3) interface of SQUIC. 

## Installation

#### Step 1: 
Download the shared library libSQUIC from www.gitlab.ci.inf.usi.ch/SQUIC/libSQUIC, and follow its README instructions. The default and recommended location for libSQUIC is the home directory, i.e., ``~/``.

#### Step 2: 
Run the following command to install the library:

.. code-block:: console

	pip install SQUIC

#### Step 3: 

Load the SQUIC package:

>>> import SQUIC

For further details type ``help(SQUIC)`` in the Python command line.

_Note: The number of threads used by SQUIC can be defined by setting the enviroment variable OMP_NUM_THREADS (e.g., ``base> export OMP_NUM_THREADS=12``). This may require a restart of the session)._

## Example

To run a simple example : 

```angular2
import SQUIC 
import numpy as np

# set location of libSQUIC (set after importing package)
SQUIC.PATH_TO_libSQUIC('/path/to/squic')

# generate sample from tridiagonal precision matrix
p = 1024
n = 100
l = .4

# generate a tridiagonal matrix
a = -0.5 * np.ones(p-1)
b = 1.25 * np.ones(p)
iC_star = np.diag(a,-1) + np.diag(b,0) + np.diag(a,1)

# generate the data
L = np.linalg.cholesky(iC_star)
Y = np.linalg.solve(L.T,np.random.randn(p,n))

[X,W,info_times,info_objective,info_logdetX,info_trSX] = SQUIC.run(Y,l)
```

