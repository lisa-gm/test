Examples
========

.. _python:

Python
------

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


.. note::

	The number of threads used by SQUIC can be defined by setting the enviroment variable OMP_NUM_THREADS (e.g., ``export OMP_NUM_THREADS=12`` from the command line). This may require a restart of the session).



.. _R:

R 
---


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


.. note::

	The number of threads used by SQUIC can be defined by setting the enviroment variable OMP_NUM_THREADS (e.g., ``export OMP_NUM_THREADS=12``) from the command line. This may require a restart of the session).


