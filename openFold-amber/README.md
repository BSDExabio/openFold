# openFoldTools

Protein folding using only simulated annealing, distance and torsion restraints; works from an amino acid sequence; AmberTools18 wrapped in Python

**Pre-Reqs:**
1. Python (3 or later, though you can probably modify the code to work with 2): (https://www.python.org)
2. AmberTools: http://ambermd.org/GetAmber.php. <br/>
For a simple-to-install, non parallelized version, you can used conda (Miniconda: https://docs.conda.io/en/latest/miniconda.html):
```
conda install ambertools=18 -c ambermd
conda install numpy
```

3. BioPython: https://biopython.org <br/>
If you have pip, you can do:
```
pip install biopython
```

**To Run:**
1. Download and Install required programs
2. Pull Branch
3. Make FASTA/txt file, 8 column distance restraints file (format below), and 5 column torsion restraints file (format below) <br/>
    3a. example files are provided for Ubiquitin (1ubq)
4. Edit fold_parameters.json with your parameters (explaination below)
5. Run fold_protein.py:
```
python fold_protein.py
```

**Input to the Program:  fold_parameters.json**
1. name of the protein (or run) as a string; this is just to identify output files, so you can really use any string you want
2. FASTA file (or txt file) with the sequence in single letters (i.e., "NLYIQWLKDGGPSSGRPPPS")
3. Distance Restraints list in 8 col file
4. Torsion Restraints list in 5 col file
5. List of Force Constants for the distance restraints as floats (in kcal/mol·Angstroms)
6. List of Force Constants for the torsion restraints as floats (in in 70 kcal/mol·rad)
7. List of (highest) Temperatures for the simulated annealing cycles as floats (in K)

>If the length of these Lists (#5-7) is 1, then that single value is used for all of the simulated annealing cycles.<br/>
>If the length of these lists is = number of cycles, then the first value is used for the first cycle, the second value for the second cycle, and so on.<br/>
>If the length is > cycles, the extra values are ignored.<br/>
>If the length is < cycles, the first value is used for all cycles.<br/>
>Any of the values may be 0.0, but it is reccomended the temperature is >= 100.0K.<br/>

7. Number of simulated annealing cycles to run (int)
8. The mpi "prefix" (for example, "mpirun -np 4 "), if you would like to call sander (AmberTools minimization and simulated anealing program) with mpi threading (https://www.open-mpi.org/). This is completely optional and can take an empty string instead ("").
9. The path to the Amber forcefield, if you would like to change the forcefield; will vary depending on where your AmberTools package is

>The given example file has what I have found to be good defaults.

**Distance Restraints Format:**

Distance Restraints File should be formatted as a list of restraints, with the following 8 columns:

>atom1_residue_number (int), atom1_residue_name, atom1_name, atom2_residue_number (int), atom2_residue_name, atom2_name, lower_bound_distance (float), upper_bound_distance (float)

For example:

    2   MET   CB    41    ALA   CB    3.81    5.81
    3   PHE   CB    15    GLY   CA    9.4     11.6
    etc

I used make_rst.py (details below) to make restraints. The file uses BioPython and should be easy to modify and use if you are making restraints from an original pdb file. Feel free to create your restraints list in other ways.

**Torsion Restraints Format:**

Similar to Above, but there are 5 columns:

>residue_number (int), residue_name, angle_name, lower_bound (float), upper_bound (float)

For example:
```
1    LYS    PSI    138.7    140.7
2    VAL    PHI    -103.7    -101.7
2    VAL    PSI    113.4    115.4
3    PHE    PHI    -76.1    -74.1
etc
```

**Output:** folded protein in pdb file; note the starting velocities are randomized, so the output is nondeterministic

**Optional Scripts:**
1. make_rst.py <br/>
Makes restraints from original pdb file.
```
python make_rst.py <name of pdb file without the .pdb extension> <distance range float in Angstroms> <angle range float in degrees>
```
2. secstruc.py <br/>
Appends secondary structure restraints to your original restraints file
```
python secstruc.py <sequence> <secondary structure preduction sequence> <distance file> <angle file>
```

2. scores.py <br/>
If you want RMSD and TMScores, the TMScore program (https://zhanglab.ccmb.med.umich.edu/TM-score/) will produce both; scores.py will run the program, parse the info and put it in a seperate file ("scores") for you.
```
python scores.py <TMScore executable> <original pdb file> <your new pdb file>
```
3. metrics.py <br/>
If you run it after you run scores.py, it will append the "scores" text file with data on the on the average difference between correct and new atom distances and torsions (as well as standard deviations). Could be easily modified for other metrics.
```
python metrics.py <original pdb file> <your new pdb file>
```

**Viewing Trajectory:**

The relevant files for viewing trajectory in VMD are "prmtop" (file type: AMBER7 Param) and "siman1.nc", "siman2.nc", etc, up to the number of cycles of simulated annealing (let VMD determine the file type automatically).


**Use:**

Please feel free to use/modify code. Everything is completely open-source. This work was done at the Center for Molecular Biophysics at Oak Ridge National Lab. Contact jesskwoods (at) gmail.com with corrections, questions, comments.