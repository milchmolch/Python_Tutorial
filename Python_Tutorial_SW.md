# URPP Tutorials
## URPP Evolution in Action

Stefan Wyder



# Introduction to Python

6 Functions  
7 Libraries  
8 Dictionaries  
9 Tuples  

**Optional topics**    
10 Sets  
11 [Repetition on Slicing](http://software-carpentry.org/v4/python/slice.html) (Access of lists, strings, tuples)  
12 String Functions



# 6. Functions

Functions are little stand-alone programs that are called from within your own program.

We define new functions using `def`. Let's define a very simple function `SquareNumber` that returns the square of a number:

```python
def SquareNumber(Number):
    Squared = Number ** 2
    return Squared
```
  
To call a function, we just use the function's name, followed by the arguments in parentheses:  
`SquareNumber(4)` returns 16.  
  
Function calls always have parentheses, if the function has no argument, with empty parentheses.


A function can have many `return`s:
```python
def sign(num):
    if num > 0:
	   return 1
	elif num == 0:
	   return 0
	else:
	   return -1
```


Be aware that any variables that we create as part of the function only exist inside the function, and cannot be accessed outside (which is also true in loops). 



## Default arguments

We can define default values. The function `adjust` has a required argument `value` and an optional argument `amount`: 
```python
def adjust(value, amount=2.0):
  return value * amount
```

If we call this function with one parameter, it is assigned to `value`, and 2.0 is used for `amount`
```python
print adjust(5)
```
```
10
```

If we call `adjust` with two parameters, the second overrides the default for `amount`.
```python
print adjust(5, 1.001)
```
```
5.005
```

## Why do we write functions?

Which one do you prefer?
```python
for x in for range(1, GRID_WIDTH-1):
   for y in for range(1, GRID_HEIGHT-1):
     if (density[x-1][y] > density_threshold) or \ if
        (density[x+1][y] > density_threshold):
        if (flow[x][y-1] < flow_threshold) or \ if
           (flow[x][y+1] < flow_threshold):
           temp = (density[x-1][y] + density[x+1][y]) / 2
           if abs(temp - density[x][y]) > update_threshold: if
              density[x][y] = temp
```

Or this one?
```python
for x in for grid_interior(GRID_WIDTH):
   for y in for grid_interior(GRID_HEIGHT):
      if density_exceeds(density, x, y, density_threshold):
         if flow_exceeds(flow, x, y, flow_threshold):
            update_on_tolerance(density, x, y, tolerance)
```

Human short term memory can hold 7+-2 items. If somebody has to keep more than a dozen things in their mind at once to understand a block of code: break it into comprehensible pieces with functions.

- Makes the code easier to read
- Avoids code duplication
- One task, one function
- Choose expressive names
- Tell us what the function is doing, not how
- Functions should not be longer than 60-100 lines

Good programmers will write first the scaffold and only then the functions it implies



## Exercises

1. Convert your code from Heidi Lischers Exercise 1a into a function that returns the sum of 
  the integers 1 + 2 + 3 + 4 .... + n
2. Calculate the sum for n = 10, n = 100 and n = 1000.
3. Make your script beautiful, choose expressive variable names, comment it. 


# 7. Libraries

Only the basic Python functionality is available out of the box. For more specialized tasks,
we need to import additional third-party functions bundled in **libraries** (aka modules or packages that are collections of modules).

There are thousands of libraries available for every imaginable task: web development, databases, game development,
image manipulation, system administration, ...

Some important libraries often used in scientific computing:

![Scientific Ecosystem Python](EcosystemHomoScientificus.png)

from [Christian Elsaesser](http://www.physik.uzh.ch/~python/python_2015-01/lecture4)
  
  
  
Library  | Description  
---------------- | -------------
math | Basic mathematical functions
os | Items related to the file system 
sys | System-level commands (sys.argv!)
random | Random sampling and numbers
NumPy | Multidimensional array, numerical computing, linear algebra
SciPy | Numerical integration & optimization
Sympy | Symbolic mathematics
Matplotlib | plotting
pandas | Data structures & analysis (data frames!)
rpy2 | Using R from python
IPython | Interactive Console
Biopython | Biological computation
pysam | SAM reading & manipulation



Particularly interesting for bioinformatics is **Biopython** which provides 
tools for computational molecular biology (sequence analysis, Alignment, BLAST, Phylogeny, Pubmed,...) and **pysam**, a module for reading and manipulating BAM files. 

For R users: there is **pandas**, a rising star inspired by data.frames in R. 




## Loading modules

In order to use a module, it must both be installed on your computer and imported into your program. There are some built-in modules that come with Python, including `sys`, `os`, `math`, `random`, `time` (obtaining and converting dates and times), `re` (regular expressions).

A module is imported using the `import` statement. For example, to import the module math, which contains many standard mathematical functions, we do:
```python
import math
print math.log(40)
```
```
3.68887945411 
```
  
Note that we to call the function using the `ModuleName.FunctionName` syntax.


A more convenient way if we need only 1 function is 
```python
from math import log
print log(40)
```
```
3.68887945411
``` 
  
Note that we called `log` directly.
  
  
We can also import a function under a different name (handy if two modules define functions with the same name)
```python
from math import log as ln
print ln(40)
```
```
3.68887945411
```

We can also use `import *` to bring everything in the module into the current namespace at once. This is convenient but can be dangerous, however, ff someone adds a new function or variable to the next version of the module, that `import *` could silently overwrite something that you're importing from somewhere else, leading to a hard-to-find bug.

```python
from math import *
print log(40)
```
```
3.68887945411
```

## (optional) Installing modules

With the exception of a the built-in modules (like os, sys, random) third-party modules have to be installed on your system. 

The way how modules are installed depends on the operation system and for each operating system there are several options. Usually the best way is first to check the package manager of the system (homebrew/fink/MacPort for Mac computers, apt-get for Ubuntu Linux). Check also the [official documentation](https://docs.python.org/2/install/).

You find detailed hints on the [start page](README.md) of this python tutorial.

Alternatively, you can install [Anaconda Python v2.7](http://continuum.io/downloads)
Anaconda already comprises the 200 most popular Python packages for science, math, engineering, data analysis.


## Exercise

Import the module math and list the functions (methods) that are provided by it:

Then in Spyder type 'math.', then a window open showing you all the available 

- Check for a few functions what they are doing using the help page

## (Optional) Command-line arguments

Copy the following lines and save them as `test_argv.py` using spyder.

```python
import sys
print sys.argv
```

Run the script with a command line argument, e.g. `python test_argv.py 4`. Run it also with multiple and without arguments.
Observe what is happening. 


\\#CountLines.py
```python
import sys

def count_lines(reader):
   return len(reader.readlines())

if len(sys.argv) == 1:
   LineCount = count_lines(sys.stdin)
else:
   rd = open(sys.argv[1], 'r')
   LineCount = count_lines(rd)
   rd.close()

print LineCount
```

If you make extensive use of command-line options have a look into the modules `argparse` and `getopt`.

Every Python file can be used as a library by other programs.


## Exercise

Writing a module is very easy.

Paste the code for the function `sign` from above into a file sign.py 
```python
def sign(num):
    if num > 0:
	   return 1
	elif num == 0
	   return 0
	else:
	   return -1
```

write another script that imports sign.py (`import sign`) and uses the sign function for printing the sign for [-8, -1, 0, 1, 6].


# 8. Dictionaries

- Dictionary (also known as associative array, hash or map) is another type of container
- Collection of names, or **keys**, with each key pointing to an associated **value**

- The keys for a dictionary must be unique
- Keys can be integers or strings ()
- Unordered: Elements don't have a fixed position


```python
PhoneNumbers = {}     				# Create an empty dictionary 
PhoneNumbers['John'] = ['463673']
PhoneNumbers['Mary'] = ['279943']
```

True to the dictionary analogy, values in dictionaries are looked up according to their keys, rather than by their position 
(as would happen in a list). 

We extract values from the dictionary using square brackets []
```python
print PhoneNumbers['Mary']
```
prints out Mary's phone number `279943`.


The contents of dictionaries can be modified
```python
PhoneNumbers['Mary'] = '123'
print PhoneNumbers['Mary']
```
```
123
```

There are several ways to create dictionaries. The most direct way is like this:
```
MyDictionary = {key:value, nextkey:nextvalue}
```
For example
```python
PhoneNumbers = {'John':'463673', 'Mary':'279943'}
```

In this example, the keys are strings, and the values are strings.

:bulb: Python does allow a statement to be split across lines if the splits occurs within (),[] or {}.
```python
PhoneNumbers = {
	'John':'463673',
	'Mary':'279943',
    'William':'393493',
    'Bob':'236333', 
	'Linda':'976866', }
```
is also valid, and often helps to make long list/dictionary entries more readible.



:bulb: The third way of creating is handy when you have 2 lists, one containing keys and the other containing values.
```python
Names = ['John','Mary']
Numbers = ['463673','279943']
PhoneNumbers = dict(zip(Names,Numbers))
```

If we are not sure whether a key is in a dictionary or not, we can test it:
```python
'Robbie' in PhoneNumbers
```
```
False
```

```python
'Mary' in PhoneNumbers
```
```
True
```

It is often convenient to combine dictionaries and lists, making a dictionary of lists. For instance, you could associate lists
of contact data with particular persons.

```python
Contacts = {}
\\# [PhoneNumber, City, Street, YearOfBirth] 
Contacts['John'] = ['463673', 'Zurich', 1984]
Contacts['Mary'] = ['279943', 'Paris', 1983]
Contacts['William'] = ['393493', 'London', 1990]
```

You can also have a dictionary of dictionaries.
  
  
  
In summary, dictionaries are more flexible than lists: In a dictionary, you can create an entry e.g. for key 16 without having any data corresponding to keys 0 to 15. This lets you fill in data as they become known.

Importantly, dictionaries provide efficient lookup.


## Important dictionary functions

### The .get() function

allows to extract values from a dictionary but with a default value to be returned if the entry doesn't exist.

```python
codon_table['atg']
codon_table.get('atg')  # is the same

codon_table.get('atg', 0)
```

### The .keys() function

Extracts a list of the keys in a dictionary. Remember that there is no intrinsic order to the keys or values in a dictionary.


### The .values() function

Extracts a list of the values in a dictionary. Remember that there is no intrinsic order to the keys or values in a dictionary .


### Listing keys and values

In Python pseudocode is similar to an if statement:

```
for MyItem in MyCollection:
   do a command with MyItem
   do another command
resume operation of main commands
```

You can loop through a dictionary using:

```python
for person in PhoneNumbers:
    print person, PhoneNumbers[person]
```
```
John 463673
Mary 279943
```

### Sorting

The power of dict comes now from sorting: We can sort the keys and then retrieve the associated values in a particular order.
After sorting, we get back a sorted list as dictionaries are unordered. 

```python
SortedKeys = sorted(PhoneNumbers.keys())
```

We can then loop over the sorted keys using (we don't have to write the `.keys()`):

```python
for PersonSorted in SortedKeys:
    print SortedPerson, PhoneNumbers[PersonSorted]
```

To get a list containing sorted values we do:
```python
SortedValues = sorted(PhoneNumbers.values())
```

If we need the key-value pairs sorted by values it is a bit more complicated:
```python
SortedValuesAsPairs = sorted(PhoneNumbers.items(), key=lambda x: x[1])
```
Again we are getting back a list.


The sorting order can be changed using the `reverse=True` parameter.


## Exercise: Codon Table

We have a dictionary codon_table containing the codon table, it will look up the amino acid
encoded by the codon. 

```python
codon_table = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M', 'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K', 'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L', 'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q', 'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V', 'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E', 'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S', 'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'*', 'TAG':'*', 'TGC':'C', 'TGT':'C', 'TGA':'*', 'TGG':'W'}
```

For instance, `print codon_table['ATG']` will print out 'M'


1. Write a script that prints out the protein sequence encoded by the DNA 'GTGCGGCCACCT'. Print out the position,the codon and the growing amino acid sequence
2. Convert it into a function (if you did not write it as a function)
3. Make your script safer by also allowing both upper and lower case letters, check that only valid letters occur, ...
:bulb: Use the Cheat Sheet to find out which string function to use for converting letters to  
4. Make a count table how many codons encode the same amino acid
5. Print out a sorted count table
6. Write a backtranslator (protein -> DNA)
:bulb: Make a second dictionary with reversed key-values


## Exercise: Counting insects

1. The following script you found on the internet. Try to understand it. How does the input file to be formatted?
2. Make up an input file and run the script.
3. Modify the script so that it reads in only the second column of each line.


```python
\\# CountInsects.py
import sys

def count_names(lines):
  '''Count unique lines of text, returning dictionary.'''

  result = {}                   # Create an empty directionary to fill
  for name in lines:            # Handle input values one at a time
     name = name.strip()
     if name in result:     # If we have seen this name before...
            result[name] = result[name] + 1    # add one to its count
     else:                                      # If it is the first time we see that name
            result[name] = 1

  return result


reader = open(sys.argv[1], 'r')
lines = reader.readlines()
reader.close()
\\# Count distinctive values
count = count_names(lines)
for name in count:
   print name, count[name]  
```



# 9. Tuples

Watch the [video](http://software-carpentry.org/v4/python/tuples.html) on tuples from Software Carpentry. 

- A tuple is just an immutable list
- created with parentheses
```python
names = ('John', 'Mary')
```

# (optional) 10. Sets

- A **set** is an unordered collection of distinct values.
- Their elements can't be modified after being added
- fast data structure

Main functions |    |
---------------|-----
set.add('')    | Add element
set.remove('')    | Remove element
  
  
  

Methods | Operators
--------|-----------
set1.difference(set2) | set1 - set2
set1.intersection(set2) | set1 & set2
set1.issubset(setall) | set1 <= setall
 | set1 < setall
set1.issuperset(setall) | set1 >= setall
 | set1 > setall
set1.symmetric_difference(set2) | set1 ^ set2
set1.union(set2) | set1 \| setall


# (optional) 11. String Functions

Check the String Functions on the Quick Reference Sheet Python 2.7 (http://www.astro.up.pt/~sousasag/Python_For_Astronomers/Python_qr.pdf)


# (optional) 12. Slicing

Watch the Repetition on Slicing [video](http://software-carpentry.org/v4/python/slice.html) (Access of lists, strings, tuples)
  
  
  
# Some tips how to write scripts

1. Write the simplest version of code
2. Refactor (remove duplication, reorganize the code)
3. If speed or memory are an issue: Optimize only at this point


Test the script with simple (simplified) made-up input file



# Sources

- [Software Carpentry v4](http://software-carpentry.org/v4/python/index.html)
- [Haddock & Dunn. Practical Computing for Biologists. Sinauer Associates 2011.](http://practicalcomputing.org))


# Learning more Python


**Cheat Sheets**
- [Quick Reference Sheet Python 2.7](http://www.astro.up.pt/~sousasag/Python_For_Astronomers/Python_qr.pdf)
- [CheatSheet from Practical Computing Biologists](http://practicalcomputing.org/files/PCfB_Appendices.pdf)

**More exercises**
- [Interactive basic course with exercises](http://www.codecademy.com/courses/python-beginner-en-pwmb1)
- [Exercises (requires registration)](http://www.programmr.com/zone/python)
  
**Other courses**
- [Python for Biologists (mostly sequence analysis)](http://pythonforbiologists.com/)
- [Dive into Python](http://diveintopython.org)
- [Scientific Programming with Python: 4-day course @ UZH](http://www.physik.uzh.ch/~python/python_2015-01/programme.php)
- [Edx online course <introduction into computer science>](https://www.edx.org/course/introduction-computer-science-mitx-6-00-1x-0#.VNOMIGSG-PW): Interesting but time-consuming (90hrs):


