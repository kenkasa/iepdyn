# IEPDYN: Integral-Equation formalism of Population DYNamics

**Important Note: The IEPDYN program has been integrated into the ANAlyzing TRAjectories (ANATRA) package. Users are strongly encouraged to use ANATRA for the latest version and updates.**
- https://github.com/kenkasa/anatra

## Introduction

**IEPDYN (Integral-Equation formalism of Population DYNamics)** is an MD-based method that provides a systematic approach to extract kinetic information from molecular simulation data without explicitly sampling rare-event trajectories.  

For more details, see:  
- K. Kasahara, R. Okabe, C. A. Chang, T. Mori, and N. Matubayasi, *J. Chem. Phys.* (in press)
- arXiv: https://arxiv.org/abs/2601.09187  

## Programs

* **iepdyn**  
  Main program written in Fortran90/95.  
  Distributed under the GNU GENERAL PUBLIC LICENSE Version 2, June 1991.  

  No additional programs are bundled in this repository.

## Requirements

* Intel OneAPI

Other Fortran compilers will be supported in future releases.

## Install

Clone the repository and build the program as follows:
```
git clone https://github.com/kenkasa/iepdyn.git
cd iepdyn/src
make
```
After compilation, the executable is located at:
```
/path/to/iepdyn/bin/iepdyn.x
```

## Basic usage

Run `iepdyn.x` as follows:
```
/path/to/iepdyn/bin/iepdyn.x [input_file]
```
You can generate a sample input file with detailed explanations by running:
```
/path/to/iepdyn/bin/iepdyn.x -h
```
An example of the sample input is shown below:
```
&input_param
fcv      = "cvdata1" "cvdata2" ! Time-series CV data
flist_cv = "list" ! CV-file list (Either fcv or flist_cv should be specified)
/
&output_param
fhead = "filehead" ! header of output file
/
&option_param
use_dissociate_state = .false. ! define dissociate state or not
use_reflection_state = .false. ! define reflection state or not
use_product_state    = .false. ! define product (absorbing) state or not
calc_steady          = .false. ! calculate steady-state (equilibrium) populations or not
calc_Pint            = .false. ! calculate time integral of Pj analytically or not
nstate               = 4       ! # of states
ndim                 = 1       ! # of dimensions.
nmol                 = 1       ! # of target molecules (typically 1)
dt                   = 0.1d0   ! Time grid
t_sparse             = 0.1d0   ! Sparse time-grid (used for computing TCF)
t_range              = 10.0    ! Timescale for computing K-, M-, R-, and P0-functions
t_extend             = 100.0   ! Extended timescale for outputting P- and Q-functions
dt_tcfout            = 1.0     ! Time grid for outputting P- and Q-functions
initial_state_ids    = 1 2     ! Intial state IDs
reflection_state_ids = 4       ! Reflection state IDs
product_state_ids    = 1       ! Product (absorbing) state IDs
/

&state
-500.0 -16.0  0.5 ! State 1 (weight of 0.5)
 -16.0 -15.0  0.5 ! State 2 (weight of 0.5)
 -15.0  -2.0  0.0 ! State 3
  -2.0 500.0  0.0 ! State 4
/
```
An interactive Python script for generating input files ``iepdyn_setup.py`` is bundled in this repository.
Run the script without any arguments:
```
/path/to/iepdyn/iepdyn_setup.py
```
This script guides you through the input preparation process interactively.

## Documentation

- [Parameter reference](Params.md)  
- Tutorial: *(link to be added)*  

## License

This software is distributed under the GNU GENERAL PUBLIC LICENSE Version 2, June 1991.
