# IEPDYN Input File Specification

This document describes the input file format for the IEPDYN program.  
The input file consists of several blocks:

- `&input_param`
- `&output_param`
- `&option_param`
- `&state`

---

## &input_param

### Description
Defines how collective variable (CV) data are provided.

### Variables

#### `fcv`
- **Type**: list of strings  
- **Description**: Time-series CV data files  
- **Note**: Mutually exclusive with `flist_cv`

**Example**
```fortran
&input_param
 fcv = "cv1.dat" "cv2.dat"
/
```

#### `flist_cv`
- **Type**: string  
- **Description**: File containing a list of CV files  
- **Note**: Mutually exclusive with `fcv`

**Example**
```fortran
&input_param
 flist_cv = "cv_list.txt"
/
```

## CV File Format
Each CV file (specified by `fcv` or listed in `flist_cv`) must follow the format below:
```
0   CV1   CV2 ...
1   CV1   CV2 ...
2   CV1   CV2 ...
```
The first column corresponds to the time index. This column is arbitrary, because it will not be read in iepdyn program.

## &output_param

### Description
Controls output file naming.

### Variables

#### `fhead`
- **Type**: string  
- **Description**: Header for output files  
- **Default**: `"filehead"`

**Example**
```fortran
&output_param
 fhead = "sample_run"
/
```

## &option_param

### Description
Defines simulation options and parameters.

---

### Boolean Options

#### `use_dissociate_state`
- **Type**: logical  
- **Description**: Whether to define dissociation states  

#### `use_reflection_state`
- **Type**: logical  
- **Description**: Whether to define reflection states  

#### `use_product_state`
- **Type**: logical  
- **Description**: Whether to define absorbing (product) states  

#### `calc_steady`
- **Type**: logical  
- **Description**: Calculate steady-state populations  

#### `calc_Pint`
- **Type**: logical  
- **Description**: Calculate time-integrated probability analytically  

---

### System Parameters

#### `nstate`
- **Type**: integer (**required**)  
- **Description**: Number of states  

#### `ndim`
- **Type**: integer  
- **Description**: Number of CV dimensions  
- **Default**: `1`

#### `nmol`
- **Type**: integer  
- **Description**: Number of molecules  
- **Default**: `1`

---

### Time Parameters

#### `dt`
- **Type**: real  
- **Description**: Time grid  
- **Default**: `1.0d0`

#### `t_sparse`
- **Type**: real  
- **Description**: Sparse time grid (for TCF)  
- **Default**: `1.0d0`

#### `t_range`
- **Type**: real  
- **Description**: Time range for correlation functions  
- **Default**: `10.0`

#### `t_extend`
- **Type**: real  
- **Description**: Extended time range for propagation  
- **Default**: `100.0`

#### `dt_tcfout`
- **Type**: real  
- **Description**: Output interval for time correlation functions  
- **Default**: `2.0d0`

---

### State Index Lists

#### `initial_state_ids`
- **Type**: list of integers  
- **Description**: IDs of initial states  

#### `reflection_state_ids`
- **Type**: list of integers  
- **Description**: IDs of reflection states  
- **Condition**: Used only if `use_reflection_state = .true.`

#### `dissociate_state_ids`
- **Type**: list of integers  
- **Description**: IDs of dissociation states  
- **Condition**: Used only if `use_dissociate_state = .true.`

#### `product_state_ids`
- **Type**: list of integers  
- **Description**: IDs of product states  
- **Condition**: Used only if `use_product_state = .true.`

---

### Example

```fortran
&option_param
 use_dissociate_state = .false.
 use_reflection_state = .true.
 use_product_state    = .false.
 calc_steady          = .true.
 calc_Pint            = .false.

 nstate = 3
 ndim   = 2
 nmol   = 1

 dt        = 1.0d0
 t_sparse  = 1.0d0
 t_range   = 10.0
 t_extend  = 100.0
 dt_tcfout = 2.0d0

 initial_state_ids    = 1 2
 reflection_state_ids = 3
/
```

## &state

### Description
Defines the region of each state in CV space.

---

### Format

Each line corresponds to one state:

```text
xmin_cv1 xmax_cv1 xmin_cv2 xmax_cv2 ... weight
```
- Number of values per line:  
```
2 × ndim + 1
```
- Constraints:
- `xmin < xmax` for each dimension

---

### Example (ndim = 1)

```fortran
&state
0.0 1.0 1.0
1.0 2.0 1.0
/
```
### Example (ndim = 2)

```fortran
&state
 0.0 1.0  0.0 1.0  1.0
 1.0 2.0  0.0 1.0  1.0
 0.0 1.0  1.0 2.0  1.0
/
```

## Notes

- `fcv` and `flist_cv` are mutually exclusive
- nstate must match the number of lines in &state
- Boolean values must be written as:
```
.true. / .false.
```
- Units and interpretation of CVs depend on the simulation setup
