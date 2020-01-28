# Pegaus gene fusions pipeline
Pegausus deletion caller for Illumina short read sequencing files currently compatible with M. tuberculosis. This software is developed for use in the ubuntu distribution of linux. This pipeline can be used to detect deletions and gene fusions in the genomes of M. tuberculosis using either M. tuberculosis H37Rv or CDC1551 as a reference. The pipeline is implemented in BASH and is built using third party libraries to complete the various steps of the assembly process. Two versions of pegasus is availible, a command line version and a graphical user interface version. Both these perform the same basic functionality, however the GUI version is more restricted in the methods that can be called.

# Installation
Because of the dependencies installation can be a pain, I have tried to automate as much as possible using build.sh but some files need to be installed to the programs directory.
Install with git 
```
git clone --recursive https://github.com/JamesGallant/Genomics.git
cd Genomics
bash build.sh
```
or download the zip file to your prefered destination and unzip.
```
unzip ./Genomics-master.zip
mv ./Genomics-master ./Genomics
cd Genomics
bash build.sh
```
Novocraft has a free version of **Novoalign** but we cannot distribute it here so this has to be installed mannually. Navigate to http://www.novocraft.com/products/novoalign/ and click the download button. Navigate to **version 3.07.00**, this software was tested on this specific version but higher versions should work as well. Click the relevent package based on your OS and this should download to your downloads folder. This needs to be copied to the **programs** folder in the **Genomics** folder and rename to **novocraft.tar.gz**, this naming is important. Once all of this is done, untar the file and were done with novo

```
tar -xzf novocraft.tar.gz

```
# Command line usage

To see the positional arguments use
 ```
 bash pegasusGui.sh -h
 ```
 ```
 bash Pegasus.sh --help
 ```
 ```
 bash Pegasus.sh
 ```
 
 # GUI usage
 ```
 bash PegasusGUI.sh
 
 ```
 
 # file requirements:
 **Raw files**
 These files need a unique identifier following by *R1_001.fastq.gz* for forward file and *R2_001_fastq.gz* for the reverse raw file. It should looks something similiar to this:
 ```
 Genome1_R1_001_fastq.gz
 Genome2_R2_001_fastq.gz
 ```
 **List of samples**
 This file should contain the unique identifier only and have one identifier per line, similiar to this.
 ```
 Genome1
 Genome2
 Genome3
 ```
 **Regions of interest for targeted deletions**
 This file stipulates the regions of interest. Our software will go to the positions stipulated here and pull out the specififed region of the genome. Each line should contain a gene name with a 3' and 5' position separted by **tabs**.
 Similiar to this:
 ```
 Gene1  1 100
 Gene5  200 300
 Gene100  1000  2000
 ```
 The pipeline will iterate through each Gene providided here for each strain provided in the sample list file. This will pull out coverage of the region to which deletions can be inferred. This is not used for gene fusions, gene fusions uses automated detection with split read callers. 
 
 # Output
 **Single nucleotide polymorphisms**
 This is not really the idea behind this software but can be done anyway, although these won't be annotated only gene coordinates will be provided in vcf format.
 
 **Structural variants**
 Concatenated file of all deletions found by Lumpy and Delly structural variant callers. This file has the coordinates and annotations of the affected genes or intergenic regions. 
 
 **Targeted regions**
 This outputs a text file containing the read counts or coverage for each base in the region specified. A rudimentary plot is also drawn from this data to quickly see if the coverage drops or not.
 
 **Gene fusions**
 A list of multigene deletions that fall within open reading frames can be found within the chimeric_genes folder. 
 The *de novo* assembled putative gene fusions can be found in the folder named chimeric genes and within that directory look for a contig_ordering folder. The folder will be named abacas followed by gene names. Naviagate to your favorite gene and open the .NoNs file, this contains the consensus sequence without N's. Use this sequence to confirm the translation and the gene fusion, we used ORF finder from NCBI.
 
