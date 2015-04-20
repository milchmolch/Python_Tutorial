# URPP Tutorials
## URPP Evolution in Action

![URPP logo](Logo_URPP_Kreisganz_kl2.png)

Stefan Wyder


# Regular Expressions


check also http://www.dalkescientific.com/writings/NBN/

## Introduction

Regular expressions (aka regex or regexp or RE) is a tiny programming language to describe sets of strings, a sort of pattern.

A first example: count the number of fasta sequences in a file (fast & safe):
```
grep -c "^>" file.fa
```

Regular expressions often provide a safer (and faster) way of searching than a simple text search.


They can be used for several things:

- Powerful search and replace function
- Extract information
- Format checking


Here we practise regular expressions using python. We could also use the shell (grep/egrep, sed, awk), R, any other programming language and even many text editors and OpenOffice.

## Simple Patterns

The easiest regular expression is a literal string. For example, the regular expression `test` will match the string `test` exactly.


In python, we first have to load the built-in module `re`:  
```python
import re
```

The basic format is (query being a regular expression):
```python
Result = re.search(query, string)
```
  
```python  
MyString = "Arabidopsis thaliana"
re.sub("thaliana", "lyrata", MyString)
'Arabidopsis lyrata'
```

The same could have been achieved with the `replace()` function, as we did simple string replacement: `MyString.replace("thaliana", "lyrata")`.

  
Let's start with a simple example. We define a regular expression `MyRe` putting the letter r immediately before the opening quotation mark:
```python
MyRe = r"(\w)(\w+)"
```

The pattern `MyRe` captures 2 strings as indicated by the pair of brackets (1. the first letter, and 2. all the rest) counting from left to right.

Now we search our regular expression in the string `Arabidopsis thaliana`:
```python
MyString = "Arabidopsis thaliana"
MyResult = re.search(MyRe, MyString)
```
We could have done `MyResult = re.search(MyRe, "Arabidopsis thaliana")` directly which gives the same result.


```python
All matches together
MyResult.group(0)
'Arabidopsis'
```

Now we can get the first captured match (first pair of brackets)
```python
MyResult.group(1)
'A'
```

And the second captured match (second pair of brackets)
```python
The second captured match
MyResult.group(2)
'rabidopsis'
```


##Functions that use regular expressions

-------------------|----------------
Function | |    
re.sub(query, replacement, string) | Make *all* substitutions in a string    
re.search() | Detect the presence of a pattern
re.match() |  Like re.search but only if pattern matches the entire string
re.split() | Split a string according to a pattern


##Replacing text / Substitution

re.sub() is for replacing text. Its arguments are <pattern> (the regular expression), <repl(acement)> (the substitution pattern) and <string> (the string to work on).

The simplest regular expression pattern are just literal characters:
```python
MyString = "23rd May 2000"
re.sub("May", "July", MyString)
[1] "23rd July 2014"
```

sub() replaces all occurences. Hence here we remove any space
```python
re.sub(" ", "", " Hello World ")
'HelloWorld'
```

We can also replace a maximum number of occurences using  the `count` argument:
```python
re.sub(" ", "", " Hello World ", 1)
'Hello World '

re.sub(" ", "", " Hello World ", 2)
'HelloWorld '

re.sub(" ", "", " Hello World ", 3)
'HelloWorld'
```

Regular expressions have much more abilities. Here we use a regular expression to only keep the 3 digits after the comma.

```python
re.sub(r"(\d+\.\d{3})\d+", r"\1", "34.73322532")
'34.733'
```

or reformat a date:

```python
re.sub(r"(\d+)\w{2} (\w+) (\d+)", r"\2 \1 \3", "23rd May 2015")
'May 23 2015'
```
Often the same thing can be done using string functions like `split()` and pasting together subsets.



##Escape special characters ‘$ * + . ? [ ] ^ { } | ( ) \

If we look for a meta-character ‘$ * + . ? [ ] ^ { } | ( ) \’, we precede them with a backslash. As they have a double meaning we need '\' to interpret them as ordinary characters.

In Arabidopsis, isoforms of a gene X are called like X.1, X.2, X.3. Sometimes we need a list of genes which we can achieve using a regular expression. It works with X.11 or X.100.

```python
re.sub(r"([^\.]+)\.\d+", r"\1", "AT5G11100.3")
"AT5G11100"
```

