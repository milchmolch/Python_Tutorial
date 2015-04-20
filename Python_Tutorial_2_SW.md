# URPP Tutorials
## URPP Evolution in Action

![URPP logo](Logo_URPP_Kreisganz_kl2.png)

Stefan Wyder



# Introduction to Python


# BioPython


Chapters organized like in [BioPython tutorial](http://biopython.org/DIST/docs/tutorial/Tutorial.html)

**Heidi**  
1  Introduction  
2  What can you do with Biopython?  
3  Sequence objects  
4  Sequence annotation objects  

**Stefan**  
5  Sequence Input/Output  
6  Multiple Sequence Alignment objects  
7  BLAST  
9  Accessing NCBI's Entrez databases  
(Genbank, Medline, GEO, Unigene)  
10  Swiss-Prot and ExPASy  
16  Graphics including GenomeDiagram


## Download files
 
Create a new directory and download the example files 
Use wget (or curl on Mac)

wget ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/Escherichia_coli_K_12_substr__MG1655_uid57779/NC_000913.gbk wget ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/Escherichia_coli_K_12_substr__MG1655_uid57779/NC_000913.fna wget ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/Escherichia_coli_K_12_substr__MG1655_uid57779/NC_000913.ffn wget ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/Escherichia_coli_K_12_substr__MG1655_uid57779/NC_000913.faa http://potato.plantbiology.msu.edu/data/PGSC_DM_v3.4_pep_representative.fasta.zip


## 6.5 Sequence Input/Output


The following chapter is a shortened (and a bit modified) version of [Peter Cock's workshop](
https://github.com/peterjc/biopython_workshop) on Biopython.
  
  
  
Dealing with assorted sequence file formats is one of the strengths of Biopython. The primary module we'll be using is `Bio.SeqIO`, which is short for sequence input/output (following the naming convention set by BioPerl's `SeqIO` module).

For these examples we're going to use files for the famous bacteria Escherichia coli K12 (from the NCBI FTP server).



### Counting records

We'll start by looking at the protein sequence in the FASTA amino acid file, NC_000913.faa. First take a quick peek using `head` to look at the start of the file (in the shell):

```
$ head NC_000913.faa
>gi|16127995|ref|NP_414542.1| thr operon leader peptide [Escherichia coli str. K-12 substr. MG1655]
MKRISTTITTTITITTGNGAG
>gi|16127996|ref|NP_414543.1| fused aspartokinase I and homoserine dehydrogenase I [Escherichia coli str. K-12 substr. MG1655]
MRVLKFGGTSVANAERFLRVADILESNARQGQVATVLSAPAKITNHLVAMIEKTISGQDALPNISDAERI
FAELLTGLAAAQPGFPLAQLKTFVDQEFAQIKHVLHGISLLGQCPDSINAALICRGEKMSIAIMAGVLEA
RGHNVTVIDPVEKLLAVGHYLESTVDIAESTRRIAASRIPADHMVLMAGFTAGNEKGELVVLGRNGSDYS
AAVLAACLRADCCEIWTDVDGVYTCDPRQVPDARLLKSMSYQEAMELSYFGAKVLHPRTITPIAQFQIPC
LIKNTGNPQAPGTLIGASRDEDELPVKGISNLNNMAMFSVSGPGMKGMVGMAARVFAAMSRARISVVLIT
QSSSEYSISFCVPQSDCVRAERAMQEEFYLELKEGLLEPLAVTERLAIISVVGDGMRTLRGISAKFFAAL
ARANINIVAIAQGSSERSISVVVNNDDATTGVRVTHQMLFNTDQVIEVFVIGVGGVGGALLEQLKRQQSW
```

Now let's count the records with Biopython using the `SeqIO.parse` function. Paste the following lines into a file `count_fasta.py`:

```python
from Bio import SeqIO
filename = "NC_000913.faa"
count = 0
for record in SeqIO.parse(filename, "fasta"):
    count = count + 1
print("There were " + str(count) + " records in file " + filename)
```

Have a look at the [SeqIO entry](http://biopython.org/wiki/SeqIO) in the Biopython wiki. As always you can also get help on the `SeqIO.parse` function using `help(SeqIO.parse)` or, in IPython, using `SeqIO.parse?`

### Looking at the records

In the above example, we used a for loop to count the records in a FASTA file, but didn't actually look at the information in the records. The `SeqIO.parse` function was creating [SeqRecord objects](http://biopython.org/wiki/SeqRecord). Biopython's `SeqRecord` objects are a container holding the sequence, and any annotation about it - most importantly the identifier.

For FASTA files, the record identifier is taken to be the first word on the > line - anything after a space is not part of the identifier.

This simple example prints out the record identifers and their lengths:

```python
from Bio import SeqIO
filename = "NC_000913.faa"
for record in SeqIO.parse(filename, "fasta"):
    print("Record " + record.id + ", length " + str(len(record.seq)))
```
Notice that given a `SeqRecord` object we access the identifer as `record.id` and the sequence object as `record.seq`. As a shortcut, `len(record)` gives the sequence length, `len(record.seq)`.


If you save that as `record_lengths.py` and run it you'll get over four thousand lines of output:

```
$ python record_lengths.py
Record gi|16127995|ref|NP_414542.1|, length 21
Record gi|16127996|ref|NP_414543.1|, length 820
Record gi|16127997|ref|NP_414544.1|, length 310
Record gi|16127998|ref|NP_414545.1|, length 428
...
Record gi|16132219|ref|NP_418819.1|, length 46
Record gi|16132220|ref|NP_418820.1|, length 228
```

#### Exercises
1. Count how many sequences are less than 100 amino acids long.
2. Write a script which only prints out 1 sequence provided as a command line argument.  
Extracting a sequence from a large fasta file is a frequent task. 
Tip: use `sys.argv` to get the sequence name.
3. (Optional) Plot a histogram of the sequence length distribution.  
For plotting you can use either Python's [`pylab` module](http://matplotlib.org/examples/index.html) or you save the result into a file and use R. 


### Looking at the sequence 

The record identifiers are very important, but more important still is the sequence itself. In the `SeqRecord` objects the identifiers are stored as standard Python strings (e.g. `.id`). For the sequence, Biopython uses a string-like `Seq` object, accessed as `.seq`.

In many ways the Seq objects act like Python strings, you can print them, take their length using the `len()` function, and slice them with square brackets to get a sub-sequence or a single letter.


#### Checking proteins start with methionine

In the next example we'll check all the protein sequences start with a methionine (represented as the letter "M" in the standard IUPAC single letter amino acid code), and count how many records fail this. Let's create a script called `check_start_met.py`:

```python
from Bio import SeqIO
filename = "NC_000913.faa"
bad = 0
for record in SeqIO.parse(filename, "fasta"):
    if not record.seq.startswith("M"):
        bad = bad + 1
        print(record.id + " starts " + record.seq[0])
print("Found " + str(bad) + " records in " + filename + " which did not start with M")
```


If you run that, you should find this E. coli protein set all had leading methionines:

```
$ python check_start_met.py
Found 0 records in NC_000913.faa which did not start with M
```
Good - no strange proteins. This genome has been completely sequenced and a lot of work has been done on the annotation, so it is a 'Gold Standard'. Now try this on the potato protein file `PGSC_DM_v3.4_pep_representative.fasta`:

```
$ python check_start_met.py
PGSC0003DMP400032467 starts T
PGSC0003DMP400011427 starts Q
PGSC0003DMP400068739 starts E
...
PGSC0003DMP400011481 starts Y

Found 208 records in PGSC_DM_v3.4_pep_representative.fasta which did not start with M
```

### Different File Formats

So far we've only been using FASTA format files, which is why when we've called `SeqIO.parse()` the second argument has been `"fasta"`. The Biopython `SeqIO` module supports quite a few other important sequence file formats (see the table on the [SeqIO wiki page](http://biopython.org/wiki/SeqIO).

If you work with finished genomes, you'll often see nicely annotated files in the EMBL or GenBank format. Let's try this with the E. coli K12 GenBank file, NC_000913.gbk, based on the previous example:

```
>>> from Bio import SeqIO
>>> fasta_record = SeqIO.read("NC_000913.fna", "fasta")
>>> print(fasta_record.id + " length " + str(len(fasta_record)))
gi|556503834|ref|NC_000913.3| length 4641652
>>> genbank_record = SeqIO.read("NC_000913.gbk", "genbank")
>>> print(genbank_record.id + " length " + str(len(genbank_record)))
NC_000913.3 length 4641652
```

All we needed to change was the file format argument to the `SeqIO.read()` function - and we could load a GenBank file instead. You'll notice the GenBank version was given a shorter identifier, and took longer to load. The reason is that there is a lot more information present - most importantly lots of features (where each gene is and so on). We'll return to this in a later section, working with sequence features.

### Writing Sequences Files in Biopython

Now we'll focus on writing sequence files using the function `SeqIO.write()`.

#### Converting a sequence file

Recall we looked at the E. coli K12 chromosome as a FASTA file `NC_000913.fna` and as a GenBank file `NC_000913.gbk`. Suppose we only had the GenBank file, and wanted to turn it into a FASTA file?

Biopython's `SeqIO` module can read and write lots of sequence file formats:

```python
from Bio import SeqIO
input_filename = "NC_000913.gbk"
output_filename = "NC_000913_converted.fasta"
records_iterator = SeqIO.parse(input_filename, "gb")
count = SeqIO.write(records_iterator, output_filename, "fasta")
print(str(count) + " records converted")
```
Previously we'd always used the results from `SeqIO.parse()` in a for loop - but here the for loop happens inside the `SeqIO.write()` function.

Also have a look at the output file:
```
$ head NC_000913_converted.fasta
>NC_000913.3 Escherichia coli str. K-12 substr. MG1655, complete genome.
AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTC
TGATAGCAGCTTCTGAACTGGTTACCTGCCGTGAGTAAATTAAAATTTTATTGACTTAGG
TCACTAAATACTTTAACCAATATAGGCATAGCGCACAGACAGATAAAAATTACAGAGTAC
ACAACATCCATGAAACGCATTAGCACCACCATTACCACCACCATCACCATTACCACAGGT
AACGGTGCGGGCTGACGCGTACAGGAAACACAGAAAAAAGCCCGCACCTGACAGTGCGGG
CTTTTTTTTTCGACCAAAGGTAACGAGGTAACAACCATGCGAGTGTTGAAGTTCGGCGGT
ACATCAGTGGCAAATGCAGAACGTTTTCTGCGTGTTGCCGATATTCTGGAAAGCAATGCC
AGGCAGGGGCAGGTGGCCACCGTCCTCTCTGCCCCCGCCAAAATCACCAACCACCTGGTG
GCGATGATTGAAAAAACCATTAGCGGCCAGGATGCTTTACCCAATATCAGCGATGCCGAA
```

**Warning:** The output will over-write any pre-existing file of the same name.


There is also a handy helper function to convert a file:
```
>>> from Bio import SeqIO
>>> help(SeqIO.convert)
```



The `SeqIO.convert()` function is effectively a shortcut combining `SeqIO.parse()` for input and `SeqIO.write()` for output.

`The SeqIO.write()` function is happy to be given multiple records like this, or simply as a list of `SeqRecord objects`. You can also give it just one record.

We'll be doing this in the next example, where we call `SeqIO.write()` several times in order to build up a mult-record output file.

#### Filtering a sequence file

The simplest solution is to open and close the file explicitly, using a file handle. The `SeqIO` functions are happy to work with either filenames (strings) or file handles, and this is a case where the more low-level handle is useful.

Here's a working version of the script, save this as `filter_length.py`:
```python
from Bio import SeqIO
input_filename = "NC_000913.faa"
output_filename = "NC_000913_long_only.faa"
count = 0
total = 0
output_handle = open(output_filename, "w")
for record in SeqIO.parse(input_filename, "fasta"):
    total = total + 1
    if 100 <= len(record):
        count = count + 1
        SeqIO.write(record, output_handle, "fasta")
output_handle.close()
print(str(count) + " records selected out of " + str(total))
```
We get the expected output:
```
$ python filter_length.py
3720 records selected out of 4141
$ grep -c "^>" NC_000913_long_only.faa
3720
Yay!
```

#### Filtering by record name

A very common task is pulling out particular sequences from a large sequence file. Membership testing with Python lists (or sets) is one neat way to do this. Recap:

```
>>> wanted_ids = ["PGSC0003DMP400019313", "PGSC0003DMP400020381", "PGSC0003DMP400020972"]
>>> "PGSC0003DMP400067339" in wanted_ids
False
>>> "PGSC0003DMP400020972" in wanted_ids
True
```
**Exercise**   
Guided by the `filter_length.py` script, write a new script starting as follows which writes out the potato proteins on this list:

```python
from Bio import SeqIO
wanted_ids = ["PGSC0003DMP400019313", "PGSC0003DMP400020381", "PGSC0003DMP400020972"]
input_filename = "PGSC_DM_v3.4_pep_representative.fasta"
output_filename = "wanted_potato_proteins.fasta"
count = 0
total = 0
output_handle = open(output_filename, "w")
\# ...
\# Your code here
\# ...
output_handle.close()
print(str(count) + " records selected out of " + str(total))
```

The sample solution is called `filter_wanted_ids.py`, and the output should be:

```
$ python filter_wanted_id.py
3 records selected out of 39031
```

**Advanced Exercise:** Modify this to read the list of wanted identifiers from a plain text input file (one identifier per line).

**Discussion:** What happens if a wanted identifier is not in the input file? If some idetifiers contain trailing whitespaces? What happens if an identifer appears twice? What order is the output file?

When we write a program we also have to think what could go wrong. Good programs print out a warnings. 

**Exercise:**  Check what [additional methods](http://biopython.org/DIST/docs/_api_159/Bio.SeqUtils-module.html) are available for `Bio.Seq` objects and write a program which uses at least one.

### Working with Sequence Features

Read [this chapter](https://github.com/peterjc/biopython_workshop/blob/master/using_seqfeatures/README.rst) from Peter Cook's workshop.
  
  
There is much functionality of `SeqIO`, e.g. Biopython's `SeqIO.index(…)` function which lets us treat a sequence file like a Python dictionary. You can learn about them in the [Biopython Tutorial](http://biopython.org/DIST/docs/tutorial/Tutorial.html).


## 6.6. Multiple Alignments

`Bio.AlignIO` deals with files containing one or more sequence alignments represented as Alignment objects.

If you are interested in Multiple Alignments you can have a look at Peter's [workshop](https://github.com/peterjc/biopython_workshop/blob/master/reading_writing_alignments/README.rst)


## 6.7 Blast


BLAST can be run with BioPython either from using the command line tools -- provided they are installed -- or through the web. In this section, we use the qblast from the `Bio.Blast.NCBIWWW` module to call the online version of BLAST. Note that the results would be the same if we were to use the command line tools. As pointed out from the qblast documentation, there are three required arguments:

The *first argument* is the blast program to use for the search, as a lower case string. The options and descriptions of the programs are available at http://www.ncbi.nlm.nih.gov/BLAST/blast_program.shtml. Currently qblast only works with blastn, blastp, blastx, tblast and tblastx.
The *second argument* specifies the database in which to perform the search. Again, the options for this are available on the NCBI web pages at http://www.ncbi.nlm.nih.gov/BLAST/blast_databases.shtml.
The *third argument* is a string containing your query sequence. This can either be the sequence itself, the sequence in fasta format, or an identifier like a GI number.



```python
from Bio.Blast import NCBIWWW, NCBIXML
seq = "ataccaggctgaggcccattaatgatgcaatttgctgggcttctctattttctccgtgcttccatcctcttctccgtcggcggggagaagtgaaatgccgtggagatgggcggcggcggcggcgacggcggcgacgagaaagctcaccgggatctctcagtcgcgagtttcagtagcctttaccggccgtcttctctaccgctcgttcggaagcgactccagtgaaagccgcaagaggtcactgccacggggggtcgtatcgatcggggccatcagccttgctggaggtctcgtgctcagcgccgtcaacgacctcgccatcttcaatggatgcacaacgaaggcaattgagcatgctgctgacaaccctgctgttgtggaagcaattggagtgcctatagtcagaggaccgtggtatgatgcttctcttgaggtgggccatcgacggcggtctgtgtcatgcacattccctgtatctgggccacatgggtcaggatttctccagattaaggcaacccgagatggagaggatggtctgctttcgtttctgcggcatcacgactggaagatcctattgctggaggctcatcttgaagcaccatcagatgatgaggaccagagaaagctggttaaggtgaatcttgcaagcagtggccgtggggaagatggggatccagagagtggttaatcttttgtactgaattccatggtgagtggaagatcgtgtcatctgaatggactccaaatattaaatgacatggagatctagggaagcaaaaaaaaaaaaaaaa"
E_VALUE_THRESH = 0.01
s_len = 100

result_handle = NCBIWWW.qblast("blastn", "nt", seq) 
blast_records = NCBIXML.read(result_handle) 

for alignment in blast_records.alignments[:5]:
    for hsp in alignment.hsps: 
        if hsp.expect < E_VALUE_THRESH: 
            print "****Alignment****"
            print "sequence:", alignment.title 
            print "length:", alignment.length 
            print "gaps:", hsp.gaps
            print "e value:", hsp.expect 
            print hsp.query[0:s_len] + "..." 
            print hsp.match[0:s_len] + "..." 
            print hsp.sbjct[0:s_len] + "..."
```

The (shortened) output looks like:
```
python Remote_blast.py
****Alignment****
sequence: gi|226505045|ref|NM_001150016.1| Zea mays uncharacterized LOC100276165 (LOC100276165), mRNA >gi|195621403|gb|EU960414.1| Zea mays clone 224719 hypothetical protein mRNA, complete cds
length: 791
gaps: 0
e value: 0.0
ATACCAGGCTGAGGCCCATTAATGATGCAATTTGCTGGGCTTCTCTATTTTCTCCGTGCTTCCATCCTCTTCTCCGTCGGCGGGGAGAAGTGAAATGCCG...
||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||...
ATACCAGGCTGAGGCCCATTAATGATGCAATTTGCTGGGCTTCTCTATTTTCTCCGTGCTTCCATCCTCTTCTCCGTCGGCGGGGAGAAGTGAAATGCCG...
…
…
****Alignment****
sequence: gi|149391396|gb|EF576128.1| Oryza sativa (indica cultivar-group) clone V-E12 unknown mRNA
length: 752
gaps: 0
e value: 1.77484e-78
AAGAGGTCACTGCCACGGGGGGTCGTATCGATCGGGGCCATCAGCCTTGCTGGAGGTCTCGTGCTCAGCGCCGTCAACGACCTCGCCATCTTCAATGGAT...
|||||||||| |   ||| || |||| ||||| ||||  |||||| | ||||| ||  ||| ||| |||||| |||||||||| || || ||| ||||||...
AAGAGGTCACGGATCCGGAGGATCGTGTCGATTGGGGTTATCAGCATCGCTGGCGGCGTCGCGCTTAGCGCCCTCAACGACCTTGCTATATTCCATGGAT...
```


The default output format for qblast is XML.

Note that the default settings on the NCBI BLAST website are not quite the same as the defaults on QBLAST. If you get different results, you’ll need to check the parameters (e.g. the expectation value threshold and the gap values).

**Exercise:** Find out the attributes of alignments and hsps.

You can learn more about running BLAST and parsing its results in the [Biopython Tutorial](http://biopython.org/DIST/docs/tutorial/Tutorial.html).


### Running BLAST locally

Web blast queries are relatively slow.  Local BLAST allows you to use a very large query, or to search a custom database. Installing ncbi-blast is straight-forward on Linux, e.g. with Ubuntu:

```
sudo apt-get install ncbi-blast+
```
installs blastn, blastp, blastx,… in your path.

Alternatively, Blast binaries can be downloaded from [here](http://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download).


## 6.8. NCBI's Entrez databases

### Overview databases

![image](NCBI_databases.png)

Entrez (http://www.ncbi.nlm.nih.gov/Entrez) is a data retrieval system that provides users access to NCBI’s databases such as PubMed, GenBank, GEO, and many others. You can access Entrez from a web browser to manually enter queries, or you can use Biopython’s `Bio.Entrez` module for programmatic access to Entrez. The latter allows you for example to search PubMed or download GenBank records from within a Python script.

The `Bio.Entrez` module makes use of the Entrez Programming Utilities (also known as EUtils), consisting of eight tools that are described in detail on NCBI’s page at http://www.ncbi.nlm.nih.gov/entrez/utils/. Each of these tools corresponds to one Python function in the Bio.Entrez module, as described in the sections below. This module makes sure that the correct URL is used for the queries, and that not more than one request is made every three seconds, as required by NCBI.

You can learn more in the [Biopython Tutorial](http://biopython.org/DIST/docs/tutorial/Tutorial.html).

## 6.9. Graphics

The `Bio.Graphics` module can plot chromosome and karyogram plots with annotations.  It can create PDF,EPS SVG output files as well as bitmap images (including JPEG, PNG, GIF, BMP and PICT formats).

**Examples**  

- Genome organization of a Bacterium [(publication)](doi:10.1007/s10482-009-9316-9)

<img src="VanDerAuweraFig1.gif" style="width: 200px;"/>
<img src="VanDerAuweraFig2.gif" style="width: 200px;"/>

- Locations of tRNA genes in the Arabidopsis*genome.

![image](tRNA_chrom.png)

You can learn more in the [Biopython Tutorial](http://biopython.org/DIST/docs/tutorial/Tutorial.html).


# Sources

- [Biopython Tutorial](http://biopython.org/DIST/docs/tutorial/Tutorial.html)
- [Peter Cock's workshop on Biopython](https://github.com/peterjc/biopython_workshop)

# Links

- Biopython [wiki](http://biopython.org/wiki/), e.g. page on [SeqIO](http://biopython.org/wiki/SeqIO)

- [Short Tutorial (interactive notebook)](http://nbviewer.ipython.org/github/gditzler/bio-course-materials/blob/master/notebooks/BioPython-Tutorial.ipynb)


# ideas / to add

- Installation using easy_install  
easy_install -f http://biopython.org/DIST/ biopython

- pypi

- Summary  
  - Bioinformatics data is heavy on strings (sequences) and various types of tab delimited tables,
as well as some key:value pairs such as GenBank records (field header: field contents). There
are also some complex data structures such as multiple alignments, phylogenetic trees, etc.
BioPython is a collection of Python modules that provide functions to deal with Bioinformatics
data types and functions for useful computing operations (reverse complement a DNA string,
find motifs in protein sequences, access web servers, etc.) as well as ‘wrappers’ that provide
interfaces to run other software (both via webservers and installed on your local computer) and
work with the output.from http://fenyolab.org/presentations/Introduction_Biostatistics_Bioinformatics_2014/pdf/homework2.pdf
  - The SeqIO.parse function is creating SeqRecord objects
  - Biopython's SeqRecord objects are a container holding the sequence, and any annotation about it - most importantly the identifier.


why file handles




