# ztree2stata

`ztree2stata` is an `ado` file for STATA. 
It imports a z-Tree (Fischbacher, 2007) data file and
converts it into Stata format. Specifically `ztree2stata`:

1.  opens an ASCII text file saved by z-Tree,
2.  keeps the data of the specified table,
3.  renames variables using the variable names in z-Tree, and
4.  converts the data into numerical type if possible.

## Installation
copy `ztree2stata.ado` and `ztree2stata.hlp` into
`C:/Program Files/StataNN/ado/base/z`. If you cannot find such a
directory, then run `sysdir` command and see \[U\] **20.7 How do I add
my own ado-files?**.  

## Quick start

Import the subjects table of 040517_1230.xls

```bash
ztree2stata subjects using 040517_1230.xls
```

Import the globals table of 040517_1230.xls after clear the data in the
memory

```bash
ztree2stata globals using 040517_1230.xls, clear
```

Import the data of from the 2nd and the 4th treatments and save the data
in dta format

```bash
ztree2stata subjects using 040517_1230.xls, treatment(2 4) save
```

Import the subjects table of 040517_1230.xls and 221211_1749.xls and
combine them

```bash
ztree2stata subjects using 040517_1230.xls, save
ztree2stata subjects using 221213_1749.xls, clear save
use 040517_1230-subjects.dta, clear
append using 221213_1749-subjects.dta
```

## Syntax

ztree2stata *table* using *filename* [, *options*]

## Options

`clear` permits the data to be loaded even if there is a dataset
already in memory and even if that dataset has changed since the data
were last saved.

`save` allows Stata to save the data in memory as
*filename-table*.dta.

`replace` permits `save` to overwrite an existing dataset.

`treatment`(*numlist*) specifies treatments that will be
imported into memory. If this option is omitted, then all treatments
will be imported.

`except`(*string*) specifies strings that will be used to
skip the automated renaming. If there is any trouble with variable
names, then the except option may solve it. See the example below for
more details.

`string`(*string_varlist*) specifies variables to which
`destring` (the conversion process to numeric) is not applied. This
option was necessary for the previous versions through 2014; it is no
longer required to specify which variable is a string in the current
version.

## Remarks and examples

z-Tree records the various data during the experiment in multiple
tabular formats, and after the experiment is completed, these tables are
combined and output as a single tab-delimited ASCII file with xls
extension. There are several issues when Stata imports the file. They
include, for example,

1.  The file includes several tables (e.g., subjects, globals, and
    clients), though they must be separated for analysis.
2.  Because the file includes variable names as strings, every numeric
    data is interpreted as a string.
3.  When z-Tree data includes several treatments, the width of the
    selected table varies with treatments; thus, variables may be
    recorded in different columns.

`ztree2stata` can handle these issues and conveniently convert the z-Tree data file into Stata’s dta format.

### ⊳ Example 1

`ztree2stata subjects using 040517XY.xls`

Stata reads `040517XY.xls` and keeps the data of the subjects table.

-   *table* = `subjects`  
    Specify one of the tables which have been defined in z-Tree.

-   *filename* = `040517XY.xls`  
    Stata looks for `040517XY.xls` in the current directory. Therefore,
    you need to put your data file in the current directory. If the size
    of the file is too large, then you need to increase the memory size
    before you use `ztree2stata`.

### ⊳ Example

`ztree2stata globals using 040517XY.xls, tr(2 4) save`

Stata opens `040517XY.xls` and keeps the data of the globals table in
treatment 2 and 4.

-   *options* = `tr(2 4)`  
    Stata reads the data of treatment 2 and 4. If the data does not
    include some of the specified treatments, then it returns an error.

-   *options* = `save`  
    If `040517XY-globals.dta` does not exist yet, this option allows
    Stata to save the data in memory as `040517XY-globals.dta`;
    Otherwise, Stata returns an error message and does not save the data
    in memory. To overwrite the existing file, use `save` with `replace`
    option.

### ⊳ Example

`ztree2stata subjects using 050301AB.xls, except(foo goo) `

-   *options* = `except(foo goo)`  
    If you have any trouble with variable names, then you may want to
    define an exception. In this example, `ztree2stata` renames a
    variable whose name includes “foo” or “goo” into foo# or goo#, where
    \# is the original column number where the variable is located. For
    instance, suppose that you have 3 variables in a z-Tree table —
    “food,” “good?,” and “goodfood!” — and that they are located in the
    6th, 7th and 10th column, respectively. Then, they will be renamed
    as “foo6”, “goo7,” and “goo10.”

### □ Technical Notes

-   The ado and hlp files, ztree2stata.ado and ztree2stata.hlp, are
    available through the author’s website.

-   ztree2stata cannot open Excel files. A data file created by z-Tree
    is not an Excel file but a tab-delimited ASCII file, while its
    extension is xls. Therefore, once a data file is overwritten as an
    Excel file, ztree2stata can no longer open it.

-   The following symbols will be deleted from the variable names:
    exclamation marks (!), colons (:), semi-colons (;), equal signs (=),
    double quotation marks (“), and spaces ( ).

-   The except option can be used to eliminate any other symbols or
    letters from variable names.

-   Note that when there is an unpaired double quotation mark in a data,
    Stata’s import results may not be as intended.

-   This command is provided as it is, without any warranty.

## Reference

Fischbacher, U. 2007. “z-Tree: Zurich Toolbox for Ready-made Economic
Experiments,” *Experimental Economics*, 10(2), pp.171-178.
(<http://www.iew.unizh.ch/ztree/index.php>)

Takeuchi, K. 2023. “ztree2stata: A data converter for z-Tree and Stata users,”
*Journal of the Economic Science Association*, R&R. 
