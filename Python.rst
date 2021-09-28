######
Python
######

Tutorial
########

Section 3
*********

**Calculator**


* :code:`//` does floor division
* :code:`%` is the remainder
* :code:`**` calculates powers
* :code:`_` stores the last result shown on the interpreter
* :code:`j` creates complex numbers

**Strings**

:code:`-1` starts counting from the right

Slice indices have defaults: :code:`[2:]` means from 2 to the end, :code:`[:2]` means from 0 to 1 (2 excluded).

Strings are immutable, you cannot assign values to individual locations

:code:`len(s)` returns the length of a string


Section 4
*********

**if**

.. code-block:: python

  if   :
      ...
  elif   :
      ...
  elif   :
      ...
  else:
    
**for**

for iterates over the items of any sequence (list or a string)

.. code-block:: python

  for w in words[:]:

makes a copy of words and changes of words within the loop do not affect it.

This would not be true in

.. code-block:: python

 for w in words:

**range**

:code:`range(start, end, step)`

end is never included, all numbers can be negative

**else clause on loops**

executes when a loop finishes normally (i.e. not broken by a 'break' statement

**pass statement**

does nothing!

**functions**

.. code-block:: python

  def fib(n):
      """ this is a docstring """

docstring is a string that follows the function's definition and are used to automatically produce documentation. 

The execution of a function introduces a local 'symbol table'

arguments are passed by 'call by object reference'. The passed objects are mutable.

The in keyword checks whether a sequence contains an element or not 

The default arguments are evaluated only once (at the first function call).

**Keyword arguments**

keyword arguments must follow positional arguments

.. code-block:: python

  def func(*arguments, **keywords):

arguments will be a tuple with all positional arguments,

keywords will be a dictionary with all keyword arguments. 

`*a` unpacks tuples, or lists

`**keywords` unpacks dictionaries

**Documentation**

The first line should always be a short, consice summary of the object's purpose

**Coding style**

Use 4-space indentation, no tabs

Lines should not exceed 79 characters

Use blank lines to separate functions, classes and larger blocks of code

Use spaces around operators and after commas, but not directly inside bracketing constructs, e.g. a = f(1, 2) + g(3, 4)

Name classes and functions consistently e.g. classes with capitalised first letter of each word, functions separate with underscores.


Chapter 5. Data structures
**************************

---+++ Lists
list.append(x) is the same as list[len(list):] = [x]
list.extend(L) is the same as list(len(list):] = L

l1 + l2 concatenates 2 lists
lists are mutable
l[2:5] = [] removes elements 2-4 and shrinks the list!

y = x.copy()  
    deep copies a list to another

collections.deque implements a queue with fast pops from both sides. 

list.index returns the first index of value
x.index('hello') returns i where x[i] == 'hello'. Raises an error if 'hello' is not in the list.


---+++ Functional programming tools
filter(function,sequence)
returns a sequence consisting of those items for which function(item) is True 
map(function, sequence) calls function(item) for each of the sequence's items and returns a list of the return values
reduce(function, sequence) returns a single value constracted by calling the binary function 'function) on the 2 first items of teh sequence, then on the result and the next item, and so on.

---+++ List Comprehensions
can implement all three functional programming tools above. 

---+++ Tuples
Tuples are immutable
empty = () # Empty tuple
singleton = ('hello',) # note trailing comma, enclosing parentheses are not enough on their own.
t = (1,2,3)
x,y,z = t
This is tuple packing and sequence unpacking

---+++ Sets
defined with curly brackets, (apart from a={} which is a dictionary)
Sets are unordered collections with no duplicate element. Basic uses include membership testing, eliminating duplicate entries and mathematical operations like union, intersection, difference and symmetric difference.
a = set() defines an empty set (a = {} defines an empty dictionary)

---+++ Dictionaries
Three ways of defining dictionaries
1) tel = {'jack': 4098, 'sape': 4139}
2) tel = dict([('jack',4098), ('sape',4139)])
3) tel = dict(jack=4098, sape=4139)
and a fourth
4){x: x**2 for x in (2, 4, 6)}

---+++ Looping
for i,v in enumerate(List)
returns the index and the list element

for q,a in zip(list1, list2):
the result will be
q, a  = list1[0],list[0]
q, a  = list1[1],list[1]
........................

for i in reversed(list) # reverses the list

for i in sorted(list) # sorts the list

for k, v in dict.iterterms():

will return the key value pairs

If needing to change a list while looping through it, make a copy first.


Section 6. Modules
******************

reload() reloads a module, if it has been changed
sys.path shows the path where python searches for modules

A program doesn’t run any faster when it is read from a .pyc or .pyo file than when it is read from a .py file; the only thing that’s faster about .pyc or .pyo files is the speed with which they are loaded.

The module complie all can create .pyc (or .pyo) files for all modules in a directory.

Packages are a way of structuring Python’s module namespace by using “dotted module names”



Section 7 I/O formatting
************************

.format is the new method for formating strings
'string ' % (args) is the old method, much like a sprintf


