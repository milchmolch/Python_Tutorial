# Biopython

## Solutions

## Sequence Input/Output


**1.** Count how many sequences are less than 100 amino acids long.

```python
from Bio import SeqIO

input_filename = "NC_000913.faa"
count = 0
total = 0
for record in SeqIO.parse(input_filename, "fasta"):
    total = total + 1
    if 100 <= len(record):
        count = count + 1
print(str(count) + " records shorter than 100aa out of " + str(total))
```
```
3720 records shorter than 100aa out of 4140
```


**2.** Write a script which only prints out 1 sequence provided as a command line argument.
Tip: use sys.argv to get the sequence name.

```python
from Bio import SeqIO
from sys import argv

wanted_seq = argv[1]
input_filename = "NC_000913.faa"

for record in SeqIO.parse(input_filename, "fasta"):
    if record.id == wanted_seq:
                print ">" + record.description  #record.id does only contain name until first whitespace
                print record.seq
```

```
python print_wanted_seq.py "gi|16128004|ref|NP_414551.1|"
>gi|16128004|ref|NP_414551.1|gi|16128004|ref|NP_414551.1| succinate-acetate transporter [Escherichia coli str. K-12 substr. MG1655]
MGNTKLANPAPLGLMGFGMTTILLNLHNVGYFALDGIILAMGIFYGGIAQIFAGLLEYKKGNTFGLTAFTSYGSFWLTLVAILLMPKLGLTDAPNAQFLGVYLGLWGVFTLFMFFGTLKGARVLQFVFFSLTVLFALLAIGNIAGNAAIIHFAGWIGLICGASAIYLAMGEVLNEQFGRTVLPIGESH
```


**3.** (Optional) Plot a histogram of the sequence length distribution.
For plotting you can use either Python's pylab module or you save the result into a file and use R.

```python
from Bio import SeqIO
import matplotlib.pyplot as plt

filename = "NC_000913.faa"
data = []

for record in SeqIO.parse(filename, "fasta"):
    data.append(len(record.seq))
plt.hist(data, bins=50)
plt.show()
```

## Writing Sequences Files in Biopython


Guided by the filter_length.py script, write a new script starting as follows which writes out the potato proteins on this list:
```python
from Bio import SeqIO
wanted_ids = ["PGSC0003DMP400019313", "PGSC0003DMP400020381", "PGSC0003DMP400020972"]
input_filename = "PGSC_DM_v3.4_pep_representative.fasta"
output_filename = "wanted_potato_proteins.fasta"
count = 0
total = 0

output_handle = open(output_filename, "w")
for record in SeqIO.parse(input_filename, "fasta"):
    total = total + 1
    if record.id in wanted_ids:
        count = count + 1
        SeqIO.write(record, output_handle, "fasta")
output_handle.close()
print(str(count) + " records selected out of " + str(total))
```

**Advanced Exercise:** Modify this to read the list of wanted identifiers from a plain text input file (one identifier per line).


## BLAST

Find out the attributes of alignments and hsps.
```
dir(blast_records.alignments[0])
['__class__',
 '__delattr__',
 '__dict__',
 '__doc__',
 '__format__',
 '__getattribute__',
 '__hash__',
 '__init__',
 '__module__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 'accession',
 'hit_def',
 'hit_id',
 'hsps',
 'length',
 'title']


dir(hsp)
['__class__',
 '__delattr__',
 '__dict__',
 '__doc__',
 '__format__',
 '__getattribute__',
 '__hash__',
 '__init__',
 '__module__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 'align_length',
 'bits',
 'expect',
 'frame',
 'gaps',
 'identities',
 'match',
 'num_alignments',
 'positives',
 'query',
 'query_end',
 'query_start',
 'sbjct',
 'sbjct_end',
 'sbjct_start',
 'score',
 'strand']
```