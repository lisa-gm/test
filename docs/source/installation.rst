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

`For Mac:` 

- CMake (>3.9): For example using homebrew ``brew install cmake``


- `Intel MKL libraries <https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit/download.html?operatingsystem=mac&distributions=webdownload&options=online>`_

- OpenMP: For example using homebrew ``brew install libomp`` 

- C++/C Compilers (Clang): This is the default; nothing is required.

- Fortran Compiler (GNU): For example, using homebrew ``brew install gcc``

`For Linux:`

- CMake (>3.9)

- `Intel MKL libraries <https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit/download.html?operatingsystem=linux&distributions=webdownload&options=online>`_ 

- OpenMP 

- C++/C/Fortran Compilers (GNU)


**Compile & Install**

`Step 0:` Git clone the repository.

`Step 1:` After installing Intel MKL, the evironment variable MKLROOT must be set via the command:

.. code-block:: console

	source /opt/intel/mkl/bin/mklvars.sh intel64  # or 
	source /opt/intel/oneapi/mkl/latest/env/vars.sh  intel64


`Step 2:` Compile CHOLMOD and related libraries from SuiteSparse (version 4.5.4 is provided) by the script:
	
.. code-block:: console

	sh make_cholmod.sh 


.. note::

	This script will copy the relevant files in the ``/extern/`` folder.

`Step 3:` Create the build directory and compile the project using cmake with the following commands:  

.. code-block:: console

	mkdir build
	cd build
	cmake ..
	make -j12

`Step 4:` Install the libSQUIC library in the default location (i.e., ``~/``) using the command:


.. code-block:: console

	make install 


.. _r package:


R Package
---------

.. highlight:: R

`Step 1:` Download the shared library libSQUIC from `www.gitlab.ci.inf.usi.ch/SQUIC/libSQUIC <www.gitlab.ci.inf.usi.ch/SQUIC/libSQUIC>`_, and follow its README instructions. The default and recommended location for libSQUIC is the home directory, i.e., ``~/``.

`Step 2:` Use ``devtools`` to install SQUIC R package.

.. code-block:: R

	library(devtools) 
	# The environment variable PATH_TO_libSQUIC defines the location of libSQUIC - this is by default set to the home directory of the user.
	# If this is not the location of libSQUIC, it can be changed via the command Sys.setenv(PATH_TO_libSQUIC="/path/to/squic/") 
	install_git("https://www.gitlab.ci.inf.usi.ch/SQUIC/SQUIC_R.git")


.. note::

	Along side ``devtools``, the packages ``Rcpp`` and ``RcppArmadillo`` are required for compilation. These can all be insall via the command ``install.packages(c("devtools","Rcpp","RcppArmadillo"))``.

`Step 3:` Load the SQUIC package:

.. code-block:: R

	library(SQUIC)  

For further details type ``help(SQUIC)`` in the R command line.

.. note::

	The number of threads used by SQUIC can be defined by setting the enviroment variable OMP_NUM_THREADS (e.g., ``export OMP_NUM_THREADS=12`` from the command line). This may require a restart of the session).

Example
-------

To run a simple example : 

.. code-block:: R

	library(SQUIC)

	p = 1024
	n = 100
	lambda = .4

	# generate a tridiagonal matrix
	iC_star = Matrix::bandSparse(p, p, (-1):1, list(rep(-.5, p-1), rep(1.25,   p), rep(-.5, p-1)));

	# generate the data
	z    = replicate(n,rnorm(p));
	iC_L = chol(iC_star);
	data = matrix(solve(iC_L,z),p,n);

	# Run SQUIC
	out <- SQUIC(data,lambda)


.. _python package:


Python Package
--------------

.. highlight:: python

`Step 1:` Download the shared library libSQUIC from `www.gitlab.ci.inf.usi.ch/SQUIC/libSQUIC <www.gitlab.ci.inf.usi.ch/SQUIC/libSQUIC>`_, and follow its README instructions. The default and recommended location for libSQUIC is the home directory, i.e., ``~/``.

`Step 2:` Install the Python interface using pip, i.e.

.. code-block:: console

	pip install SQUIC

`Step 3:` Load the SQUIC package:

>>> import SQUIC

For further details type ``help(SQUIC)`` in the Python command line.


.. note::

	The number of threads used by SQUIC can be defined by setting the enviroment variable OMP_NUM_THREADS (e.g., ``export OMP_NUM_THREADS=12``) from the command line. This may require a restart of the session).



Example
-------

To run a simple example : 

.. code-block:: default

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