Reading from and writing to files
f.open                    (opens)
f.read                    (reads everything)
f.readline                (reads a line)
for line in f :
                    (makes a loop that reads the file line by line
f.write                   (writes)
f.tell                    (tells the current position)
f.seek(offset, fromWhat)  (moves from 0: start, 1: current position, 2: end)
f.close                   (closes the file)


json module writes and reads json files



Section 8 Errors and exceptions
*******************************


Error is a syntax error (syntactic)

An exception is syntactically valid, but generates an error at run time


try:
except:
else:
finally:

code in else, is executed if the try block finished without raising an exception
code in finally is executed always (even if the try block is aborted with a break)


The 'with' statement allows objects like files to be used in a way that ensures they are always cleaned up promptly and correctly.

with open('file.txt') as f:
    code goes here

--- find elements in a vector ---
(cond).nonzero() returns a vector of True and False depending on condition

--- Converting iterable objects to lists ---
list(iter_obj)
dict(list(iter_obj)) creates a dictionary.


Section 9. Classes
******************

Classes have two kinds of attribute names: data attributes and methods

Data attributes can be added after instantantiation
    X = MyClass()
    X.a_new_data_attribute = 3
Data Attributes can be deleted
    del X.a_new_data_attribute 

Method vs function objects
    X = MyClass()
    MyClass.fun is a function object
    X.fun       is a method object

Class and Instance variables
    Class Dog:
        kind = []          # Class variable
        def __init__(self):
           self.name = []       # Instance variable
    
    Class variables are shared by all instances. So
    x = Dog()
    y = Dog()
    
    x.kind.append('wolf')
    will also make y.kind equal to ['wolf'].
    
    This happens when class variables are mutable objects, such as lists.
    
    To avoid conflicts between data and methods, use a naming conventions, 
    such as naming methods with capitalised names and data with small.

Multiple inheritance
    Class DerivedClassName(Base1, Base2, Base3):

    attributes not found in the DerivedClassName are looked for in Base1 first 
    and in all its subclasses, Base2 then and all its subclasses and so on 
    (Depth first, left to right).


super()
    When wanting to call a function of the parent class, we can do
    ParentClassName.method(self)

    Alternatively, we can just write
    super().method()
    Note that there is no need to write self within there (python 3)
    In python 2 this would have to be 
    super(ChildClassName, self).method()

    The advantages of super is that it takes care of multiple inheritance 
    issues.

    
    Example: Initialising a subclass

    class ChildClassName():
        def __init__(self, params, paramsParent):

            # Call the initialiser of the parent class
            super().__init__(paramsParent) 

            # Other stuff that apply only to the child class
            self.params = params


--------------------------------------------------------------------------------
Private variables
    Private instance variables that cannot be accessed but from within the 
    object, do not exist in Python. Variables that are not wished to be accessed
    as part of the class' API should be prefixed with an underscore (_varName)

Name Mangling
    Any identifier of the form __spam (at least two leading underscores, at most
    one trailing) is textually replaced with _classname__spam, where clssname is
    the current class name with leading underscore(s) stripped. 

Iterators
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]


Generators
    Generators are a simple tool for creating iterators. They are written like 
    regular functions but use the yield statement whenever they want to return 
    data.
    def reverse(data):
        for index in range(len(data)-1, -1, -1):
            yield data[index]
    
    >>>
    
    >>> for char in reverse('golf'):
    ...     print(char)
    ...
    f
    l
    o
    g



Pandas
######


Chapter 2
*********

    ? before or after a variable gives help about it.
    ?? prints the source code too!
    %run scriptname runs the script
    vim has 3rd party extensions for sending blocks of code directly to python
    %xmode changes the amount of information shown when an exception is thrown
    %reset clears the namespace
    %del deletes the variable from the namespace (, but not from the memory? 
    look it up)
    %logstart logs the session
    %who and %whos (as in matlab)
    ! runs commands on the shells
    $ inserts python variables in the shell e.g. 
        x = 'ls -l'
        !$x
        is equivalent to !ls -l
    Bookmarks
        %bookmark db /path/to/dir
        %cd db takes you to /path/to/dir
    Debugging
        %debug right after an exception invokes the post mortem debugger
        %pdb makes python invoke the debugger automatically after each exception
        %run -d filename invokes the debugger before executing the file
        %run -d -b2 /home/... starts automatically the debugger and sets the 
        breakpoint
    Timing
        %time     times a command once
        %timeit   times a command several times
    Profiling
        cProfile  profiles a file from the command line
        %run -p   profiles a whole file, as cProfile does, without exiting the 
        session
        %prun     profiles interactively a python statement
        %lprun    is a line by line profiler (needs to be installed first)
    Reloading modules
        dreload() function of ipythons deep (recursively) reloads modules
    Configuration
        ~/.ipython/profile_default/ipython_config.py


