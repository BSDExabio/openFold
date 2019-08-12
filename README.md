# ProteinFoldingPipeline
Pipeline for protein folding using only simulated annealing, distance and torsion restraints; works from an amino acid sequence; AmberTools18 wrapped in Python

**Pre-Reqs:**
1. Python (3 or later, though you can probably modify the code to work with 2) (https://www.python.org)
2. AmberTools: http://ambermd.org/GetAmber.php. <br/>
For a simple-to-install, non parallelized version, you can used conda (Miniconda: https://docs.conda.io/en/latest/miniconda.html):
```
conda install ambertools=19 -c ambermd
conda install numpy
```

3. BioPython: https://biopython.org <br/>
If you have pip, you can do:
```
pip install biopython
```
**To Run WITHOUT your own restriants:**
1. Download and Install Required programs
2. Pull Branch
3. Make/Download FASTA/txt file
4. Download original pdb file to pull restraints from
5. Run automate_pipeline.py:
```
python automate_pipeline.py <fasta> <name string (MUST match name on pdb file)> <Angrstoms for restraint range (float) <force constant> <# cycles simulated annealing>
```

**To Run WITH your own restriants (SUGGESTED):**
1. Download and Install Required programs
2. Pull Branch
3. Make FASTA/txt file and 8 column restraints file (format below)
4. Run fold_protein.py:
```
python fold_protein.py <fasta> <name string> <restraints file> <force constant> <# cycles simulated annealing>
```

**Input:**

1. FASTA file (or txt file) with the sequence in single letters (i.e., "NLYIQWLKDGGPSSGRPPPS")
2. name of the protein as a string (used for output files)
3. Restraints list in 8 col file:

Restraints File should be formatted as a list of restraints, with the following columns:

atom1_residue_number (int), atom1_residue_name, atom1_name, atom2_residue_number (int), atom2_residue_name, atom2_name, lower_bound_distance (float), upper_bound_distance (float)

For example:

    2   MET   CB    41    ALA   CB    3.81    5.81
    3   PHE   CB    15    GLY   CA    9.4     11.6
    etc

I used make_rst.py to make restraints. The file uses BioPython and should be easy to modify and use if you are making restraints from an original pdb file. Feel free to create your restraints list in other ways.

4. Force Constant for the restraints as a float (in kcal/mol·Angstroms)
5. Number of simulated annealing cycles to run (int)

**Output:** folded protein in pdb file

**Optional Scripts:**
1. make_rst.py
```
python make_rst.py <name of pdb file without the .pdb extension> <# Angstroms above/below range midpoint (float)>
```
2. scores.py <br/>
If you want RMSD and TMScores, the TMScore program () will produce both, and scores.py will parse the info and put it in a seperate file for you.
```
```
3. METRICS

**References:**

Peter JA Cock, Tiago Antao, Jeffrey T Chang, Brad A Chapman, Cymon J Cox, Andrew
Dalke, Iddo Friedberg, Thomas Hamelryck, Frank Kauff, Bartek Wilczynski, et al. Biopython:
freely available python tools for computational molecular biology and bioinformatics.
Bioinformatics, 25(11):1422–1423, 2009.

Thomas Hamelryck and Bernard Manderick. Pdb file parser and structure class implemented
in python. Bioinformatics, 19(17):2308–2310, 2003.

Peter W Rose, Andreas Prli´c, Ali Altunkaya, Chunxiao Bi, Anthony R Bradley, Cole H
Christie, Luigi Di Costanzo, Jose M Duarte, Shuchismita Dutta, Zukang Feng, et al. The
RCSB protein data bank: integrative view of protein, gene and 3d structural information.
Nucleic Acids Research, page gkw1000, 2016.

D.A. Case, I.Y. Ben-Shalom, S.R. Brozell, D.S. Cerutti, T.E. Cheatham, III, V.W.D. Cruzeiro, T.A. Darden, R.E. Duke, D. Ghoreishi, M.K. Gilson, H. Gohlke, A.W. Goetz, D. Greene, R Harris, N. Homeyer, S. Izadi, A. Kovalenko, T. Kurtzman, T.S. Lee, S. LeGrand, P. Li, C. Lin, J. Liu, T. Luchko, R. Luo, D.J. Mermelstein, K.M. Merz, Y. Miao, G. Monard, C. Nguyen, H. Nguyen, I. Omelyan, A. Onufriev, F. Pan, R. Qi, D.R. Roe, A. Roitberg, C. Sagui, S. Schott-Verdugo, J. Shen, C.L. Simmerling, J. Smith, R. Salomon-Ferrer, J. Swails, R.C. Walker, J. Wang, H. Wei, R.M. Wolf, X. Wu, L. Xiao, D.M. York and P.A. Kollman, AMBER 2018. University of California, San Francisco (2018).

GuoliWang and Roland L Dunbrack. Pisces: recent improvements to a pdb sequence culling
server. Nucleic Acids Research, 33(suppl 2):W94–W98, 2005.

Stefan Seemayer, Markus Gruber, and Johannes S¨oding. Ccmpred—fast and precise prediction
of protein residue–residue contacts from correlated mutations. Bioinformatics,
30(21):3128–3130, 2014.

Sheng Wang, Siqi Sun, Zhen Li, Renyu Zhang, and Jinbo Xu. Accurate de novo prediction
of protein contact map by ultra-deep learning model. PLoS Computational Biology,
13(1):e1005324, 2017.

Axel T Brunger, Paul D Adams, G Marius Clore, Warren L DeLano, Piet Gros, Ralf W
Grosse-Kunstleve, J-S Jiang, John Kuszewski, Michael Nilges, Navraj S Pannu, et al. Crystallography
& nmr system: A new software suite for macromolecular structure determination.
Acta Crystallographica Section D: Biological Crystallography, 54(5):905–921, 1998.

Axel T Brunger. Version 1.2 of the crystallography and nmr system. Nature protocols,
2(11):2728, 2007.

Michael Nilges, G Marius Clore, and Angela M Gronenborn. Determination of threedimensional
structures of proteins from interproton distance data by hybrid distance
geometry-dynamical simulated annealing calculations. FEBS Letters, 229(2):317–324, 1988.

Evan G Stein, LukeMRice, and Axel T Br¨unger. Torsion-angle molecular dynamics as a new
efficient tool for nmr structure calculation. Journal of Magnetic Resonance, 124(1):154–164,
1997.

Peter Eastman, Jason Swails, John D Chodera, Robert T McGibbon, Yutong Zhao, Kyle A
Beauchamp, Lee-Ping Wang, Andrew C Simmonett, Matthew P Harrigan, Chaya D Stern,
et al. Openmm 7: Rapid development of high performance algorithms for molecular dynamics.
PLoS Computational Biology, 13(7):e1005659, 2017.

William Humphrey, Andrew Dalke, and Klaus Schulten. Vmd: Visual molecular dynamics.
Journal of Molecular Graphics, 14(1):33–38, 1996.

Yang Zhang and Jeffrey Skolnick. Scoring function for automated assessment of protein
structure template quality. Proteins: Structure, Function, and Bioinformatics, 57(4):702–
710, 2004.

Jinrui Xu and Yang Zhang. How significant is a protein structure similarity with tm-score=
0.5? Bioinformatics, 26(7):889–895, 2010.
