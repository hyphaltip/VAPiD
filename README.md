![VAPiD](https://github.com/rcs333/VAPiD/blob/master/.idea/VAPiD_Logo.png) 
# Viral Annotation Pipeline and iDentification
VAPiD is a ultra-lightweight script for quickly annotating and preparing sequences of human viruses for sending to NCBI Genbank.  VAPiD takes a fasta file of human viruses, an NCBI-generated author list, and sample metadata and exports Sequin files that are ready to submit to NCBI as well as human readable annotations which can be used for other analysis.

BMC Bioinformatics Manuscript (https://rdcu.be/biOrD)!

VAPiD can perform three different types of viral annotation:
1. Rapid search of a local compressed viral database for any set of viruses with a reference genome (recommended)
2. Annotation of a single viral species based on a preferred reference genome using a single Genbank accession number.
3. Comprehensive web NT database search for all viral sequences (much slower)

VAPiD is currently tested and working on Windows 10, Ubuntu 10.4, and Mac OS X.

Viruses that VAPiD has been tested with:  
RSV, Parainfluenzas, Metapneumovirus, Coronaviruses, Enterovirus/Rhinoviruses, Hepatitis A-E, Nipah, Sendai, Measles, Mumps, Rubella, Ebola, West Nile Virus, HTLV, HIV (The references for HIV can be a little variable), Norovirus, JC, BK, HPV. However, any non segmented virus that has been previously deposited on genbank or for which you posses a .gbf file ~should~ work. If you would like to use VAPiD for a virus not listed just go ahead and try running it - if you get bad annoations or errors then send an email to uwvirongs@gmail.com and I'll add support for your favorite virus. 

VAPiD currently does not support segmented viral genomes, although theoretically one could run individual segments one at a time. 

Your FASTA sequence names shouldn't be more than 23 characters, although this won't crash the program your output .gbf files will be corrupted, VAPiD will print a warning when this happens so you can edit the names and resubmit. 

As of VAPiD v1.3 support for custom virus names has been added and restrictions on slashes have been removed. For more information see the section below titled "Custom Names".

# Quickstart Installation Guide

1. Ensure you have python with numpy and biopython, mafft, and blast+ installed locally and on your path.
2. Download and install [VAPiD](https://github.com/rcs333/VAPiD/archive/master.zip)
3. Download [VAPiD Viral Database](https://github.com/rcs333/VAPiD/releases) and put it in the VAPiD folder
4. Download and install [tbl2asn](https://www.ncbi.nlm.nih.gov/genbank/tbl2asn2/) this needs to be put on your path.

# Quickstart Run Guide
1. Put your viral genomes in a single fasta file, preferably with the strain name for each sequence as the sequence header.
2. Make a author metadata submission template file for your submission at
https://submit.ncbi.nlm.nih.gov/genbank/template/submission/
3. Run VAPiD.
`vapid --db all_virus.fasta <your_virus.fasta> <your_author_metadata.sbt>`
4. VAPiD will prompt you for sample collection dates, collection locations, and coverages for each sequence in your fasta file.
5. Email .sqn files for each virus to gb-sub@ncbi.nlm.nih.gov


# Installation

**Mac or Linux**

1. Install all dependencies (Shout-out to the wonderful people who wrote these!)

Python - tested almost exclusively on python 2.7.14. Python 3 and above have syntax issues and actually break when you try to manually enter metadata.
https://www.python.org/downloads/

If you're running Python < 2.7.4 follow this guide to install pip (You may need administrator privileges.)
https://pip.pypa.io/en/stable/installing/

Download the get-pip.py and run it from the command line by typing `python get-pip.py`

If you're using Python >= 2.7.4 or you've successfully installed pip:
Open a command line and type

`pip install numpy`

`pip install biopython`

Now download and unzip this repository to your computer (anywhere will work)

Then you'll need to install tbl2asn and put it on your path. (Another option is to download tbl2asn and simply unpack it directly into the folder you've unpacked this repository into.) 
tbl2asn can be found at https://www.ncbi.nlm.nih.gov/genbank/tbl2asn2/

**Windows**

1. Install Python 2.7.14 https://www.python.org/downloads/
  Make sure that when this is installing on the third step 'Customize your python installation' the box that says put python on your path is checked. This makes installing and running this annotator much easier. (For best results run the installer program as administrator)
  If you don't see this option during python installation, after its fully installed open a command prompt window and type:

`set PATH=%PATH%;C:\python27\`

  or change C:\python27\ to wherever Python was installed (but C:\python27\ is the default install location).  

2. Install Numpy and Biopython 

`python -m pip install numpy`

`python -m pip install biopython`


3. Then download and unzip this repository (which can be downloaded with the green link on the right to the top of the page) to your computer (anywhere will do)

4. Download tbl2asn for windows https://www.ncbi.nlm.nih.gov/genbank/tbl2asn2/  and unzip it, then copy and paste every single file in the tbl2asn folder (there should be like 7 .dlls and an .exe that is simply called tbl2asn) into the folder that you just made in step 3.

**After the above has completed there's still a bit of setup that needs to be done.** 

1. Download a reference database over on the release tab, (https://github.com/rcs333/VAPiD/releases/). Download the .nhr .nsq and .nin file and place them in the VAPiD folder. You can download all of them to see which one works best for your case. Pick a different reference using the `--db` flag! (If you don't specify a database VAPiD will try all_virus, then compressed, then refseq automatically) 

2. (Mac and Linux only )Install MAFFT using your favorite package manager ('brew install mafft' or 'sudo apt-get install mafft') or download and install from https://mafft.cbrc.jp/alignment/software/ for your appropriate system. (THIS IS ALREADY DONE FOR WINDOWS and is included under the GPL licence)

3. A .sbt file that contains your name and information about the organization that you wish to submit your sequences to NCBI under. This .sbt file can be generated by filling out the form here: https://submit.ncbi.nlm.nih.gov/genbank/template/submission/ (The example.sbt file is included so that you can verify your installation, please don't use this for actual submissions)

4. Put the newly generated .sbt file onto your computer. (Generally it's easiest to just put it in the VAPiD folder). If you'll be submitting multiple sequences from different people you can generate more than one, put them all in this folder, and choose which one you want to use at run time. (Although you can only select one .sbt per fasta input) 

5. (Optional)
You can generate a .csv file with most metadata that you wish to associate with your sequences. The file should have across the top Strain (the name of the fasta sequences that you'll import) followed by columns with NCBI approved metadata https://www.ncbi.nlm.nih.gov/Sequin/modifiers.html
 
An important note is that the strain names in the metadata csv MUST match the sequence names in your fasta files. (i.e. the part after the >) 

An example metadata file has been provided under the name example_metadata.csv - this metadata file will work with the example fasta file provided (example.fasta).

If you don't have very many sequences at a time or you include a fasta not in your metadata sheet the program will automatically prompt you for strain name, collection date, country of collection and coverage (for ngs reads). You must fill out strain name, collection date and location or NCBI will not accept your submission. Coverage is not necessary and if skipped during the automatic prompting, will not create any issues.

# Usage - vapid.py

Create your fasta file with all of the sequences that you would like to annotate. You can have as many sequences as you want. And you should name the sequences in your fasta file what you would like the strain name to be. (For the provided fasta file, example.fasta, this name would be 'test'). **FASTA STRAIN NAMES SHOULD NOT HAVE SPECIAL CHARACTERS IN THEM!!** (Things like ' " ? - # * ect.) This is still true for the base name as of v1.3. See the custom names section below for more information on this. 

Then you would need to run the vapid.py script from the command line. First cd to the directory that VAPiD is living in - if you cloned from GitHub it'll be ../VAPiD/ 

Detailed help can be generated by running vapid.py -h or without any arguments. 

**Example CD Command**
Just change the path here to wherever you ended up unzipping the VAPiD folder on your computer. 

`cd C:\User\Downloads\VAPiD`

**General Usage**

`python vapid.py fasta_file_path author_template_path --metadata_loc metadata_info_path`

Optional arguments - choose one of these

`--db custom_database_location --online --r reference_accesion_num --f`

Secondary optional argument - pick any you want

`--no_spell_check --all`

**Example Usage (With metadata in the sheet)**

`python vapid.py example.fasta example.sbt --metadata_loc example_metadata.csv`

**Example Usage (No metadata sheet)**

`python vapid.py example.fasta example.sbt`

**Example Usage (With metadata sheet AND specifying the reference)**

`python vapid.py example.fasta example.sbt --metadata_loc example_metadata.csv --r KF530268.1`

**Example Usage (No metadata sheet, specifying the reference, and no spellchecking)**

`python vapid.py example.fasta example.sbt --r KY45632.1 --no_spell_check`

**Example Usage (with a custom blast database)**

`python vapid.py example.fasta example.sbt --metadata_loc example_metadata.csv --db /User/my/path/to/a/working/blastdb/`

You need to have BLAST+ available on the system path and the specified directory needs to be set up correctly or the program will crash

**Example Usage (online with no metadata)**

`python vapid.py example.fasta example.sbt --online`

**Example Usage (default reference database, spellchecking and transfering 'gene' annotations as well as 'CDS' annotations**

`python vapid.py example.fasta example.sbt --spell_check --all`

`python vapid.py -h` prints out a list of arguments and some help information

The metadata_loc argument is optional and if you don't provide a metadata location or if your sheet does not contain metadata for some sequences the program will prompt you for it.

You can just put relative paths to your sbt and fasta file, I find it is easiest to store everything in the VAPiD
folder - that way you don't have to worry about typing paths. 

# Output

The program will run for a bit and generate a folder for each sequence in your fasta file. The folder will be named the same as what you named the strain in the fasta file. (So if the first line of your fasta file is >SC12309 then there will be a folder called SC12309 with all of the .gbf, .tbl, and ect files that you need and love.) 

tbl2asn will also spit out some errors and warnings onto the command line.

The program itself will also examine each sequence record for stop codons and notify you of which ones contain stops.

Inside each folder will be some files, you can examine .gbf files either in a text editor or in something like Geneious.
To submit your sequence to NCBI simply email the .sqn file to gb-sub@ncbi.nlm.nih.gov and then shortly after your sequences will be deposited on NCBI. 

# Custom Names

I have added support to submit viruses with free form full names. For example >Sample 3 (USA/Human/2016). Also if you are using the manual metadata input feature where it prompts you for the minimum required metadata VAPiD automatically appends (country/col_year) to your name. This is in an attempt to improve the quality of sequence being sent to NCBI so they like us more. 

There are two different ways to submit with free form full names.

**Add a full_name column to your metadata.csv file** 

metadata.csv line 1: `strain,collection-date,country,coverage,full_name`

metadata.csv line 2: `test,2017,USA,42.5,test (USA/2016/A)`

Simply add a column that is named EXACTLY 'full_name' and put the full name into the metadata.csv file. If you are using this option you still need to have your fasta headers match. So for the example above your fasta file would still need to start with just >test. This is so that I can find the record corresponding to 'test' in the metadata file. In this case you do not need to put a space in the full_name. i.e. test-usa/2016/a would work using this method. 

OR

**Have your FASTA header be anything you want and use --slashes at runtime**

In this option you would have your fasta header be >test (USA/Human/2016). And you would add `--slashes` to your python command running vapid.py. When using this option, internally your samples are still represented by test, right now it is required that there be a space between >test and anything with backslashes in it. So when using this option your metadata.csv file would still have just test and you could associate collection year or any other metadata with this sample. 

In either of these methods your results will be placed into a folder that is named test. VAPiD will break if you don't have a space between your fasta header and anything with backslashes. 

I do NOT reccomend batching submissions that mix these options. Also, if my solution to this problem has introduced any new problems or simply isn't appropriate for your case please let me know and I can add support for your specific case. 


# Implementation Details and Important Notes

A large problem is actually inconsistent spelling in GenBank sequence records or sequence records that do not have every protein annotated. The ESpell utility from NCBI is currently being used to check spelling on protein names. However this can result in certain protein names losing capitilization (i.e. IIIa3 will get changed to iiia3). 

# Future directions
Preprint is avalible at (https://www.biorxiv.org/content/early/2018/09/18/420463) and paper is currently under review.
Email uwvirongs@gmail.com for feature requests or bug reports.  
