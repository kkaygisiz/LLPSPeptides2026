Molecular Dynamics Intermolecular Interaction Analyzer

Python script for analyzing and visualizing intermolecular interactions between two peptide variants in molecular dynamics simulations. Calculates contact frequencies, generates interaction matrices, and produces publication-quality heatmaps and statistical visualizations.
# The abbreviation "OWWW" stands for C8-WWWDK and "WWNW" stands for C8-WWNWK.

1. Data Organization

Create folder structure:

project/
├── Contact-analysis.ipynb (consists of 5 interconnected scripts)
└── data/
    ├── mix.tpr           (GROMACS topology)
    ├── mix.xtc           (GROMACS trajectory)
    ├── output/           (results from script 1, 2, 3, 4)
    └── output_noHbond/   (results from script 5)

3. Run Scripts in Order

bash
# Step 1: Calculate contact frequency
# Step 2: Generate interaction matrix
# Step 3: Export Heatmap Matrix as excel sheet (optional)
# Step 4: Save Plots with smaller fonts (optional)
# Step 5: Filter and analyze non-hydrogen interactions

Requires:
# GROMACS files
u = mda.Universe(r".\data\mix.tpr",      # Topology
                 r".\data\mix.xtc")      # Trajectory

# Contact threshold
contact_threshold = 3.5  # Ångströms

2. Output files:


    interaction_analysis.png (2 subplots)

    interaction_analysis_WITH_MOLECULE_SOURCE.txt (detailed report)
    01_time_dependent_interactions.png
    02_top_40_pairs_with_molecules.png
    03_atoms_by_molecule.png (2 subplots)
    04_interaction_statistics.png (4 subplots)
    05_interaction_matrix_OWWW_vs_WWNW.png

    interaction_matrix_NO_HYDROGENS.xlsx
    top_interactions_NO_HYDROGENS.xlsx
    01_heatmap_NO_H_values.png
    02_heatmap_NO_H_clean.png
    03_heatmap_NO_H_clustered.png
    04_top_40_pairs_bar_chart.png
    05_top_20_pairs_HORIZONTAL_molecules.png
    06_top_15_OWWW_atoms.png
    07_top_15_WWNW_atoms.png
    08_cumulative_interactions.png

    Reports: Plain text (.txt)
    Data: Excel spreadsheets (.xlsx)