Chapter 3
*********

    ndarray: a multidimensional array object
    shape
    dtype
    np.array(list-like) creates an array
    np.zeros, np.empty, np.ones also create arrays
    dtypes: np.int32, np.float64
    x.astype() changes the type of the array
    astype always creates a copy of the array

    Operations
        arithmetic operations between arrays apply the operations elementwise

    Slicing
        Normal
            x[1:10:2] Normal slicing 1-dimension

            x[0][2] = x[0,2]  Normal slicing 2-dimensions

            x[-n] means the nth from the end (x[-1] is the last)

        Fancy slicing
            x[[1,2,3]] takes elements 1,2,3

            Passing multiple index arrays selects 1-D array of elements 
            corresponding to each tuple of the indices. E.g. 
            x[[1,2,3],[4,5,6]] will return the elements  
            x[1,4], x[2,5] and x[3,6].

            x[np.ix_([1,2,3],[4,5,6])] on the other hand will return a 3 x 3 
                matrix with rows 1,2,3 and columns 4,5,6 of the original. 
                In other words it returns:
                x[np.array([1,2,3]).reshape(3,1), 
                      np.array([4,5,6]).reshape((1,3))]

        Boolean slicing
            In boolean masking, to select every but 'Bob' you can do 
            x[~(names=='Bob')]

            Boolean indexing ALWAYS creates copies of the data!

            and and or keywords don't work with boolean arrays, use & and |

        Normal slicing broadcasts (gives views), Fancy and boolean slicing 
        copy

        x[,].copy() copies broadcast data. 

        np.where(cond, xarr, yarr) 
                checks condition, and returns the part of xarr where cond is 
                true and the part of yarr otherwise. yarr and xarr must have the
                same dimension (or 1 can be a scalar), and this is the dimension
                 of the returned matrix

                                                                                
    Reshaping and transposing 
        x.reshape((a,b), order={'C', 'F', 'A'}) 
                reshapes x
                                                                                
        x.T 
               transposes 2-D arrays. 

        x.transpose((x,y,z)) 
                transposes a 3-D array making the x dimension first, the y 
                second and the z 3rd. It works in higher dim. too.

    Concatenating
        np.concatenate([arr1, arr2], axis=1)
        the arrays must be at least two dimensional

        np.hstack(tuple), np.vstack
            'tuple' is a tuple of the variables to be stacked, i.e. (x1,x2)
             hstack stacks them horizontally. 
             vstack stacks them vertically.
             One dimensionaly arrays are stacked horizontally.


        
    Linear algebra (numpy.linalg)
        dot(A,B)
                matrix multiplication. can also be done as a.dot(b)

        diag, dot, trace, det, eig, inv, pinv, qr, svd, solve, lstsq

    Random number generation (numpy.random)
            normal, seed, permutation, shuffle, rand, randint, randn, binomial
            beta, chisquare, gamma, uniform  


    Universal functions perform elementwise operations on data in ndarrays
            abs, sqrt, exp, ceil, cos, add, subtract, maximum etc (p. 96)

    Vectorisation: replacing loops with array expressions 

    Statistical methods
        sum, mean, std, var, min, max, argmin, argmax, cumsum, cumprod

    Sorting
        np.sort(x) 
                returns a sorted array
        x.sort() 
                sort the array inplace

    Set logic
        unique, intersect1d, union1d, in1d, setdiff1d, setxor1d.
    
    File I/O
         numpy data
                 np.save, np.savez, np.load   
         
         text data 
                 np.loadtxt, np.genfromtxt, np.savetxt 

    Make copies
         y = x.copy()
         Assignment does not create real copies, but pointers to the same data.

    Combinatorics
    scipy.misc.comb

Chapter 4
*********

Series
    obj.values
    obj.index
    obj.name        (series name)
    obj.index.name  (index name)

    Usage
            Series([list data], index = [list index])  (from lists)
            Series({'index1': data1, 'index2': data2}) (from dictionary)

    isnull, notnull
             detect missing data (NaN). Both a function and an instance method.


