# ZYfoamListTimes

[![OpenFOAM version](https://img.shields.io/badge/OpenFOAM-7-brightgreen)](https://github.com/OpenFOAM/OpenFOAM-7)

## Usage
get time list from `processors21`, if you use 21 cores:
```
ZYfoamListTimes -processors 21
```
rm every time directory in `processors21`:
```
ZYfoamListTimes -processors 21 -rm
```

## advanced usage

**parallelly reconstructPar!**

You can run below commands to accelerate your `reconstructPar` 6 times faster. 
```
timeList=`ZYfoamListTimes -processors 21`
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

If you use uncollated fileHandler, just change `ZYfoamListTimes -processors 21` to `foamListTimes -processor`.