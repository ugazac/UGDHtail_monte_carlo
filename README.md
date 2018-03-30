# UGDHtail_monte_carlo

The code contained in this repository is for a Nature submission from Zachary Wood's lab at University of Georgia. The code was written in C by O. Krishna Dev at University of Michigan, who should be contacted for any technical queries.

# Description of program
The code generates random self-avoiding conformations for a given n-mer of the UGDH tail. For simplicity, in the current version, polySer is used for the tail, but the code can take any arbitrary sequence. The input for the program is as follows:
<first_tail*.inp> : input file describing the monomer containing the tail ending with a n-mer of polySer.
Hexamer_surface.pdb : The surface defined by the hexamer in PDB coordinates. Note that the coordinates in this file are taken for examining the clash between the tail and the hexamer surface. Any translation/rotation of the coordinates of atoms in this file will result in incorrect behavior.
<sec*mer.pdb> : The coordinates of the atoms of the second tail.

The program takes the first input (the n-mer tail) and generates the coordinates according to the input (which includes all bond angles and phi/psi values for the monomer). Then, ‘n’ random numbers are generated and phi/psi combinations (at 10 degree intervals) are selected at random for the n-mer tail keeping the rest of the monomer fixed.

Using the Ramachandran et al procedure with hard sphere potential, self-clashes are examined in the n-mer tail. In case no clash is found, the conformation is taken for further analysis of clash with the hexamer surface and the second tail.

For all the clashes, the Ramachandran et al defined ‘outer limits’ are used. The program will output six numbers to stdout which are delimited by a {tab} character. These six numbers are described below in the order in which they appear as output:
No clash : cumulative number of self-avoiding tail conformations found that do not show clash with either the hexamer surface or the second tail.
No surface clash : cumulative number of self-avoiding conformations found that do not show clash with the surface, but clash with the second tail.
Self-avoiding conformations : the cumulative number of self-avoiding conformations examined for clashes.
Ratio of (1)/(3) : this ratio determines the decrease in entropy in the tail due to confinement by the surface and the presence of the second tail.
Ratio of (2)/(3) : this ratio determines the decrease in entropy in the tail due to confinement only by the surface.
Total number of trials : the total monte carlo trials performed to arrive at the self-avoiding conformations.


# Pre-requisites and instructions for compiling the code

The code uses RDRAND instruction set (https://en.wikipedia.org/wiki/RdRand) to generate hardware generated random numbers defined by Intel as True Random Number Generators as opposed to software generated Pseudo-Random Number Generators. Both Intel (Ivy bridge architecture onwards) and AMD CPUs (Excavator architecture onwards) support the RDRAND instructions.

A shell script (compile.sh) has been provided to compile the C code with the required flags.

For convenience, the input files for the 3-mer, 4-mer, 5-mer, 6-mer, 8-mer and 10-mer used in the Nature paper are provided in the repo. Likewise, the hexamer surface and the conformation of the second tail (corresponding n-mers) used for the analysis are also given.

Steps to be followed for compiling and using the program on Linux are as follows:

```git clone https://github.com/ugazac/UGDHtail_monte_carlo```


```cd UGDHtail_monte_carlo```


```sh compile.sh```

The instructions above will compile the code and generate a executable called UGDH_tail_monte_carlo. The program usage is given below. Note that the order of arguments is important and should not be changed.

```UGDH_tail_monte_carlo first_tail_3mer.inp hexamer_surface.pdb sec_3-mer.pdb ```
