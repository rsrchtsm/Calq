# TooLQit

TooLQit is a LQ toolkit which contains leading order (LO) FeynRules models of all possible scalar and vector leptoquark (LQ) models. We provide the LQ models in the form of 
Universal FeynRules Output (UFO) files suitable for simulating in MadGraph5. In addition to this, we also provide the .fr files and the mathematica notebooks UFO files.

# CaLQ Version 1.0.0

## Introduction
CaLQ is a python based LHC limits calculator. It estimates the indirect limits on the LQ parameters (LQ-quark-lepton coupling and mass of LQ) using the chi-square test. 
In the current version, the CaLQ supports the U1 and S1 LQ models. The indirect limits are obtained by the recasting the current LHC dilepton search data
in terms of the LQ model parameters. The LHC dilepton search data can be found in the following papers, 
[ditau](https://www.hepdata.net/record/ins1782650) and [dielectron and dimuon](https://www.hepdata.net/record/ins1849964). 
We provide the theory and a detailed implementation of the method in this [paper](https://www.hepdata.net/record/ins1782650). 

## Setting Up
The calculator is written in python3 and we have mentioned the required packages in _requirements.txt_. One can install those by setting up a
virtual environment and installing the dependencies. 
```sh
cd <calq-directory>
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

The CaLQ is available in two modes: 
1) [interactive mode](#interactive-mode)
2) [non-interactive mode](#non-interactive-mode)

## Interactive Mode

To use the calculator in interactive mode,
```sh
python3 calq.py
```
You will be greeted with CaLQ banner and a list of available commands, a list of available couplings and the default values. 
The prompt will be `calq > `. Enter the following commands to initiate the calculator
```
calq > import_model = U1
calq > mass = 1234
calq > couplings = X10LL[1,2] X10LL[2,2]
calq > initiate
```
Once initiated the calculator will compute and display the chi-square minima for the given mass and coupling.
This will be followed by a prompt where the user can enter a coupling(s) of their choice to check whether
it is allowed or not.

The list of valid commands are:

- `import_model` to specify which leptoquark model to use.
- `mass` should be an integer between 1000 and 5000 (inclusive).
- `couplings` should enter the couplings in the following way X10LL/RR[i,j] for U1 and Y10LL/RR[i,j] for S1.
- `significance` should enter the value 1 or 2.
- `extra_width` should enter any positive real number, incase of any additional decay width of LQ.
- `ignore_single_pair` should enter _yes_ or _no_. _yes_ means that the single and pair productions will be ignored and this will speed up calculations. The default value is no.
- `systematic_error` denotes the systematic error margin. The default value is 10%.
- `status` displays the current values of input parameters.
- `help` displays the list of commands available.
- `initiate` will compute the chi-square polynomial and its minima corresponding to the current values of input parameters.

Once initiated, the calculator goes to input values accepting mode and the prompt changes to ` > `. This prompt will only accept queries of the `<f1> <f2> ... <fn>`, where _\<f1\>_ to _\<fn\>_ are floating point numbers, _n_ is the number of couplings in the parameters and the values are space separated. The expected order of couplings (which is the same as the input order) is mentioned after initiating.

Corresponding to every query, the delta chi-square value will be displayed. Whether this is allowed withing the {1,2} sigma limit is also displayed.

Type `done` to exit query mode. An example query after executing the above commands would be:
```
 > 0.1 0
 > 0.37 0.0001
 > 0.5 0.7
 > done
```

The prompt returns to the input mode and input parameters hold the previous values which can be updated. Finally, to exit the calculator,
```
calq > exit
```

## Non-interactive Mode

To use the calculator in non-interactive mode, use the tag `-ni` or `--non-interactive`. Note that input cards and query values must be provided in this mode. An example of usage in this mode:
```sh
python3 calq.py -ni --input-card sample/sample_1.card --input-values sample/sample_1.vals --output-yes sample/sample_1_yes.csv --output-no sample/sample_1_no.csv --output-common sample/sample_1_common.csv
```

```sh
python3 calq.py [options]
```
Options:
- `--help`: Display this help message.
- `--input-card=[filename]`: Input card file. Line 1: mass, line 2: couplings, line 3: ignore_single_pair, line 4: sigma. These are same as input parameter values mentioned in the interactive version.
- `--input-values=[filename]`: Input values to check from the given file. Each line contains a query value. If there are _n_ couplings, then each line would be `<f1> <f2> ... <fn>`, where _\<fi>_ are float values.
- `--non-interactive` or `-ni`: Run in non-interactive mode. This requires input-card and input-values to be specified
- `--no-banner` or `-nb`: calq banner is not printed.
- `--output-yes=[filename]`: Specify the name of output file (allowed values) (overwrites the existing file). Default: calq_yes.csv
- `--output-no=[filename]`: Specify the name of output file (disallowed values) (overwrites the existing file). Default: calq_no.csv

## Bugs and queries

Any queries or bugs regarding the calculator can be emailed to [us](arvind.bhaskar@iopb.res.in;subhadip.mitra@iiit.ac.in)
