# MitoSalt on UCL cluster

## MitoSalt on short reads, best on Hestia
<br>No copying of fastqs is needed as I mounted the RDS dirs

<br>I did not manage to submit as conda is not properly activated when I invoke it with qsub, so one workaround is to run it on the node using nohup:
```
conda activate MitoSALt
cd /data/hestia/rkabiljo/MitoSAlt_1.1.1
nohup bash ./callMitoSaltSR.sh ../UCL_RDS/WGS/fastq/IC_RP_0003_R1.fastq.gz ../UCL_RDS/WGS/fastq/IC_RP_0003_R2.fastq.gz IC_RP_0003
```


On Myriad, there are two versions, one for long reads, one for short reads.  The difference is that for long reads, instead of sending the two fastq files, send one bam file, already aligned.  My hack is to skip the alignment - I commented it out in the code, and copy the alignment and rename it to a particular nam e format that the script will recognise as the alignment and it picks it up from there.  It also needs to be sorted, so I do that as the first step.  It only works for PacBio data, not for ONT.
```
cd Scratch/MitoSAlt_1.1.1
conda activate MitoSALt
perl MitoSAlt1.1.1_LR.pl config_human.txt withRG_tmp_ONT_CG.bam  ONT_CG_RG
```


