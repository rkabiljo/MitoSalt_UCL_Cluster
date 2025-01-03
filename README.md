# MitoSalt on UCL cluster

## Install MitoSalt on hestia
```
wget https://sourceforge.net/projects/mitosalt/files/MitoSAlt_1.1.1.zip --no-check-certificate
unzip MitoSAlt_1.1.1.zip
 cd MitoSAlt_1.1.1/
./setup.sh
```
###CHOSE NO FOR ALMOST EVERYTHING:
 ```
 Starting advanced installation, answer yes or no to the following queries below: 
 Erase existing files and create fresh (backup data from tab/indel folders)? (yes/no): 
yes
 Download and compile hisat2? (yes/no): 
no
 Download and compile bedtools2? (yes/no): 
no
 Download and compile bbmap? (yes/no): 
no
 Download and compile last? (yes/no): 
no
 Download and compile samtools? (yes/no): 
no
 Download and compile sambamba? (yes/no): 
no
 Get UCSC utilities? (yes/no): 
no
 Download human genome and extract Mt sequence? (yes/no): 
no
 Build human indexes? (yes/no): 
no
 Download mouse genome and extract Mt sequence? (yes/no): 
no
 Build mouse indexes? (yes/no): 
no
 Install R package Plotrix? (yes/no): 
no
 Install R package RColorBrewer? (yes/no): 
no
 Number of threads to use for building hisat index (your system info is given below): 
CPU(s):                             128
Thread(s) per core:                 1
Core(s) per socket:                 64
Socket(s):                          2
```
The tools are provided via conda, and the rest of the dependencies I copied across from my previous installs
edit these files
```
config_human.txt 
config_humanSR.txt
```

## MitoSalt on short reads, best on Hestia
<br>No copying of fastqs is needed as I mounted the RDS dirs

<br>I did not manage to submit as conda is not properly activated when I invoke it with qsub, so one workaround is to run it on the node using nohup:
```
conda activate MitoSALt
cd /data/hestia/rkabiljo/MitoSAlt_1.1.1
nohup bash ./callMitoSaltSR.sh ../UCL_RDS/WGS/fastq/IC_RP_0003_R1.fastq.gz ../UCL_RDS/WGS/fastq/IC_RP_0003_R2.fastq.gz IC_RP_0003
```

### If the final R script crashes because of BioStrings:

```
conda activate MitoSALt
cd /data/hestia/rkabiljo/MitoSAlt_1.1.1/
R
```
Now from R install:
```
if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager")
#select a mirror
BiocManager::install("Biostrings",lib="bin")
#after this is done, quit R
```

On Myriad, there are two versions, one for long reads, one for short reads.  The difference is that for long reads, instead of sending the two fastq files, send one bam file, already aligned.  My hack is to skip the alignment - I commented it out in the code, and copy the alignment and rename it to a particular nam e format that the script will recognise as the alignment and it picks it up from there.  It also needs to be sorted, so I do that as the first step.  It only works for PacBio data, not for ONT.
```
cd Scratch/MitoSAlt_1.1.1
conda activate MitoSALt
perl MitoSAlt1.1.1_LR.pl config_human.txt withRG_tmp_ONT_CG.bam  ONT_CG_RG
```

### MitoSalt, Myriad, ONT
the first warning I wanted to get rid of is missing Read Group tags.  Run script addRGtag.sh and then try using that alignment in MitoSalt.  Still crashing.
```
qsub addRGtag.sh
```
