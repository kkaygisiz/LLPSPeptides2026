Molecular Dynamics Dihedral Angle Calculator

Analysis tool for calculating backbone dihedral angles from molecular dynamics trajectories. Designed for peptide conformation analysis comparing different molecular variants (OWWW vs WWNW).
# The abbreviation "OWWW" stands for C8-WWWDK and "WWNW" stands for C8-WWNWK.

This Python script analyzes molecular dynamics simulation data to calculate backbone dihedral (phi) angles for peptide molecules. It processes the last frame of a trajectory, computes angles between defined atom sets, and generates statistical summaries.

1. Data Organization

Create folder structure:

project/
├── Dihedral_analysis-Phi.ipynb
└── data/
    ├── mix.tpr           (GROMACS topology file)
    ├── mix.xtc           (GROMACS trajectory file)
    └── output-dihedral-phi/
        └── (results generated here)

Input Data Format
GROMACS Trajectory Files

Required files:

    mix.tpr - Topology/parameter file
    mix.xtc - Trajectory file (compressed)

2. Configure Dihedral Definitions

DIHEDRAL_DEFINITIONS were determined manually by atom ID in Pymol.

DIHEDRAL_DEFINITIONS = {
    'OWWW': {
        1: ['C2', 'C3', 'N4', 'C12'],
        2: ['C12', 'C13', 'N6', 'C23'],
        # ... more dihedrals
    },
    'WWNW': {
        1: ['C2', 'C3', 'N4', 'C12'],
        # ... more dihedrals
    }
}


Notation:

    Each dihedral = [atom1, atom2, atom3, atom4]
    Calculates angle between planes formed by (1-2-3) and (2-3-4)
    Atom names must match MD nomencalture exactly

A dihedral angle is the angle between two planes formed by four atoms:

      Plane 1        Plane 2
      (1-2-3)        (2-3-4)
        |              |
    1 - 2 - 3 - 4

The dihedral angle = angle between these planes
Range: -180° to +180°


The script uses the following method:

2.1. Calculate vectors from consecutive atoms
   v0 = atom1 - atom2
   v1 = atom3 - atom2
   v2 = atom4 - atom3

2.2. Calculate normal vectors to planes
   n1 = v0 × v1  (cross product)
   n2 = v1 × v2

2.3. Calculate angle between planes
   angle = atan2(cross(n1, n2)·v1_normalized, n1·n2)

Interpretation for angle Phi
Angle	Meaning
0°	Cis/synperiplanar
±120°	Gauche
180°	Trans/Antiperiplanar


3. Expected Output

bash
data/output-dihedral-phi/
├── 01_dihedral_angles_all_residues.xlsx
├── 02_dihedral_angles_OWWW.xlsx
├── 03_dihedral_angles_WWNW.xlsx
├── 04_dihedral_angles_AVERAGED.xlsx
├── 05_SUMMARY_all_analysis.xlsx
├── 06_dihedrals_by_residue.png
└── 07_averaged_dihedrals_comparison.png





