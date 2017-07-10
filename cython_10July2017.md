# Cython Tutorial
### 10 July 2017
-Dillon Niederhut (Enthought)

When should you use Cython vs Numpy?

Numpy is an intermediate solution to speeding up Python. Try Numpy first. But there are some things Numpy will not do.

Cython is better for iterative code. Numpy better for vectorized code.

Cython is basically a translator between Python and C.

If coding outside of the notebook, instead of %%cython magic, you compile a whole 'pyx' file with cython.

	cython -a example.pyx


## 00-Intro

Cython good for heavy numerial computing.

Some C programmers now code in Cython because easier to read and pull new people into project.

To see where the C code gets stored:

	ls ~/.ipython/cython

Cython code exists to be turned into C. The C code is then translated to machine language, the *.so files.

## 01-Python-Slow

For addition, for instance, Python is a fraction slower than C. This adds up when do many additions in a row.

The `dis` module disassembles Python code. 

Why is Python "slower"?

- C-level is interpreting bytecodes
- Polymorphism: Python code can handle adding strings, ints, etc.
- Error checking.

Putting all this weight on the Python interpreter makes it slower than C.

	%timeit func(variables)

## 02-Cython-Comparison

Example of declaring a Cython function in the notebook.

	%load_ext Cython

	%%cython -n cyfoo

	def cyfoo(a,b):
		return a + b

Cython gives speed by by 
	
	* Converting from interpreted to compiled code.

	* Going from dynamic to static typing.

	* Using `cpdef` (basically a C function written in Python syntax) and declaring C-type variables:

	cpdef double cyfac_loop(int n):
    	cdef double r = 1.0
    	cdef int i
    	for i in range(1, n+1):
        	r *= <double>i
    	return r

## 03-Cython-Types

All valid Python code is valid Cython code. Cython, however, gives access to a lot of C things.

Cython enforces static typing (Cannot set a varaible declared to be an int to a string)

Doubles are preferred for compatibility with Python `float` types.

Rarely need to use pointers in Cython unless interfacing with a C library.

## 04-Cython-Functions

Cython supports three kinds of functions:

	1. Python `def` functions: compiled Python functions that work with Python Types.

	2. C-level `cdef` functions: low-overhead C-level functions that support C-only types. C-functions with Python-like syntax. 

	3. Hybrid `cpdef` functions: C-level functions with auto-generated Python compatibility wrappers.

`cdef` variables/functions are not visible to Python outside defining scope. 

`cpdef` functions are two functions in one. They are just like `cdef` functions with an implicitly defined Python wrapper for free. You cannot use C-only types here.

To get helpful error messages from `cdef` and `cpdef` functions:

	cpdef int checked_div(int a, int b) except *:
    	return <int>(a / b)

Versus just

	cpdef int unchecked_div(int a, int b):
    	return <int>(a / b)

To raise exceptions with C return types,

	cpdef int func() except -1: 
    	# ...                   
    	raise ValueError("...")

We guarantee that -1 is never a valid return, so Cython uses it as a sentinal to flag that an exception has occurred. 

## 05-Profiling-and-Performance

With magic `%%prun` can display what time was spent in each function.

But notice that profiling is run in Python, so actual time of the Cython code is not what is shown. But profiling is useful to see which functions are causing slowdowns.

Includes overhead.

	cython file.pyx -a 

Will generate an html representation of the Cython optimizations. 

	Yellow == slow

Often all you need to do is declare types.

Can redefine as cdef or cpdef. Can speficiy the return type (by default, assumes a python object), for example, 

	cdef int func(variable)

## 06-Numpy-Buffers-Fused-Types

`cimport` allows interfacing with other Cython code at the C-level and at compile time. Whereas the regular `import` statement interfaces with other Python modules at runtime.

For example,

	cimport numpy as cnp

We can also cimport C and C++ function from the C and C++ standard (template) libraries.

For example, to access functions in the C stdlib math.h header file,

	from libc.math cimport exp, log, sqrt

## 07-Cythonize-Pandas-GroupBy

Can speed up `Pandas GroupBy` by rewriting with `Numpy` arraus and get further speed ups with Cython.


	def splitby(df):
	    idx = df.index
    	# NumPy array of "posts" that delineate the row indices
    	# with which to split the dataframe.
    	posts = np.where(idx[1:] != idx[:-1])[0] + 1
    	split_labels = idx[np.concatenate([[0], posts, [-1]])]
    	split_data = np.split(df.values, posts, 0)
    	return list(zip(split_labels, split_data))

Versus a Cython version:

	cimport cython
	cimport numpy as cnp
	import numpy as np

	def splitby_cython(df):
    	cols = df.values
    	idx = df.index.values
    	n = idx.shape[0]
    	result = []
    	thispost = 0
        
    	for i in range(1, n):
    	    if idx[i] != idx[i-1]:
    	        result.append((idx[i-1], cols[thispost:i]))
    	        thispost = i
            
    	result.append((idx[i-1], cols[thispost:]))
    	return result


## 08-Extension-Types

Can declare types of *instance* attribues at compile-time.



