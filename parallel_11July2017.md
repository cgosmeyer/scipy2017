# Parallel Python
### 11 July 2017

General about parallel programming, not a speficic tool.

Multithreading: Share data. C/Numpy/Pandas. Fast but dangerous.
Multiprocessing: Move data around. Pythonic. Safe.
Distributed System: A central schedular who has many workers (the idea behind spark, dask, tensorflow). 

*Never try to write new code in parallel! Always start in serial, being sure it is totally debugged.*  Get it working with small data first.

## Environment

Can use docker where all packages are pre-loaded in an image. 


## 01-Map

Can make a task parallel by transforming the body of a for loop into a function and then using `ProcessPoolExecuter` to apply that function across all the filenames in parallel using multiple processes.

For example, sequential code (processing each file independently):

	results = []
	for x in L:
		results.append(f(x))

	#or
	results = [f(x) for x in L]

Can write instead in parallel:

	from concurrent.futures import ProcessPoolExecutor
	e = ProcessPoolExecutor()
	results = list(e.map(f, L))

Useful way to visualize computation time (profile code) is the magic `%%snakeviz`.

## 02-Submit

`submit` is an interface that asynchronously calls individual functions. You can submit different functions and can perform a bit of logic on each input.

Sequential:
	
	results = [slowadd(i, i, delay=1) for i in range(10)]

In parallel with `submit`:

	futures = [e.submit(slowadd, 1, 1, delay=1) for i in range(10)]
	results = [f.result() for f in futures]

The "futures" are pointers to remote results that will happen in the future. 

`ThreadPoolExcecutor` vs `ProcessPoolExecutor`: Threads are happening in the same program and data doesn't need to be moved vs data is moved between processes. The latter is slower, but threads can be unsafe. With numpy, pandas, scikit-learn, probably want to use threads. With HDF5, pandas, probably want to use processes. (Though most recent version of HDF5 is now thread-safe.)

If a job or function call is raised during `submit`, you only need to inspect the result to see the exception.


## 03-Collections-Spark-and-Dask

Collections provide a fixed set of operations, eg, `map`, `filter`. `groupby`, and `join`.  Systems like Spark and Dask include "big data" collections. Good for handling nested for loops.

Sequential example:

	series = []
	for fn in filenames:   # Simple map over filenames
    	series.append(pd.read_hdf(fn)['close'])

	results = []

	for a in series:    # Doubly nested loop over the same collection
    	for b in series:  
        	if not (a == b).all():     
            	results.append(a.corr(b))  # Apply function

	result = max(results)

Parallel code with Dask:

	import dask.bag as db

	b = db.from_sequences(filenames)
	series = b.map(lambda fn: pd.read_hdf(fn)['close']) # pair all possible combinations.

	corr = (series.product(series)
				  .filter(lambda ab: not (ab[0] == ab[1]).all())
				  .map(lambda ab: ab[0].corr(ab[1]))
				  .max())
	result = corr.compute()

## 04-Parallel-Dataframes

When applications align with "big data" collections, parallel computing looks a lot like normal computing.


## 05-Cross-Validated-Parameter-Search

An application can be to find the optimal parameters for tuning a machine learning model.  (So sampling parameter space in parallel.)

For example,

	param_grid = {
    'C': np.logspace(-10, 10, 1001),
    'gamma': np.logspace(-10, 10, 1001),
    'tol': np.logspace(-4, -1, 4),
	}

	param_samples = ParameterSampler(param_grid, 10) # increase 10 to better fill in param space

	from concurrent.futures import ThreadPoolExecutor
	e = ThreadPoolExecutor(4)

	futures = []

	for split in cv_splits:
		for params in param_samples:
			future = e.submit(evaluate_one, SVC, params, split)
			futures.append(future)
	results = [f.result() for f in futures]

## 07-Distributed-Cross-Validation-Parameter-Search

Can do the same as above, but even faster, if use a cluster. 

	cv_splits = [load_cv_split(i) for i in range(2)]  # Increase the number 2 after parallel computation acheived
	param_samples = ParameterSampler(param_grid, 100)    # Increase the number 10 after parallel computation acheived

	from dask.distributed import Client
	e = Client('schedulers:9000')

	futures = []

	parameters = list(param_samples)

	for split in cv_splits:
		for params in parameters:
			future = e.submit(evaluate_one, SVC, params, split)
			futures.append(future)

	results = [f.result() for f in futures]

## 08-Sort-Groupby

It is important to know alternative algorithms to achieve same results in faster time.  

For example, 

	rdd.sortBy(lambda t: t, ascending=False).take(5)

Vs

	rdd.top(5) 

`top` is faster because algorithm doesn't sort whole list, just smartly gets the top 5 values. Whereas `sortby` sorts the whole list and then skims off the top 5 values.


## 09-Distributed-Dataframes

If a file is too large to load into memory, can use a cluster to load the file into smaller dataframes. 

*No median function in parallel - very difficult to do.*

## 10-Performance-Tuning