DataFrame
    obj.columns
    Usage
            From a dict of equal length lists
            data = {'state': [1,2,3], 'year': [2000, 2001, 2002]}
            DataFrame(data)
            
            As a nested dict of dicts
            pop = {'Nevada': {2001: 2.4, 2002: 2.9}
                   'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}

            The DataFrame constructor takes in general:
            2D ndarray, dict of arrays, lists or tuples, dict of Series
            dict of dicts, list of dicts or series, list of lists or tuples,
            another dataframe, NumPy MaskedArray, NumPy structured/record array

    Index objects
        index objects have a number of methods and properties for set logic
        append, diff, intersection, union, isin, delete, drop, insert
        is_monotonic, is_unique, unique
        pd.Index(['a', 'b', 'c'], name='side']) creates an index, which can 
        be passed as an argument to either index or columns in the creation
        of a series/dataframe.
            

    del a['columnName'] 
           deletes the column

    df.dtypes 
            returns the data types of the columns

    Reindex
            obj.reindex changes the index, filling the gaps with NaN, a 
            specific value, or forward/backward fills
            In DataFrames it reindexes rows by default, and columns with the 
            keyword columns. Can also be done with .ix

    Dropping
            obj.drop(['name','name2'], axis)

    Slicing
        Series
            obj['b'], obj[['b', 'c', 'd']] index-based
            obj[1] obj[1:3] index based
            If the indices are integers, obj[1] will return the index 1 (not
            position)
             
        DataFrame
            Single square brackets
                obj['column_name'] returns the column
                obj[0:1] returns the first row
                I.e. when a column name, or a list of column names are entered
                in a single bracket the columns are returned.
                When a slicing or boolean operator is entered, the rows are 
                returned

                a.columnName is like a['columnName'] 
   
                irow, icol and get_value provide reliable position-based 
                indexing


            Double indices
                df.loc[:,:]  works with labels
                df.iloc[:,:] works with positions
                df.ix[:,:] usually behaves like loc, but falls back to behaving
                like iloc if the label is not in the index. In particular:
                    -> If the index is of integer type it will only use 
                    label-based indexing and not fall back to position based.
                    -> If the index does not contain only integers, then, given
                    an integer will only use position based.
                .ix is useful in mixing position with label arguments, e.g.
                df.ix[0:3, 'keys']

                df.iloc[[1,2,3],[1,2,3]] will not return the same as 
                ndarrayobject[[1,2,3],[1,2,3]] but the same as 
                ndarrayobject(np.ix([1,2,3], 1,2,3]) (see numpy notes above)


            DataFrame as index
                let 'a' be a DataFrame
                a[a>0] = a*10 will multiply all >0 elements with 10.
                Note that the rhs argument is the same DataFrame (i.e. has
                the dimension of the full DataFrame. This wouldn't work in 
                numpy where you'd have to write a[a>0] = a[a>0]*10. 
                This last expression also works with DataFrames
           
    Data alignment 
        Addition (+) creates unions of dataframes with NaN at the entries which
        are not common to both frames. The obj.add() method has a fill_value
        property for adding different values when missing

    frame.apply(f, axis)
            apply the function f along the axis

    frame.applymap(f)
            apply a function element wise

    series.apply(f)
            apply a function to a series element wise

    series.map(f)
            map series' values according to a func, dict or another series 
            the difference with the apply functions is that this also  
            takes a dict or a series for mapping

    frame.sort_index(axis, ascending)
            sort the indices on either axes
            frame.sort_index(by='column_name) sorts based on the values of 
            column_name

    series.order()
            for sorting series

    obj.index.is_unique
            checks if the indices are unique


    Summary and statistics
        df.describe()
            describe the dataframe 
            include = 'all' describes the non numeric columns too.
            include = [np.number] describes the numeric and include ='0'
            the string columns
        df.sum, df.mean, df.idxmin, df.idxmax, df.cumsum, 
        Properties: axis, skipna, level
        full table pp. 139


    Unique, value counts, membership
        s.unique, unique.sort
        s.value_counts   
        s.isin           (finds membership)
            df.column.isin(['a', 'b']) returns a true/false series depending 
            on whether df.column is 'a' or 'b'
        pd.any(axis) returns true if there is at least 1 true value across the
            selected axis
        df.count  counts non nan values


    Missing data
        Missing data are excluded in all the descriptive statistics
        dropna, fillna, isnull, notnull
        dropna
                In series it returns all the non nan entries
                In dataframes, by default drops the rows that contain at least
                one nan.
                how = 'all' will only drop rows that are all nan
                axis = 1    works for columns
                thresh = 3 will keep rows with at least 3 non nan values
        fillna
                fillna(0) replaces nan with 0
                fillna with a dictionary replaces different values in 
                different columns
                fillna returns a new object. inplace modifies the same object
                arguments: value, method, axis, inplace, limit 

    Hierarchical indexing
        Renaming levels
                data.index.names = ['key1', 'key2']
                data.columns.names = ['state', 'color']

        frame.swaplevel('key1', 'key2')
                swaps the priority of the levels 

        frame.sortlevel 
                sorts the levels (lexicographically)

        Summary statistics by level
                many summary statistics functions accept a level option


    Using columns as indices
        frame.set_index(['c', 'd'])
               sets columns 'c', 'd' as indices
               drop=False, leaves them as values as well

        frame.reset_index
               removes all the indices 

    df.astype
            changes the type of data

    Multiindex
        pd.MultiIndex.from_arrays([['US', 'US', 'US', 'JP', 'JP'],
           [1, 3, 5, 1, 3]], names = ['cty', 'tenor'])

Chapter 5
*********


    read_csv
            read csv files
    read_tab
            read tab delimited files. 
            Arguments:
            sep:          specifies custom delimiter. Also accepts regex
            header=None   for no header
            names:        specifies names
            index_col:    specifies column as a header. Given a list it
            produces hierarchical indices
            skiprows:     skips rows
            na_values:    defines sentinel for na values
    to_csv
            writes csv file

    csv.reader
            numpy's csv module provides lower level access to csv files
            useful if they are malformed and read_csv can't handle them

    pd.read_excel('Energy Indicators.xls', header=16, skiprows=[17],
                 skip_footer=38, parse_cols=[2,3,4,5], na_values='...')
 


Chapter 7
*********

    merge, concat, combine_first

    merge
        pd.merge(df1, df2, left_on='lkey', right_on='rkey')
        pd.merge(df1, df2, on='key') (if frames have a common key)
        how='inner' intersects, how='outer' unions,  
        how='left' uses only keys of the left frame, same for how='right'

    concat
        concatenates Series and DataFrames 

    combine_first
        b.combine_first(a) joins b with the elements of a that don't exist
        in b

    Reshape and pivot
        stack, unstack and pivot

        df.unstack(level=1)
            moves level 1 from the x axis to the y

    Transformations
        duplicated() 
                returns true for duplicated rows
        drop_duplicates()
                removes them
                data.drop_duplicates(['k1', 'k2'], take_last=True)
                will consider only columns k1 and k2 and keep the last instead
                of the first

        map
                series method, that takes a function, dict or a series as an
                argument and returns another series with the map. 
                Index objects also have a map method
     
        replace
                replace a value with another. takes a list or a dict as an 
                argument.  

        rename
                method for renaming indices

        Categorical objects
                x = pd.cuts(data, bins) returns a categorical object, where data
                are put into the given bins.
                x.labels returns number of the bin each data falls into
                x.levels returns the name of each bin
                pd.value_counts returns the number of data points in each bin.
                pd.qcuts produces quantiles

        Get dummy variables
                pd.get_dummies()

        String methods
                str.split
                ','.join(list of strings)
                str.index, string.find finds string within a string. index
                throws an exception if not found, find returns -1
                str.count counts the occurences of a string 
                str.replace('a', 'b') replaces a with b in a string

        Regular expressions
                re.compile compiles a regular expression for reusing
                Pandas have a string property that vectorises lots of string
                methods, including regular expressions.



Chapter 9
*********


    GroupBy
        df.groupby(df['key'])   (use the same df)
        df.groupby('key')       (same as above, using the column directly)
        df.groupby(df1['key'])  (use a different df)
        df.groupby(['key1', key2']) groups first by key1 and then by key2
    Methods
        count, size, sum, mean, median, std, var, min, max, prod, first, last

        count returns a count for each column separately excluding NAs.
        size returns the size for all the dataframe, including NAs
        
        
    groupby supports iterations as:
        for (k1, k2), group in df.groupby(['key1', 'key2']):

    GroupBy methods: Aggregate, Transform, Apply.

    list(df.groupby('key1') 
        returns a list with as many tuples as groups; the tuple's first element
        is its name and the second the respective data frame. 
        dict(list(df.groupby('key1'))) returns a dictionary.

    grouping can be performed with a dictionary which maps indices (or column 
    names (axis = 1)) to custom groups (pp. 257). The same can be achieved with 
    series and functions. 

    Aggregating with a custom function
        grouped.agg(fun) 
        The function 'fun' takes a series and returns a scalar.
        grouped.agg(fun) applies the function to each group and returns
        a row for each one.

    Transform
        grouped.transform(fun)
        Like aggregate, but returns a dataframe of the same size as the 
        original group. If the function returns a scalar, then the scalar
        is broadcasted to all the rows. If it returns a series (of the 
        same number of rows) then the elements of the new series are 
        placed at the position of the old

    Apply
        grouped.apply(fun)
        Most generic, applies the fun to each series in the data frame
        and attempts to concatenate the results.


Chapter 11
**********

    Align
        df1.align(df2, join='inner')
            aligns frames according to index 



Display options
***************

Max number of rows
    pd.options.display.max_rows = 999
Float formatting
    pd.options.display.float_format = '{:,.2f}'.format

Dates
*****

Creating date ranges
====================

.. code-block:: python

  pd.date_range('start-date', 'end-date', periods, freq)


Timestamps
==========

Time stamps is the most basic type of time series data that associates values with points in time. 

.. code-block:: python

   pd.Timestamp('2012-05-01')

Periods
=======

A period represents a span rather than a point in time

Time stamps is the most basic type of time series data that associates values with points in time. 

.. code-block:: python

   pd.Period('2011-01', 'M')

Time offsets
============

.. code-block:: python

   pd.Timedelta('1 day')


Rolling windows
===============

The code below, calculates the mean using a rectangular window of 60 points and plots the result...

.. code-block:: python

 df.rolling(window = 60).mean().plot()


Autocorrelation
===============

.. code-block:: python

 from statsmodels.tsa.stattols import acf

This calculates the autocorrelation at time t as

.. code-block:: python

  np.sum((x[:-t] - np.mean(x)) * (x[t:] - np.mean(x))) / np.sum((x - np.mean(x))**2)

Augmented Dickey Fuller test
============================

This tests the null hypothesis that the unit root is present in a time series sample. If the unit root is present then the sample is non-stationary. The null hypothesis therefore is that the signal is non-stationary. If the p value is sufficiently small then we can reject the hypothesis that the signal is non-stationary. 

.. code-block:: python

  from statsmodels.tsa.stattools import adfuller

Shift
=====

The shift method shifts the index by a desired number of periods

.. code-block:: python

  pd.Series.shift

Seasonal decomposition
======================

The following decomposes the signal into a trend, a seasonal component and a residual

.. code-block:: python

  from statsmodels.tsa.seasonal import seasonal_decompose









Plotting
########

Pyplot
******


Start by importing :code:`pyplot`

.. code-block:: python

  import matplotlib.pyplot as plt


Creating a new figure
=====================

One way of creating a figure is the following: 

.. code-block:: python

  fig = plt.figure()
  ax1 = fig.add_subplot(211)

The figure is created first and a plot is then added onto it. The axes referred to by :code:`ax1` will be the first axes in a 2-by-1 grid.

Another way of doing this in one go is 

.. code-block:: python

  fig, axes = plt.subplots(2, 2)

will create a new figure with a 2 by 2 grid of subplots. The handles to the subplots are given in axes which is now a list of lists. 


Multiple plots can share axes:

.. code-block:: python

  fig, axes = plt.subplots(2, 2, sharex=True, sharey=True)


White spaces around multiple plots can be adjusted

.. code-block:: python

  fig.subplots_adjust(wspace=0, hspace=0)

Adding Data
===========

Data can be added to subplots as 

.. code-block:: python

  ax1.plot(x, 'k--')
  ax2.hist(randn(500), bins=50, color='k', alpha=0.5)
  ax2.clear()

Some of the most common options are:

.. code-block:: python

  alpha: float # transparency
  color or c: # color
  drawstyle or ds: {'default', 'steps', 'steps-pre', 'steps-mid', 'steps-post'}, default: 'default'
  fillstyle: {'full', 'left', 'right', 'bottom', 'top', 'none'}
  label: object # Label for the legend
  linestyle or ls: {'-', '--', '-.', ':', '', (offset, on-off-seq), ...}
  linewidth or lw: float
  marker: marker style
  markeredgecolor or mec: color
  markeredgewidth or mew: float
  markerfacecolor or mfc: color
  markerfacecoloralt or mfcalt: color
  markersize or ms: float
  zorder: float # Order compared to the rest elements of the plot

Legends
=======

You can add a label to each line in a plot 

.. code-block:: python

  ax1.plot(data.cumsum(), 'k--', label='Default')

and then show the legend

.. code-block:: python

  ax1.legend(loc='best')

and its location can be set manually using

.. code-block:: python

  ax.legend(loc=(0.8, 0))
  ax.legend(loc='lower right')


Labels
======

The subplot title can be set as 

.. code-block:: python

  ax1.set_title('My first matplotlib plot')

The x and y labels 

.. code-block:: python

  ax1.set_xlabel('x-label')
  ax1.set_ylabel('y-label')

Limits and Ticks
================

The limits to the axes can be set as

.. code-block:: python

  ax1.set_ylim(-3, 9)
  ax1.set_xlim(0, 100)

Ticks can be added using

.. code-block:: python

  ticks = ax1.set_xticks([0, 250, 500, 750, 1000])
  labels = ax1.set_xticklabels(['one', 'two', 'three', 'four', 'five'],
                               rotation=30, fontsize='small')



**Locators and Formatters**

This code sets ticks along the :code:`y` axis at multiples of 1 and formats it as a float with 1 decimal point.

.. code-block:: python

   from matplotlib import ticker
   ax.yaxis.set_major_locator(ticker.MultipleLocator(1))
   ax.yaxis.set_major_formatter(ticker.StrMethodFormatter("{x:.1f}"))

:code:`xaxis` works correspondingly for the :code:`axis` and :code:`minor` for the minor instead of major ticks.


**Ticking on Dates**

The following tools make ticks along axes that contain dates easy to work with


.. code-block:: python

   from matplotlib import dates
   ax.xaxis.set_major_locator(dates.MonthLocator(bymonthday=[1, 15]))
   ax.xaxis.set_major_formatter(dates.DateFormatter('%b-%d'))


:code:`MonthLocator` ticks on specific dates in a month, while the library also offers from a :code:`MinuteLocator` to a :code:`YearLocator`.

The :code:`DateFormatter` uses the `strftime() <https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behaviorformats>`_ formatting conventions.


**Tick location**

The Tick's location can be modified using

.. code-block:: python

   ax.yaxis.tick_right()
   ax.yaxis.tick_left()
   ax.xaxis.tick_top()
   ax.xaxis.tick_bottom()


Grids
=====

The grid can be set using


.. code-block:: python

  ax.grid(which='major', axis='y', c='k', alpha=.1, zorder=-2)


Additionally, the lines along the axes can be removed using

.. code-block:: python

   ax.spines['left'].set_visible(False)
   ax.spines['bottom'].set_visible(False)
   ax.spines['right'].set_visible(False)


Annotations
===========

Annotations can be added as 

.. code-block:: python

  ax1.annotate(label, xy=(<location_of_point_to_annotate>),
               xytext=(<location_of_text>),
               arrowprops=dict(facecolor='black'),
               horizontalalignment='center', verticalalignment='top')
    

Shapes can be added to the plot as

.. code-block:: python

  rect = plt.Rectangle((0.25, 0.35), 0.5, 0.25, color='k', alpha=0.3)    
  circ = plt.Circle((0.7, 0.6), 0.15, color='b', alpha=0.3)
  pgon = plt.Polygon([[0.15, 0.15], [0.35, 0.4], [0.2, 0.6]],
                     color='g', alpha=0.5)
  ax1.add_patch(rect)
  ax1.add_patch(circ)
  ax1.add_patch(pgon)

Batch properties
================

Properties can be set in batches using

.. code-block:: python

  props = {
    'title': 'My first matplotlib plot',
    'xlabel': 'Stages'
  }
  ax.set(**props)


Visualising matrices
====================

Suppose that you have a matrix :code:`P` that is :code:`1200 x 54`. Its first row corresponds to the value :code:`0` and the last to the value :code:`12`. Its first column corresponds to value :code:`0` and the last to value :code:`53`. To visualise this matrix use the following


.. code-block:: python

   fig, ax = plt.subplots()
   pcm = ax.imshow(p, aspect='auto', origin='lower', extent=[0, 53, 0, 12])
   fig.colorbar(pcm, ax=ax)


* :code:`aspect` makes sure that the pixels do not have a ratio of 1. Useful for visualising thin matrices
* :code:`origin` places the top left corner of the matrix to the lower left corner of the plot
* :code:`extent` sets the values displayed along the :code:`x` (:code:`0, 53`)and the :code:`y` axes (:code:`0, 12`).




Pandas
******

Series

.. code-block:: python

  s = pd.Series(...)
  s.plot()

Dataframes 

.. code-block:: python

  df = pdDataFrame(...)
  df.plot()

You can place a plot in a specific subplot

.. code-block:: python

  fig, ax = plt.subplots(1)
  s.plot(ax=ax)

Different plot types

.. code-block:: python

  s.plot() # regular plot
  s.plot.bar() # bars
  s.plot.hist() # histogram

  s.plot(kind='bar', ax=axes[0], color='k', alpha=0.7)
  s.plot(kind='barh', ax=axes[1], color='k', alpha=0.7)

  df.plot(kind='barh', stacked=True, alpha=0.5)


Seaborn
*******

Import seaborn

.. code-block:: python

  import seaborn as sns

Barplots

.. code-block:: python

  sns.barplot(x= ,y= , data= , orient= )

Distribution plots (plots an estimage of the pdf and a histogram). 

.. code-block:: python

  sns.distplot(values, bins=100, color='k')

Regression scatter plots

.. code-block:: python

  sns.regplot('m1', 'unemp', data=trans_data)

Pair plots

.. code-block:: python

  sns.pairplot(trans_data, diag_kind='kde', plot_kws={'alpha': 0.2})

Categorical plots

.. code-block:: python

  sns.catplot(x='day', y='tip_pct', hue='time', col='smoker',
               kind='bar', data=tips[tips.tip_pct < 1])


The following sets the background for a plot.

.. code-block:: python

  sns.set(style={"darkgrid", "whitegrid", "dark"
                 "white", "ticks"})

Even though this is set using seaborn, it also applies to regular pyplot and pandas plots. 


Bokeh
*****

.. code-block:: python

  from bokeh.plotting import figure, output_file, show
  
  x = [1, 2, 3, 4, 5]
  y = [6, 7, 2, 4, 5]
  output_file("lines.html")
  p = figure(title="simple line example", x_axis_label='x', y_axis_label='y')
  p.line(x, y, legend="Temp.", line_width=5)
  show(p)


.. code-block:: python

  plt.ion()          #interactive on
  plt.title()        #prints title
  plt.imshow()       #shows images
  plt.colorbar()     #shows colorbar
  plt.set_cmap('')   #sets the colormap







Decorators
##########

Consider the following simple decoration example

.. code-block:: python

    def dec(f):
        def wrapper(*args, **kwargs):
            result = f(*args, **kwargs)
            return result
        return wrapper
    
    @dec
    def fun():
        return 0

`fun` is a function that is decorated by `dec`. The syntax 

.. code-block:: python

    @dec
    def fun():
        ...

is equivalent to 

.. code-block:: python

    fun = dec(fun)


A decorator is therefore a function that takes a function as an argument and returns another function. In the above example, the decorator `dec` takes the `fun` as an argument and returns `wrapper`. Therefore, when after the decoration the user calls the function `fun`, they are actually calling the function `wrapper` with the same arguments. The function `wrapper` can do in principle whatever it wants, but most often does some actions first, then calls the function that was decorated and gets any results, it then does some work after calling the function and it finally returns some result. A typical example can be 


.. code-block:: python

    def dec(f):
        def wrapper(*args, **kwargs):
            #Do some work before calling the decorated function
            #...

            #Call the decorated function
            result = f(*args, **kwargs)

            #Do some work after calling the decorated function
            #...

            return result
        return wrapper


JWT
###

JWT authentication is a method for verifying that a user that uses a webservice is the person they claim they are. It consists of 2 parts the encoding and the decoding.

Encoding
********

The encoding part needs the `payload`, which is a dictionary, and the `secret key` that is a string that should be known only to the server. The encoding procedure results to a JWT key that is used for further authentications.

Decoding
********

In the decoding part, the user that wants to be authenticated provides the JWT key and their user identity. The decoding process takes the JWT key and the secret key and if successful will return the `payload`. If the `payload` is what we think it should be then the authentication is successful. The decoding can fail for a number of reasons, the most common of which are 1) a wrong key was provided or 2) the JWT token has expired. These errors should be caught and dealt with accordingly. 

Example
*******

An encoding function can look like the following. A user_id is provided to the function and a JWT key is returned.

.. code-block:: python

    import jwt
    secret_key = 'a_secret_key'

    def encode(user_id):
    try:
        payload = {
            'exp': datetime.datetime.utcnow() + datetime.timedelta(days=0, seconds=10),
            'iat': datetime.datetime.utcnow(),
            'sub': user_id
        }
        jwt_encode = jwt.encode(
            payload,
            secret_key,
            algorithm='HS256'
        )
        # The .decode here is used because jwt_encode is binary.
        # nothing to do with the JWT encoding-decoding procedure
        return jwt_encode.decode()


The decoding procedure can look like this

.. code-block:: python

    def decode_auth_token(auth_token):

        try:
            payload = jwt.decode(auth_token, secret_key)
            return payload
        except jwt.ExpiredSignatureError:
            return 'Signature expired. Please log in again.'
        except jwt.InvalidTokenError:
            return 'Invalid token. Please log in again.'

If the decoding procedure is successful, the above function will return the `payload` that was given. In this particular example, if the user name provided with the JWT key during the authentication matches the `sub` field of the payload, the authentication is considered successful. 

Headers
*******

The authentication token is passed in the call in the headers of the call, which should look like this

.. code-block:: python

    headers={"Authorization": 'Bearer ' + auth_token}

General
#######

dir() shows the variables loaded in memory
dir(C) also gives the methods of an object
del variable deletes the variable


Modules
#######

import module as m : imports module
reload(m)          : reloads module in variable m
This works in python 2 only.

to check whether object a contains a method/datatype b
'b' in dir(a)

execfile runs a whole file 

Reloading with ipython

Type at the beginning of the ipython session 
%load_ext autoreload
%autoreload 2



Reshaping lists
###############

zip built in function

if you have a = [1,2,3] and b = [4,5,6] and you want to make [1,4,2,5,3,6] 
it can be done with 
[item for k in zip(a,b) for item in k]

zip(a,b) will return 
[(1,4),(2,5),(3,6)] 

and the list comprehension above can be read as

for k in zip(a,b)
  for item in k
     add item in the list


Debugging
#########

import pdb

enter in the code pdb.set_trace() if you want a break point

you can call a function using pdb.run('function(a,b)')

once in the debugger,
s steps in
n steps over
b 101 sets a breakpoint in line 101
c continues
q quits



Plotting
########

plt.ion() allows to use the terminal while plotting
plt.figure() allows the creation of a second figure etc. 



Standalone applications 
########################

to create standalone applications check
http://pythonhosted.org/py2app/tutorial.html

first make the setup file, including any extra files (e.g. images directory)
py2applet --make-setup Myapplication.py images

remove any previous builds
rm -rf build dist

build the application
python setup.py py2app -A

the -A flag builds it in 'alias' mode for testing. To build the final thing ommit '-A', i.e.
python setup.py py2app 


Write a matrix as binary data to a file
#######################################

suppose that Outputs is a m x n array

import numpy as np
import struct

# open file
fid = file('testpy.bin','wb') 

# get number of elements
n_elem = Outputs.size

# convert matrix to a m x n vector, in column major order ('F')
b = Outputs.reshape(n_elem, order='F')

# write to file 
`fid.write(struct.pack('d'*n_elem, *b))`
'd' is for double precision

# close file
fid.close()
 
Import csv
##########

a = np.genfromtxt('Inputs.csv',delimiter=",",dtype=float)



Change directory to scripts own directory
#########################################

import os
import sys

abspath = os.path.abspath(sys.argv[0])
# abspath = os.path.abspath(__file__) # This can be used alternatively
dname = os.path.dirname(abspath)
os.chdir(dname)

Unfold lists
############

if a = (q,w,e,r,t)
and a function myfun takes 5 arguments, 
`myfun(*a) will unfold the tuple and pass each element as a variable to the function`


Find the data type of an object
###############################

:code:`type(a)` returns the data type (list, dict, tuple etc).

:code:`a.__class__()` returns the class of an object.

Difference between is and ==
############################

:code:`is` checks whether two objects are equal.

:code:`==` checks whether the values are equal.

.. code-block:: python

  a = 1234
  b = 1234
  b is a  # false 
  b == a # true

However, for small integers (e.g. 1) both :code:`is` and :code:`==` can evaluate to :code:`true`. This is because python caches small integer objects and both the above tests would return true!!

In general, :code:`is` returns true if the operands are represented by the same object in memory. This is not always the case with objects that compare equal with :code:`==`

.. code-block:: python

  >>> a = 1
  >>> b = 1
  >>> a == b
  True
  >>> a is b
  True

  >>> a = 2000
  >>> b = 2000
  >>> a == b
  True
  >>> a is b
  False



Regular expressions
###################

re.search() finds the first instance of an expression
re.finditer() finds all the instances of an expression but returns an iterator
re.match() matches at the beginning of a string. re.search() matches anywhere.

let a = re.search('texttobeignored(texttobematched1)texttobeignored(texttobematched2)', texttobesearched)
if the search is successful, then
a.group(0) will be texttobeignoredtexttobematched1texttobeignoredtexttobematched2
a.group(1) will be texttobematched1
a.group(2) will be texttobematched2
a.span(0) will be the limits of a.group(0)
a.span(1) will be the limits of a.group(1) etc.

read all lines in a file
########################


with open('filename.txt') as f:
    for line in f:
        do something with line


ipython vim integration
#######################

create a profile using 
$ ipytthon profile create

then open the file 
~/.ipython/profile_default/ipython_config.py

and find and uncomment the line
c.TerminalInteractiveShell.editing_mode = 'vi'


Byte Encoding Decoding
######################


A string can be turned into bytes with :code:`s.encode()`

.. code-block:: python

    > s = 'hello'
    > b = s.encode()
    > b
       b'hello'

A byte object can be turned to a string with :code:`b.decode()`

.. code-block:: python

    > b.decode()
       'hello'

A byte object can be represented as integers in the 0-255 range by converting it to a list

.. code-block:: python

    > list(b)
       [104, 101, 108, 108, 111]

A list of integers can be turned to bytes with :code:`bytes()`

.. code-block:: python

    > bytes([104, 101, 108, 108, 111])
       b'hello'


Bitwise operations 
##################



bin() 
    returns the binary form of a number
hex() 
    returns the hexadecimal form of a number
int(x, base=y)
    converts the number from base y to base 10
~
    negation of a number
<<
    left shift
>> 
    right shift
^
    bitwise xor
&
    bitwise and 
|   
    bitwise or


Short circuit evaluation
########################

'and' and 'or' are short circuit operators: if the first operand suffices to 
determine the expression, the second is not evaluated
'&' and '|'    are eager operators: both are evaluated anyway.

Example
x = 0

if x > 0 and int(3/x) > 0 : 
    print("ok")
# Works, int(3/x) > 0 never gets evaluated, x > 0 is enough 
to determine that the whole expression is false

if x > 0 & int(3/x) > 0 :
    print("ok")
# Does not work, int(3/x) > 0 gets executed anyway and raises an error.


print formatting
################

np.set_printoptions(threshold=np.nan)
pd.options.display.float_format = '{:,.2f}'.format


line profiler
#############


* git clone https://github.com/rkern/line_profiler.git
* python setup.py build
* python setup.py install

* Decorate the function you want to profile with :code:`@profile`

* run the profiler with

.. code-block:: python

    kernprof -l file_to_profile.py`

Print the results with 

.. code-block:: python

    python -m line_profiler file_to_profile.py.lprof 



Virtual environments
####################

Linux 
*****

.. code-block:: bash

    # Create environment
    python -m venv <env_name>

    # Activate
    source <env_name>/bin/activate

    # Upgrade pip
    python -m pip install --upgrade pip

    # Install requirements
    python -m pip install -r requirements.txt

    # Deactivate
    deactivate

Windows
*******

.. code-block:: bash

    # Create environment
    python -m venv <env_name>

    # Activate
    .\<env_name>\Scripts\activate

    # Upgrade pip
    python -m pip install --upgrade pip

    # Install requirements
    python -m pip install -r requirements.txt

    # Deactivate
    deactivate

Windows with Anaconda 
**********************

.. code-block:: bash

    # Create an environment
    conda create --name <name>

    # Create environment from file
    conda env create -f <filename.yml>

    # Activate the environment
    conda activate <name>

    # Deactivate the environment
    conda deactivate <name>

    # List environments
    conda env list

    # Remove environment
    conda remove --name <name> --all

* The environments are created in :code:`C:\ProgramData\Anaconda3\envs`. It is possible to create them at a different location using the :code:`--prefix` flag of :code:`conda create`.
* After an environment is deleted, an empty folder might be left in :code:`C:\ProgramData\Anaconda3\envs`, which can be removed manually. 
* Packages can be added with :code:`conda install...` once the environment is activated, or by specifying them in the creation of the environment as 

.. code-block:: bash

    # Create virtual environment
    conda create -n <name> numpy=1.16

or in a :code:`yml` file as:

.. code-block:: 

    name: dash
    dependencies:
      - python
      - pandas=1.18
      - numpy


Full instructions in https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

Visual Studio Code integration
##############################
To use a specific interpreter, add in the :code:`settings.json` (either the project's one or the global) the following line (for a windows env):

.. code-block:: 

    "python.pythonPath": "C:\\path\\to\\env\\Scripts\\python.exe"

For easy debugging, add a launch.json configuration as follows:

.. code-block:: 

        {
          "name": "Python: Current File",
          "type": "python",
          "request": "launch",
          "program": "${file}",
          "console": "integratedTerminal"
        }


AWS cli
#######


First start a new virtual environment as shown above. 

Then upgrade pip. 

.. code-block:: bash

    pip install --upgrade pip

Install `awscli`

.. code-block:: bash

    pip install --upgrade awscli

Check the aws version

.. code-block:: bash

    aws --version 

Run `aws configure` and enter your credentials


