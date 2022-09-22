We will use cif to cell to generate cell files from cif files. Cif files can be obtained from the ICSD (inorganic crystals) or the CSD (molecular crystals).

'''MgO'''
Examine the [file](http://www.castep.org/files/MgO.cif). Look at the crystal structure with jmol or magresview (drag and drop the file onto the magresview window). You will see a cubic face-centre cubic cell. How many atoms in the cell?

Use cif2cell to convert this cif file to a cell file.
```
cif2cell MgO.cif --program=castep -o MgO.cell
```
Look at the cell file. How many atoms in the cell? View this will jmol or magresview. Do you understand what cif2cell has done?


'''Molecular Tweezers'''
Examine the [file](http://www.castep.org/files/GUQHUV.cif)