With [] we define our own character class. The ^ in square brackets means that it will match any character except the one written. We have to escape the dot `\` otherwise it means any character. Thus [^\.] means any character but a dot. This way is a safe way to capture everything until the first dot.


There are often many different ways to write a working regular expression. These give the same result:

```python
re.sub(r"([^\.]+)\.[0123456789]+", r"\1", "AT5G11100.3")
"AT5G11100"

re.sub(r"([A-z0-9]+)\.0-9]+", r"\1", "AT5G11100.3")
"AT5G11100"
```


##Finding text / Format checking

re.search() tells you whether a regular expression matches the input string. If it matches it returns `a match object` and if it does not match `None`.

```python
if re.search(r"21", "21 Aug 2014"):
	print "found"
else: 
	print "not found"   
```

Save the lines as `Try_ReSearch.py` and change the search pattern.

```
python Try_ReSearch.py
found
```

If it matches the resulting object contains many attributes (private methods not shown).

```python
dir(re.search(r"21", "21 Aug 2014"))
 'end',
 'endpos',
 'expand',
 'group',
 'groupdict',
 'groups',
 'lastgroup',
 'lastindex',
 'pos',
 're',
 'regs',
 'span',
 'start',
 'string'
```

##Splitting

```python
>> s = "a 1 and 2 and 3 and 4"
>> a = re.split("\d", s) # every number is a delimiter
>> a
['a ', ' and ', ' and ', ' and ', '']
```

## The power of Regexps

Regexps can be used to remove HTML tags to get a simple text file:

```python
\# This string contains HTML.
>> v = """<p id=x>Sometimes, <b>simpler</b> is better, but <i>not</i> always.</p>"""

\# Replace HTML tags with an empty string.
>> result = re.sub("<.*?>", "", v)
>> print(result)
'Sometimes, simpler is better, but not always.'
```

##Nota bene

- case-sensitive! (but can be changed with IGNORECASE flag)


## Application in Bioinformatics

Regular expressions are used a lot for data mangling (format conversion). Furthermore, regular expressions are often used for parsing, e.g. if you want to extract information from a BLAST report (e.g. the Sequence ID and the the E-value). Regular expressions can also be used to identify Sequence motifs (e.g. to search for a motif with 3 basic amino acids across 5 positions).

- protein domains
- DNA transcription factor binding motifs
- restriction enzyme cut sites
- degenerate PCR primer sites
- runs of mononucleotides
- read mapping locations

### Final Comment


Trial and error: Sometimes regular expressions do not behave as expected. In case of difficulties try to start simple, test parts of the regular expression and combine them once the subparts work. Often it also helps to do two rounds of replacements. Another level of complication is that there are 2 different types of regular expression, e.g. in R the default are 'extended regular expressions' (perl=FALSE) but there are also Perl-like regular expressions (perl=TRUE) that look different from the default type. It is easy to confuse them. Last, there unfortunately also differences between languages so that sometimes we have to find the correct version by trial and error.


##Exercises

1. Modify some of the input strings and patterns/regular expression and check whether they produce the expected results. You can e.g. add additional text to the input string or remove parts of the regexp.
2. Try out 2 online tools (url given below). Choose the one that appeals most to you.
3. Write a regular expression that checks whether a string has the valid format like "21 Aug 2014".
4. Develop a regexp to remove the noninformative parts of these 2 sample names:  
  X20120401_Wyderrun31_1367.05.1_05.RCC
X20120401_Wyderrun31_1482.05.1_08.RCC


# Sources

- [Regular Expression HOWTO (Python doc)](https://docs.python.org/2/howto/regex.html#regex-howto)
- [Software Carpentry v4](http://software-carpentry.org/v4/regexp/index.html)
- [Haddock & Dunn. Practical Computing for Biologists. Sinauer Associates 2011.](http://practicalcomputing.org))
- [Python for Biologists](http://pythonforbiologists.com/index.php/introduction-to-python-for-biologists/regular-expressions/)

# Links

**Online tools to try regular expressions**  
- [http://regex101.com/](http://regex101.com/)   
- [http://www.regexr.com/](http://www.regexr.com/)   
- [https://www.debuggex.com/](https://www.debuggex.com/)  

**Cheat Sheets**  
- [CheatSheet from Practical Computing Biologists](http://practicalcomputing.org/files/PCfB_Appendices.pdf)

**Regular Expression in other languages**  
- [in R](http://en.wikibooks.org/wiki/R_Programming/Text_Processing#Functions_which_use_regular_expressions_in_R)  
- [using sed](http://www.grymoire.com/Unix/Sed.html#uh-4)

# Solutions 

3. re.search(r"\d{2} \w{3} \d{4}" , "21 Aug 2014")
