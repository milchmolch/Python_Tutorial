# Introduction to Python

# Solutions


## 6. Functions


1. Convert your code from Heidi Lischers Exercise 1a into a function that returns the sum of 
  the integers 1 + 2 + 3 + 4 .... + n 

```python
def SumOfSeries(n):
   sum = 0
   for number in range(1, n + 1):
      sum += number
   return sum
```

2. Calculate the sum for n = 10, n = 100 and n = 1000.

```python
print SumOfSeries(10)
print SumOfSeries(100)
print SumOfSeries(1000) 
```

3. Make your script beautiful, choose expressive variable names, comment it.

```python
\# Calculates the sum of series 1+2+3+.. +n
def SumOfSeries(n):
   sum = 0
   for number in range(1, n + 1):
      sum += number
   return sum

print SumOfSeries(10)
print SumOfSeries(100)
print SumOfSeries(1000) 
```



## 7. Libraries

write another script that imports sign.py (`import sign`) and uses the sign function for printing the sign for [-8, -1, 0, 1, 6].

**Sign.py**
```python
\\# Sign function
def sign(num):
    if num > 0:
       return 1
    elif num == 0:
       return 0
    else:
       return -1

```

**UseSign.py**
```python
import sign

Numbers = [-8, -1, 0, 1, 6]

for input in Numbers:
    print input, sign.sign(input)
``` 
  

```
python UseSign.py
```
   

## 8. Dictionaries


### Exercise: Codon Table

1. Write a script that prints out the protein sequence encoded by the DNA 'GTGCGGCCACCT'. Print out the position,the codon and the growing amino acid sequence


```
\\# -*- coding: utf-8 -*-
"""
Created on Wed Mar 18 18:56:43 2015

@author: swyder
"""

codon_table = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M', 'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K', 'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L', 'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q', 'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V', 'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E', 'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S', 'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'_', 'TAG':'_', 'TGC':'C', 'TGT':'C', 'TGA':'_', 'TGG':'W'}


dna = 'GTGCGGCCACCT'

aaseq = ''

for pos in range(0,len(dna)-1,3):
    codon = dna[pos:pos+3]
    aaseq += codon_table[codon]
    print pos, codon, aaseq
```
  
2. Convert it into a function (if you did not write it as a function)

```python
def TranslateDNA(dna):

   codon_table = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M', 'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K', 'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L', 'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q', 'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V', 'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E', 'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S', 'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'_', 'TAG':'_', 'TGC':'C', 'TGT':'C', 'TGA':'_', 'TGG':'W'}

   aaSeq = ''
   for pos in range(0,len(dna)-1,3):
      codon = dna[pos:pos+3]
      aaSeq += codon_table[codon]
   return aaSeq
```

3. Make your script safer by also allowing both upper and lower case letters, check that only valid letters occur, the length of the DNA sequence is a multiple of 3...
:bulb:
Use the Cheat Sheet to find out which string function to use for converting letters to  

```python
def SafeTranslateDNA(dna):
   "Translates DNA into amino acid and checks that input sequence is a valid DNA sequence"
   
   from sys import exit
   
   codon_table = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M', 'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K', 'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L', 'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q', 'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V', 'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E', 'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S', 'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'_', 'TAG':'_', 'TGC':'C', 'TGT':'C', 'TGA':'_', 'TGG':'W'}

   dnaUpper = dna.upper()
   if len(dnaUpper) % 3 != 0 :
       exit('DNA sequence length is not a multiple of 3')
   for letter in dnaUpper:
      if letter not in ['G','A','T','C']:
          exit('Invalid letter(s) other than GATC present in sequence') # sys.exit stops execution
   aaSeq = ''
   for pos in range(0,len(dnaUpper)-1,3):
      codon = dnaUpper[pos:pos+3]
      aaSeq += codon_table[codon]
   return aaSeq
```

4. Make a count table of how many codons encode the same amino acid

```python
\# Prints out number of different codons encoding the same amino acid 

codon_table = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M', 'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K', 'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L', 'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q', 'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V', 'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E', 'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S', 'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'_', 'TAG':'_', 'TGC':'C', 'TGT':'C', 'TGA':'_', 'TGG':'W'}

codon_count = {}

for codon in codon_table:
    aa = codon_table[codon]
    if aa in codon_count:     # If we have seen this AA before...
        codon_count[aa] = codon_count[aa] + 1    # add one to its count
    else:                                      # If it is the first time we see that name
        codon_count[aa] = 1

for ele in codon_count:
    print ele, codon_count[ele]
```

5. Print out a sorted count table

```python
\# Prints out number of different codons encoding the same amino acid 

codon_table = {
    'ATA':'I', 'ATC':'I', 'ATT':'I', 'ATG':'M', 'ACA':'T', 'ACC':'T', 'ACG':'T', 'ACT':'T',
    'AAC':'N', 'AAT':'N', 'AAA':'K', 'AAG':'K', 'AGC':'S', 'AGT':'S', 'AGA':'R', 'AGG':'R',
    'CTA':'L', 'CTC':'L', 'CTG':'L', 'CTT':'L', 'CCA':'P', 'CCC':'P', 'CCG':'P', 'CCT':'P',
    'CAC':'H', 'CAT':'H', 'CAA':'Q', 'CAG':'Q', 'CGA':'R', 'CGC':'R', 'CGG':'R', 'CGT':'R',
    'GTA':'V', 'GTC':'V', 'GTG':'V', 'GTT':'V', 'GCA':'A', 'GCC':'A', 'GCG':'A', 'GCT':'A',
    'GAC':'D', 'GAT':'D', 'GAA':'E', 'GAG':'E', 'GGA':'G', 'GGC':'G', 'GGG':'G', 'GGT':'G',
    'TCA':'S', 'TCC':'S', 'TCG':'S', 'TCT':'S', 'TTC':'F', 'TTT':'F', 'TTA':'L', 'TTG':'L',
    'TAC':'Y', 'TAT':'Y', 'TAA':'_', 'TAG':'_', 'TGC':'C', 'TGT':'C', 'TGA':'_', 'TGG':'W'}

codon_count = {}

for codon in codon_table:
    aa = codon_table[codon]
    if aa in codon_count:     # If we have seen this AA before...
        codon_count[aa] = codon_count[aa] + 1    # add one to its count
    else:                                      # If it is the first time we see that name
        codon_count[aa] = 1

sorted_codon_count = sorted(codon_count.items(), key=lambda x: x[1], reverse=True)

for ele in sorted_codon_count:
    print ele[0], ele[1]


```

6. Write a backtranslator (protein -> DNA)
:bulb:
Make a second dictionary with reversed key-values

```python

```
  

## Exercise: Counting insects

**more SeenInsects.txt** 
Ladybug
Mealworm
Fruitfly
Bumblebee
Hoverfly
Fruitfly
  
  
```
python CountInsects.py SeenInsects.txt
Ladybug 1
Hoverfly 1
Mealworm 1
Bumblebee 1
Fruitfly 2
```