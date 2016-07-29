## Purpose

The purpose of this script is to:

1. randomize a list of participants into treatment and control groups
2. create a label for each participant that can be affixed to his or her experimental materials
3. create a master table that links each participant to his or her assignment 

## Rationale

The script may be used for any randomized control trial (RCT) in which participants are known ahead of time, who may be nested within groups, and who have observable and known characteristics upon which further stratification is required.

This script assumes that treatment and control group members will receive different materials but are unaware of the difference, that is, the materials themselves will not indicate experimental condition. It's important, therefore, that each participant gets the correct materials, especially if participants take part in the experiment concurrently. As part of its randomization routine, this script automatically creates labels that can be affixed to the proper materials ahead of time.

## Supplementary file requirements
  
1. [Requires this code](https://raw.githubusercontent.com/reingart/pyfpdf/master/tools/pdflabels.py) in the same directory saved as `pdflabels.py`. If not found, the script automatically downloads and saves the file.
2. Requires a `*.csv` file with participant names and any information required if sampling should be blocked within groups (e.g., classroom id, student gender, student race or ethnicity)

## To Use

### Initialize

In terminal (works on OS X...not tested in other systems), navigate to the script directory and type: 

```bash
./randomizelabel.py
```

or, if you want to set the Python interpreter manually:

```bash
python<3> randomizelabel.py
```

Note that this script requires Python 3.x.

### Locate `*.csv` file

You will be prompted for the location of the `*.csv` file. The script will first search the local directory for all `*.csv` files and list them:

```bash
------------------------------------------------------------
Which CSV file contains the names of those to be randomized?
------------------------------------------------------------

( 1 ) fakeclasslist.csv
( 2 ) File not in this directory

CHOICE: 
```
If you place the names file in the same directory, you can just choose it from here. If you don't, you should select the number for `File not in this directory`. You will then be prompted with:

```bash
Please give full path to CSV file:
```

You should give the full path (no `~`); for example:

```bash
/Users/<username>/randomizelabel/fakeclasslist.csv
```

### Choose primary unit of randomization

```bash
---------------------------------------------
Which column contains the randomization unit?
---------------------------------------------

( 1 ) classid
( 2 ) id
( 3 ) name
( 4 ) gender
( 5 ) racecat

CHOICE: 
```
*NB: Randomization unit column cannot contain duplicate values.*

### Decide if you want to randomize within groups
```bash
----------------------------------
Are you randomizing within groups?
----------------------------------

( 1 ) Yes
( 2 ) No

CHOICE:
```
If you choose `yes` then:

```bash
---------------------------------
Which column contains the groups?
---------------------------------

( 1 ) classid
( 2 ) id
( 3 ) name
( 4 ) gender
( 5 ) racecat
```
*NB: You cannot group on the primary randomization unit.*

### Decide if you want to stratify the randomization  
*NB: If you don't choose to randomize within groups, you won't be given the option to stratify. If you want to stratify across, for example, race/ethnicity or gender, but not within classrooms, then you should just chose to GROUP on that category. Though these terms have specific meanings, they are functionally the same as far as the randomization code is concerned.*

```bash
-----------------------------------
Should randomization be stratified?
-----------------------------------

( 1 ) Yes
( 2 ) No
```
If you choose `yes` then:  

```bash
-----------------------------------------------------
Which column(s) contains the stratification category?
-----------------------------------------------------

( 1 ) classid
( 2 ) id
( 3 ) name
( 4 ) gender
( 5 ) racecat
```
You may choose more than one category. Separate multiple choices with a space.

### Check your options

To make that you get what you are expecting, the program will give you some descriptive information about your randomization choices. For example, if you chose to randomize on `id`, group on `classid`, and stratify across `gender` and `racecat`, you will see the following:

```bash
================================================================================

For the randomization unit: id

................................................................................

Number of unique values = 400

================================================================================


================================================================================

For the grouping category: classid

................................................................................

Number of unique values = 17
Unique values: 

ENGL101.01
ENGL101.02
ENGL101.03
ENGL101.04
ENGL101.05
ENGL101.06
ENGL101.07
ENGL101.08
ENGL101.09
ENGL101.10
ENGL101.11
ENGL101.12
ENGL101.13
ENGL101.14
ENGL101.15
ENGL101.16
ENGL101.17

================================================================================


================================================================================

For the stratification category: gender

................................................................................

Number of unique values = 2
Unique values: 

Female
Male

================================================================================


================================================================================

For the stratification category: racecat

................................................................................

Number of unique values = 3
Unique values: 

1
2
3

================================================================================


```

### Decide the number of treatment groups


```bash
-------------------------------------------------
How many treatment conditions, excluding control?
-------------------------------------------------

( 1 ) 1
( 2 ) 2
( 3 ) 3
( 4 ) 4
( 5 ) 5

CHOICE: 
```

### Choose the type of labels  
  

```bash
--------------------------
Which labels will you use?
--------------------------

( 1 ) Apli-01277
( 2 ) Avery-3422
( 3 ) Avery-5160
( 4 ) Avery-5161
( 5 ) Avery-5162
( 6 ) Avery-5163
( 7 ) Avery-5164
( 8 ) Avery-8600
( 9 ) Avery-L7163

CHOICE: 
```

### Choose what you want on the labels  


```bash
---------------------------------------
What do you want on the printed labels?
---------------------------------------

( 1 ) classid
( 2 ) id
( 3 ) name
( 4 ) gender
( 5 ) racecat

CHOICE: 
```

Separate multiple options with a space keeping in mind that the order matters. For example, `3 2 1`, would gives labels that showed: 


```bash
<name>
<id>
<classid>
```

## Output

Two primary files are placed in the working directory:

1. `assignment.csv`
2. `assignmentlabels_*.pdf` sheets with the labels

#### `assignment.csv`

Is a long file that contains, the randomization column and the treatment condition. Example:

id      | assign  
:-----: | :------:
q4NSkKLNNags | C  
NRIL0Ewhq8A5	| T
UXCFYfIM6JGn	| T
MMNjGO4CtvlL	| T
5Pe8c9rHidi8	| C

For merging purposes, it's probably a good idea to randomize using a uniquely identifiable variable.

#### `assignmentlabels_*.csv`

There will be one `*.pdf` for the labels for each experimental group. If you only have one treatment and one control, the you will have two files:

```
assignmentlabels_T.csv
assignmentlabels_C.csv
```

If you have, for example, two treatment groups and one control, you will have:

```
assignmentlabels_T1.csv
assignmentlabels_T2.csv
assignemntlabels_C.csv
```

The labels themselves **will not** indicate experimental group status (for obvious reasons) so this printing scheme will mitigate mix ups. The number of pages for each group will depend on the types of labels choosen.

## Acknowledgements
* Originators and contributors to [PyFPDF](https://code.google.com/p/pyfpdf/)
* [List of random names](http://listofrandomnames.com/) and [Mark Heckmann at ryouready](https://ryouready.wordpress.com/2008/12/18/generate-random-string-name/) for helping me generate my fake class data
