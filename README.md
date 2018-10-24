# CyanoHABs
comparative genomics of blooming cyanobacteria vs non-blooming species
## Author:  Quinn Fischer Jaden Wass
## Updated: 04/28/18
## Version: 1.1
## Description: Contains information on the files in GenomeProject/ Directory and its Subdirectories.
## For .sh files more specific information is contained within the file near the top.
##=================================
First directory in pipeline::
GenomeProject/data/ - Contains files related to getting genomes from online databases and their faa files.
makeLists.sh - gets relevant entries for genomes wanting to be downloaded and unifies them.
getGenomeData.sh - downloads listed genomes and extracts their faa files.
blackList.txt - contains a list of genomes that have been blacklisted from being downloaded and listed.
    sources/ - contains:
        masterList.txt - contains the unified list of genomes and their data.
        toUnify/ - contains lists of desired genome entries obtained from larger lists.
    rawGenomeData/ - contains compressed copies of genomes from database listed in its list entry.
    faa/ - contains uncompressed faa files from genomes downloaded into rawGenomeData

Second directory in pipeline::
GenomeProject/blast/ - Contains files related to running ncbi blast on faa files obtained in data/
runBlast.sh - runs blast upon downloaded faa files against our database.
blastResults/ - contains resulting blastResults.
partition/ - contains files necessary to partition blast work across multiple cores.

Third directory in pipeline::
GenomeProject/parse/ - Contains files related to parsing the ncbi blast results obtained in parse/
blastParse.sh - Parses the blast results, placing them into the parseResults/ directory.
parseResults/ - contains:
    masterParseResults.txt - parse results of all genomes sorted together.
    genomes/ - directory where individual parse results of each genome are.

Fourth directory::
GenomeProject/stats/ - Contains files related to getting stat results upon parsed blast results.
runStats.sh - runs stats upon parsed blast results.
nameCategories/ - directory containing protein names with varying specificity. Generated automatically.
tables/ - contains:
    genomeTable.txt - same as masterList.txt.
    genomeStatTable.txt - table of stat results and pathway information about each genome.

Fifth directory in pipeline::
GenomeProject/fasta - contains: all relevant info for fasta files.
   data/ - contains: all data for fasta files
      created/ - contains: all data created by sources ran
         Original_fastafiles - all fasta files created from the blastResults.txt
         Samelength_fastafiles - These fasta files are modified from Original_fastafiles by fastalength.sh and ProteinSequenceLength.py to ensure all protein sequence lengths are the same.
         Similarsequence_fastafiles - These fasta files are modified from Samelength_fastafiles by ProteinStringChecker.py and topPercent.sh to throw out any protein sequences that had too high of difference than what was expected.
      needed/ - contains: all needed data to first create fasta files. This data can also be found in the first and third pipeline directory.
         masterParseResults.txt - contains the blast results needed with the faa files to create the fasta files. 
         faaFiles/ - list of all the faa files corresponding to the blast results. Needed in order to create fasta files.
   sources/ - contains: all sources to create all files in created/
      fastaFormat.py - reads the blastresults.txt file and creates the Original_fastafiles from the faa files. 
      fastalength.sh - used to call ProteinSequenceLength.py on all the Original_fastafiles to make Samelength_fastafiles.
      ProteinSequenceLength - Takes a fasta file and ensures all the protein sequence lengths are the same.
      ProteinStringChecker.py - Takes fasta file that has all same length protein sequence and keeps all sequences similar to eachother.
      topPercent.sh - calls ProteinStringChecker on all Samelength_fastafiles.

Sixth directory in pipeline::
PGenomeProject/stockholm - contains: all relevant info for stockholm files
   data/ - contains: all stockholm files created
      Stockholmfiles/ - all stockholm files created from stockholm.sh bioscripts.convert-0.4 and Similarsequence_fastafiles.
   sources/ - contains: all relevant sources for creating the stockholm files from Similarsequence_fastafiles
      bioscripts.convert-0.4 - Open Source online tool used to convert a fasta file to a stockholm file. Download and install instructions at  https://pypi.python.org/pypi/bioscripts.convert/0.4
      stockholm.sh - used to convert all Similarsequence_fastafiles to stockholm format with bioscripts.convert/0.4

Seventh directory in pipeline::
GenomeProject/hmmer - contains: all relevant info for hmmer files. For more info on hmmer application refer to http://eddylab.org/software/hmmer3/3.1b2/Userguide.pdf
   data/ - contains: all data files created
      Hmmr_models - all hmmer models created from the stockholm files.
      hmmSearchResults - all hmmsearch results from running hmmsearch on each hmmer model with each faa file.
      hmmSearchParsing - file of parsed hmmSearchResults, creating a table format for easy to interpret data.
   sources/ - contains: all relevant sources for creating hmmer files
      hmmerModel.sh - used to run hmmbuild on all stockholm files.
      hmmerSearch.sh - used to run hmmsearch on all hmmer models for every faa file. 
      hmmer3-2-table_new.sh - used to put the hmmSearch results into a table.
      hmmSearchParsing.sh - Script that runs hmmer3-2-table_new.sh on every file in hmmSearchResults

Eighth directory::
GenomeProject/references - contains references/copies of outside information used in the pipeline.
faaData/ - contains files for building our blast database.
database.faa - file generated by makeFaaDatabase.sh for building the faa database.
makefaaDatabase.sh - script to build our database with an option to include or not include fig pathways. (Warning fig pathways make blast results take far longer, plan accordingly)
faaData/originalData/ - contains files necessary to build our blast database.
bloomTable.txt - contains information on whether or not a genome is blooming or not.
lengthTable.txt - contains information on how large in (bp) each protein complex is.
nameConversionTable.txt - contains original names used for proteins in the database and the new names used to fix errors and disambiguate the names to be more algorithm friendly. Essentially a hash table.
oldReferences/ - contains old references files/ references file that are changed into used references files.
centralmetb.all.faa.length - contains information on how large in (bp) each protein complex is.
hc2.2ndmetb.faa.length - contains information on how large in (bp) each protein complex is.
database/ - contains our blast database being used to compare the unknown faa proteins against.
sources/ - contains files from which genomes are being selected to be downloaded, blasted, parsed, and stated.
