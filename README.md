# MitoHPC2

MitoHPC2 is installed on Myriad here:
```
/home/skgtrk2/MitoHPC2
```
Activate conda
```
conda activate MitoHPC_LR
```
Something like this to run it:
```
echo $HP_SDIR
cp $HP_SDIR/init.hifi.sh .
nano init.hifi.sh 
. ./init.hifi.sh
$HP_SDIR/run.hifi.sh > run.KB.sh
bash ./run.KB.sh 
```

For short reads use the same, just init.sh instead of init.hifi.sh

