Peptide Sequencing Analysis Pipeline

Complete automated analysis pipeline for peptide de novo sequencing data with mass distribution analysis, sequence visualization, and validation metrics.
Overview

This Python script processes peptide de novo sequencing results PEAKS 13 (Bioinformatics Solutions Inc.) and prints:
    Mass distribution analysis (KDE, violin, cumulative plots)
    Sequence visualizations (WebLogos - information content & probability)
    Alternative visualizations (stacked bars, heatmaps, radar charts, bubble charts)
    Validation metrics (precision, recall, F1-score, TP/FP/FN analysis)
    Quality control dashboard summary panel)

1. Data Organization

Create a data/ folder in the project directory and place your input files:

your-project/
├── script.py
├── README.md
└── data/
    ├── denovoAllCandidates.csv      (de novo sequencing results, obtained from PEAKS 13)
    ├── Sample 1.denovo.csv          (denovo reference file, obtained from PEAKS 13)
    └── Lib2_theoretical.csv                      (theoretical library, generated using Library_generation script)

3. Configure Settings

Edit the configuration section in the script, e.g. examplewise given for library No2: XXK, X = All aa except I and C:

# Filter settings
length_criteria = 3                    # Peptide length
ptm_positions = [2]                   # PTM positions (0-indexed)
specific_letters = ['K']              # Fixed amino acids
specific_positions = [2]              # Position of fixed amino acids
alc_threshold = 85                    # Minimum ALC score (%)

# Variable positions
variable_positions = [1, 2]           # Positions to analyze

# Allowed letters and positions
allowed_letters = {'A', 'R', 'N', 'D', 'E', 'Q', 'G', 'H', 'L', 'K', 'M', 'F', 'P', 'S', 'T', 'W', 'Y', 'V'}  # amino acids used in variable positions in library
allowed_positions = {0, 1}  # variable amino acid positions in library, indexed 0

# Amino acids to track
amino_acids_to_track = ['A', 'R', 'N', 'D', 'E', 'Q', 'G', 'H', 'L', 'K', 'M', 'F', 'P', 'S', 'T', 'W', 'Y', 'V'] # amino acids used in variable and FIXED positions in library
amino_acids_at_VARIABLE_positions = ['A', 'R', 'N', 'D', 'E', 'Q', 'G', 'H', 'L', 'K', 'M', 'F', 'P', 'S', 'T', 'W', 'Y', 'V'] # amino acids used in variable positions in library
variable_positions = [1, 2] # variable amino acid positions in library, indexed 1

Line 225:
         + mass.Composition(formula='H').mass() # replace 'H' with 'C8H16O' for Octanoyl N-term acylation 

5. Input Data Format
Required CSV Files
- denovoAllCandidates.csv and Sample 1.denovo.csv results from PEAKS 13 (Bioinformatics Solutions Inc.) 

Required columns:
    Peptide: Peptide sequence with PTMs (e.g., A(+15.99)K(+42.01)R)
    ALC (%): Average local confidence score (0-100)
    ppm: Mass error in parts per million
    Mass: Peptide mass

- theoretical peptide library (expected sequences, generated using Library_generation script))

6. Output Files

All results are saved to the data/ folder:
Visualizations
Mass Distribution (6 plots):
    scatterplot_ALC_vs_ppm.png - Quality metrics scatter
    mass_distribution_KDE.png - Kernel density estimation
    mass_distribution_violin.png - Violin plot comparison
    mass_distribution_CDF.png - Cumulative distribution
    mass_error_distribution_ppm.png - PPM error histogram
    ALC_score_distribution.png - ALC score histogram

Sequence Logos (6 plots):
    logo_overall_sequence_information.png - Shannon information content
    logo_variable_positions_information.png - Variable positions only
    logo_by_position_information_by_position.png - Individual positions
    logo_overall_sequence_probability.png - Probability format
    logo_variable_positions_probability.png - Variable positions probability
    logo_by_position_probability_by_position.png - Position probability

Alternative Visualizations (7 plots):
    ALT_1_stacked_bars.png - Stacked bar chart
    ALT_2_grouped_bars.png - Grouped bar chart
    ALT_3_heatmap_pwm.png - Position weight matrix heatmap
    ALT_4_sequence_alignment.png - Sequence alignment view
    ALT_5_bubble_chart.png - Bubble chart
    ALT_6_radar_chart.png - Radar chart
    ALT_7_information_content.png - Information content by position

Traditional Analysis (4 plots):
    amino_acid_freq_by_position.png - Position histograms
    amino_acid_heatmap_by_position.png - Frequency heatmap
    amino_acid_frequency_overall_histogram.png - Overall frequency
    sequence_length_distribution.png - Length distribution

Validation Metrics (3 plots):
    precision_recall_metrics.png - Performance metrics
    TP_FP_FN_counts.png - Classification counts
    venn_diagram_TP_FP_FN.png - Overlap diagram

Quality Control:
    QC_summary_dashboard.png - 5-panel summary used in the SI of the publication (A: KDE, B: Logo, C: Frequency, D: Position, E: Stats)

Data Files
    theoretical_masses.csv - Calculated theoretical masses
    output_statistics.csv - Precision, recall, accuracy, F1-score
    true_positives.csv - Correctly identified peptides
    experimental_peptides.csv - All sequenced peptides
    filtered-peptides-allowedletters.csv - Filtered peptide list
