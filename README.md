# BugsInPy Multi-fault
A multifault version of the [BugsInPy](https://github.com/soarsmu/BugsInPy) dataset.
## Description
This repository includes all the necessary scripts and files needed to create a
multi-fault dataset from the original BugsInPy single-fault dataset. This is
done using a test case transplantation and fault location translation process
for bug exposure and identification respectively.

The test case transplantation process is based on the ideas from [An et.
al.](https://github.com/coinse/Defects4J-multifault), and is included in this
repository (see [version search replication](#replication-of-version-searching).
For details on the fault location translation process, see the
[bug-backtracker](https://github.com/DCallaz/bug-backtracker).

## Setup
### Requirements
* python3 >= 3.8
* virtualenv
* The project specific dependencies contained in `dependencies.txt`
### Steps
This repository contains a standalone clone of the BugsInPy dataset. In order to
set up this dataset, simply run the following line:
```
export PATH="$PATH:/path/to/bugsinpy/framework/bin"
```
Add the above command to your bashrc (or similar) for loading upon startup.

You can then test your installation by running:
```
bugsinpy-multi-checkout -h
```
which should print the help message of the multi-fault checkout command.
## Usage
The following commands are available for use within the dataset:

Command | Description
--- | ---
info | Get the information of a specific project or a specific bug
checkout	| Checkout buggy or fixed version project from dataset
multi-checkout | Checkout a multi-fault version project from the dataset
compile	| Compile sources from project that have been checkout
test	| Run test case that relevant with bug, single-test case from input user, or all test cases from project
coverage |	Run code coverage analysis from test case that relevant with bug, single-test case from input user, or all test cases. Also produces TCM file
to-tcm | (Optional) Create or update a code coverage TCM from the collected coverage
identify | Mark each of the faults identified in a multi-fault version in the created TCM
mutation |	Run mutation analysis from input user or test case that relevant with bug
fuzz | Run a test input generation from specific bug

Any of the above commands can be run as: `bugsinpy-<command>`, for example:
```
bugsinpy-info -p PySnooper
```
will print project specific information for the PySnooper project. All of the
commands will print help information when supplied with the `-h` option.
## Usage example
A common use case for the BugsInPy multi-fault dataset is for use in evaluation
for debugging techniques. In order to achieve this, the following process can be
done:
1. Checkout a particular project version (this command may take a while):
  ```
  bugsinpy-multi-checkout -p black -i 4 -w $PWD/black-4
  ```
2. Change to the checked out directory:
  ```
  cd black-4/black
  ```
3. Compile the project
  ```
  bugsinpy-compile
  ```
4. Collect coverage for the version
  ```
  bugsinpy-coverage -a
  ```
5. Mark each of the identified faults in the TCM
  ```
  bugsinpy-identify
  ```
  A TCM file called `coverage.tcm` will then be available for this version.
## Replication of version searching
In order to replicate the experiments and verify which bugs are exposable in
each version, the `version_search.sh` script has been provided. The script can
be run as follows:
```
./version_search [-l <log dir>] <project>
```
which will generate the versions that each bug is found to be exposable in, and
store these in the `log dir` (default `versions/`).

To ensure maximum reproducibility, ensure that you have the dependencies described
in `dependencies.txt` before running the version search. All other dependencies
are downloaded automatically, including necessary python versions, which are
stored in the automatically created `python_versions` directory.
