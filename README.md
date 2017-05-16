# Solvate.sh
Wrapper script for Amber's LEaP ('tleap') to solvate a system with specific number of water molecules. Also makes use of cpptraj for determining molecule info.

# Usage
```
Solvate.sh <input_file>
Input File Options: (default)
    target <#>         ) Target # of waters to add.
    buffer <buf>       ) Initial buffer size (10.0).
    bufx   <buf>       ) Initial buffer X size (mode 2|3 only, 10.0).
    bufy   <buf>       ) Initial buffer Y size (mode 2|3 only, 10.0).
    pdb <file>         ) Solute PDB file name.
    top <name>         ) Output topology (solvated.parm7).
    crd <name>         ) Output coordinates (solvated.rst7).
    leapin <file>      ) Leap input script for loading parameters etc.
    ionsin <file>      ) Optional Leap input for loading ions etc (run after solvating).
    templeap <name>    ) Name of temporary leap input script (temp.leap.in).
    tol <#>            ) Number of waters > target allowed, will be removed (2).
    mode <#>           ) Solvate mode:
                         (0)- SolvateOct
                          1 - SolvateBox
                          2 - SolvateBoxXYZ (bufx and bufy are scaled)
                          3 - SolvateBoxZ (bufx and bufy are fixed)
    loadpdb {yes|no}   ) If (yes), use 'loadpdb PDB'; otherwise <leapin> should set up unit <molname>.
    loadcmd <cmd>      ) Command to load solute file; default 'loadpdb'.
    soluteres <#>      ) Number of solute residues. If blank try to guess from PDB.
    molname <name>     ) Solute molecule unit name ('m').
    solventunit <name> ) Solvent unit (TIP3PBOX).
      Recognized solvent units: TIP3PBOX SPCBOX OPCBOX TIP4PEWBOX
```

First the file specified by `leapin` is read by LEaP, then the system is solvated, then the file specified by 'ionsin' is read in order to add ions etc.

# Example
Solvate an RNA tetranucleotide with 2500 TIP3P waters and 3 Na+ ions.


Input file: solvate.in
```sh
# Target number of waters
target 2500 
# Initial guess for buffer
buffer 10
# Input PDB name
pdb rGACC.pdb
# Output topology name
top rGACC.tip3p.parm7 
# Output coordinates name
crd rGACC.nomin.rst7
# Base leap input script
leapin leap.solvate.in
# Additional script for adding ions etc
ionsin leap.ions.in
# Tolerance (# of waters off from target allowed)
tol 3
# 0 - SolvateOct
mode 0
```

LEaP input: leap.solvate.in
```
source leaprc.RNA.OL3
set default pbradii mbondi2
```

LEaP Ions input: leap.ions.in
```
addions m Na+ 1
addions m Na+ 1
addions m Na+ 1
```

