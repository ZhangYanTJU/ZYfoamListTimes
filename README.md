# ZYfoamListTimes

[![OpenFOAM version](https://img.shields.io/badge/OpenFOAM-7-brightgreen)](https://github.com/OpenFOAM/OpenFOAM-7)

## Usage
**supported for both collated and uncollated fileHandler**

get time list from `processor*`:
```
ZYfoamListTimes -processor
```
rm every time directory in `processor*`:
```
ZYfoamListTimes -processor -rm
```

## advanced usage

**parallelly reconstructPar!**

You can run below commands to accelerate your `reconstructPar` 6 times faster. 
```
timeList=`ZYfoamListTimes -processor`
for time in $timeList; do
    nCores=6
    joblist=($(jobs -p))
    while (( ${#joblist[*]} >= "$nCores" ))
    do
        # When the job limit is reached wait for a job to finish
        wait -n
        joblist=($(jobs -p))
    done
    reconstructPar -time $time >log.$time&
done
```