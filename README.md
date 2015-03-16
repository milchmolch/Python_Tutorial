# URPP Tutorials
## URPP Evolution in Action

Stefan Wyder & Heidi Lischer


## Introduction to Python

1. Introduction (Why Python? IDE) 


2. Functions
3. Libraries
4. Set, Dictionnaries, Tuples
5. Slicing

Strings (Methods, )


## 1. Introduction


### Some examples

Nice Visualizations
The future of analysis notebooks: IPython Notebooks
Interactive Websites


### Why Python?

- clean & simple language  
  easy to read, fast & easy development
- expressive language  
  fewer lines, fewer bugs
- ease of manipulating text data
- popular in biology/bioinformatics/genomics
- (relatively) fast
- dynamic ecosystem
- used also in industry (Google, YouTube, NASA, ...)


### IDE (Integrated Development Environment

Today we will use [Spyder](https://github.com/spyder-ide/spyder).

Spyder is comprises an editor, debugger and 

######! [Screenshot SPYDER](Screenshot_SPYDER.png)
<img src="Screenshot_SPYDER.png" width="400">
Screenshot_SPYDER.png

Spyder is a free interactive development environment for the Python language with advanced editing, interactive testing, debugging and introspection features. It was designed to provide MATLAB-like features (integrated help, interactive console, variable explorer with GUI-based editors for NumPy arrays and Pandas dataframes), it is strongly oriented towards scientific computing and software development.

- Editor/Debugger: To show errors & warnings hover over warning triangles on the left of line numbers
- Interactive prompt: handy to test pieces of code


### Getting help

- The help() function and pydoc are often not very helpful
- Use Google
- use dir()  
e.g. 
list available methods for a variable
```python
a = 'hello'
dir(a) 
```

it also works for modules, e.g. import sys ; dir(sys)


https://docs.python.org/2/


## 2. Functions

Functions are little stand-alone programs that are called from within your own program





## 3. Libraries

modules vs libraries vs packages


Only the very basic Python functionality is available out of the box. For more specialized tasks,
we need to import additional functions bundled in **modules**.


Some important libraries often used in scientific computing:

The Ecosystem of Homo Python Scientificus
![Scientific Ecosystem Python](EcosystemHomoScientificus.png)

from Christian Elsaesser's course @UZH [<Scientific Programming with Python>](http://www.physik.uzh.ch/~python/python_2015-01/lecture4).


-------- | -------------
Library  | Description
---------------- | -------------
os | 
sys |
NumPy | Multidimensional array, numerical computing, linear algebra
SciPy | Numerical integration & optimization
Matplotlib | plotting
pandas | Data structures & analysis 
IPython | Interactive Console
Biopython | Biological computation
Sympy | Symbolic mathematics


Particularly interesting for bioinformatics is **Biopython** which provides 
tools for computational molecular biology (sequence analysis, Alignment, BLAST, Phylogeny, Pubmed,...)

For R users: there is **pandas**, a rising star inspired by data.frames in R. 

There are thousands of libraries available for every imaginable task: web development, databases, game development,
image manipulation, system administration, ...


### Loading modules

To use a module in a Python program it first has to be imported. A module can be imported using the import statement. For example, to import the module math, which contains many standard mathematical functions, we can do:

import math


import sys

print sys.argv




### Installing modules





pipy
easy_install
Linux
Anaconda: conda



## 4. Dictionaries

- Dictionaries (also known as associative array, hash or map) is another type of container
- Collection of names, or **keys**, with each key pointing to an associated **value**

- The keys for a dictionary must be unique
- Keys can be integers or strings ()
- Unordered: Elements don't have a fixed position

```python
PhoneNumber = {}     				# Create an empty dictionary 
PhoneNumber['John'] = ['463673']
PhoneNumber['Mary'] = ['279943']
```

True to the dictionary analogy, values in dictionaies are looked up according to their keys, rather than by their position 
(as would happen in a list).


There are several ways to create dictionaries. The most direct way is like this:
```
MyDictionary = {key:value, nextkey:nextvalue}
```
For example
```python
PhoneNumber = {'John':'463673', 'Mary':'279943'}
```

In this example, the keys are strings, and the values are strings.


We extract values from the dictionary using square brackets []
```python
print PhoneNumber['Mary']
```
would print out Mary's phone number.


*Tip* Python does allow a statement to be split across lines if the splits occurs within (),[] or {}.
```python
PhoneNumber = {
	'John':'463673',
	'Mary':'279943' }
```
is also valid, and often helps to make long list/dictionary entries more readible.


*Tip* The third way of creating is handy when you have 2 lists, one containing keys and the other containing values.
```python
Names = ['John','Mary']
Numbers = ['463673','279943']
PhoneNumber = dict(zip(Names,Numbers))
```


Its often handy to combined dictionaries and lists, making a dictionary of lists. For instance, you could associate lists
of Blast hits with particular proteins.

```python
TreeStat = {}
TreeStat['Kodiak'] = [68, 57.8, -152.5]
TreeStat['Juneau'] = [73, 48.7, -134.2]
TreeStat['Barrow'] = [13, 52.5, -156.5]
```

More flexible than lists: In a dictionary, you can create an entry e.g. for key 16 without having any data corresponding to keys 0 to 15. This lets you fill in data as they become known.


### Exercise: Codon Table

We use Python's elegance for preparing a codon table:
```python
bases = ['t', 'c', 'a', 'g']
codons = [a+b+c for a in bases for b in bases for c in bases]
amino_acids = 'FFLLSSSSYY**CC*WLLLLPPPPHHQQRRRRIIIMTTTTNNKKSSRRVVVVAAAADDEEGGGG'
codon_table = dict(zip(codons, amino_acids))
```

Recode the above functionality in a less pythonic (more general) way using *for* loops


Now we have a dictionary codon_table containing the codon table, it will look up the amino acid
encoded by the codon. 

`print codon_table['atg']` will print out 'M'


### Important dictionary functions

#### The .get() function

allows to extract values from a dictionary but with a default value to be returned if the entry doesn't exist.

```python
codon_table['atg']
codon_table.get('atg'])

codon_table.get('atg'], 0)
```

#### The .keys() function

Extracts a list of the keys in a dictionary. Remember that there is no intrinsic order to the keys or values in a dictionary.


#### The .values() function

Extracts a list of the values in a dictionary. Remember that there is no intrinsic order to the keys or values in a dictionary .


#### Listing keys and values

In Python pseudocode is similar to an if statement:

```
for MyItem in MyCollection:
   do a command with MyItem
   do another command
resume operation of main commands
```

You can loop through a dictionary using:

```python
for person in PhoneNumber:
    print person, PhoneNumber[person]
```
```
John 463673
Mary 279943
```

#### Sorting

The power of dict comes now from sorting: We can sort the keys and then retrieve the associated values in a particular order.


```python
SortedKeys = sorted(PhoneNumber.keys())
```

We can then loop over the sorted keys using:

```python
for PersonSorted in SortedKeys:
    print SortedPerson, PhoneNumber[PersonSorted]
```



Alternativey, we can sort the values and then retreive the associated keys:

```python
SortedValues = sorted(PhoneNumber.values())
```






## Writing scripts

1. Write the simplest version of code
2. Refactor (remove duplication, reorganize the code)
3. If speed or memory are an issue: Optimize only at this point


Test with simple (simplified) made-up input file



## Sources

- [Software Carpentry v4](http://software-carpentry.org/v4/python/index.html)
- [Haddock & Dunn. Practical Computing for Biologists. Sinauer Associates 2011.](http://practicalcomputing.org))



## Learning more Python

[Scientific Programming with Python: 4-day course @ UZH](http://www.physik.uzh.ch/~python/python_2015-01/programme.php)




## Installation Instructions


For this course we will use Python v2.7. The latest version of Python (v3.0) has some real differences and is *not* compatible with versions < v3. Many people continue using v2.7 as the power of python comes from its packages and many of them are not yet available for Python v3.

If you will need Python v3 at a later stage you will learn it - the basic features we will be learning today are very similar.


Find out which version of Python your system has with 

```python
python --version
```

Your system should have a python version around 2.7 or 2.6. If the version number is 3.0 or more install a second version 
of python using Anaconda.

### Linux

Most Linux distributions come with python already installed. However, many packages are still missing:


- In Ubuntu Linux, to installing python and all the requirements run:
```
  $ sudo apt-get install python ipython ipython-notebook
  $ sudo apt-get install python-numpy python-scipy python-matplotlib python-sympy
  $ sudo apt-get install spyder
```
- In other Linux distributions, use your package manager to install python packages


If you have problems with installing some libraries consider [Anaconda Python v2.7](http://continuum.io/downloads)
Anaconda already comprises the most popular Python packages for science, math, engineering, data analysis.
With the Anaconda environment you also get [conda](http://conda.pydata.org/docs/) to install/manage different versions of libraries and modules. With Anaconda you can also have 2 or more different version of python simultaneously.

### Mac

Mac computers come preinstalled with the python interpreter. However, the installation of some libraries is painful (e.g. if you want to use IPython notebooks).

All the exercises today you can solve with your native environment. If you would like to use Python consider installing 
[Anaconda Python v2.7](http://continuum.io/downloads). Anaconda already comprises the most popular Python packages for science, math, engineering, data analysis.

### Windows

Download [Anaconda Python v2.7](http://continuum.io/downloads). Anaconda already comprises the most popular Python packages for science, math, engineering, data analysis.



### IPython Notebooks


[IPython Interactive Demo](http://www.nature.com/news/ipython-interactive-demo-7.21492?article=1.16261) from Nature


%matplotlib inline
 
import pylab as plt
import numpy as np
from IPython.html.widgets import interact
import math
 
def plot_sine(period=10):
    x = np.linspace(0,10,100)
    plt.plot(x,np.sin(x*2*math.pi/period))
 
interact(plot_sine,period=(2,20,.5))



a = 5
b = 36
print(a*b)



[A gallery of interesting IPython Notebooks](https://github.com/ipython/ipython/wiki/A-gallery-of-interesting-IPython-Notebooks#general-python-programming)





https://plot.ly/python/heatmaps-contours-and-2dhistograms-tutorial/
http://nbviewer.ipython.org/gist/jhemann/4569783
http://nbviewer.ipython.org/gist/wesm/4757075/PandasTour.ipynb
http://nbviewer.ipython.org/urls/bitbucket.org/hrojas/learn-pandas/raw/master/lessons/01%20-%20Lesson.ipynb
http://nbviewer.ipython.org/gist/msund/7ac1203ded66fe8134cc
http://nbviewer.ipython.org/urls/bitbucket.org/amjoconn/watpy-learning-to-code-with-python/raw/3441274a54c7ff6ff3e37285aafcbbd8cb4774f0/notebook/Learn%20to%20Code%20with%20Python.ipynb
http://nbviewer.ipython.org/github/jrjohansson/scientific-python-lectures/blob/master/Lecture-1-Introduction-to-Python-Programming.ipynb


### Differences python and R

- In python calculations / batch modification on a list do **not** work as in R, e.g. MyList = range(0,5); MyList * 2
To process the list element-by-element as in R, we need a `for` loop (or numpy).

- Be careful when name copying lists or dictionaries:



### Exercises

### 1. IDE

Open the script in Spyder
Run it
Which type has variable 'XXX'?
 
### 2. Functions

Write a function 

### 3. Libraries

Import module sys and list the functions (methods) that are provided by it:

Then in Spyder type 'os.', then a window open showing you all the available 

- Try to find out which function to use to get the current working directory
- Try to find out which function to use to create a new directory


os.getcwd()
os.mk



### 4. Dictionaries