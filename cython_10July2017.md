# Cython Tutorial
### 10 July 2017
-Dillon Niederhut (Enthought)

When should you use Cython vs Numpy?

Numpy is an intermediate solution to speeding up Python. Try Numpy first. But there are some things Numpy will not do.

Cython is better for iterative code. Numpy better for vectorized code.

Cython is basically a translator between Python and C.

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

`cpdef` functions are two functions in one. They are just like `cdef` functions with an implicitly defined Python wrapper for free. 

To get helpful error messages from `cdef` and `cpdef` functions:

	cpdef int checked_div(int a, int b) except *:
    	return <int>(a / b)

Versus just

	cpdef int unchecked_div(int a, int b):
    	return <int>(a / b)

