Peptide Enrichment Score Calculator

This Python script processes de novo sequencing results from PEAKS 13 and calculates enrichment scores to identify peptides enriched in pellet fractions versus supernatant fractions.

1. Input data: 
	Requires triplicate "filtered-peptides-allowedletters.csv" that is created through "Library_Sequencing_plot_analysis.ipynb" script from de novo sequencing output from PEAKS 13 software.


2. Data Organization



Each replicate requires two directories:

    pellet/ - Enriched fraction samples
    supernatant/ - Control fraction samples

Folder structure:

project/
├── enrichment_analysis.ipynb
└── data/
    ├── pellet/
    │   ├── example_library-1/
    │   │   └── filtered-peptides-allowedletters.csv
    │   ├── example_library-2/
    │   │   └── filtered-peptides-allowedletters.csv
    │   └── example_library-3/
    │       └── filtered-peptides-allowedletters.csv
    └── supernatant/
        ├── example_library-1/
        │   └── filtered-peptides-allowedletters.csv
        ├── example_library-2/
        │   └── filtered-peptides-allowedletters.csv
        └── example_library-3/
            └── filtered-peptides-allowedletters.csv

3. Configure Settings

SAMPLE_PREFIX = "example_library"      # Folder name prefix
REPLICATES = [1, 2, 3]                # Number of replicates
INPUT_FILENAME = "filtered-peptides-allowedletters.csv"  # Input file name

def is_valid_peptide(peptide):
    cleaned = re.sub(r"\(.*?\)", "", peptide)
    return all(aa in {"W", "R", "N", "D", "K"} for aa in cleaned)

Change {"W", "R", "N", "D", "K"} to the variable amino acids used.

4. Expected Output

data/
└── enrichment_analysis/
    ├── Enrichment_Score.csv
    ├── example_library_Enrichment_Score-summary.xlsx
    ├── example_library_ratio_weblogo_group_100.png
    ├── example_library_ratio_weblogo_group_10-99.png
    └── example_library_ratio_weblogo_group_0.png

File: Enrichment_Score.csv, Columns:
Column	Description
Peptide	Original sequence with PTMs
Clean_Sequence	Sequence without PTM annotations
Group	Enrichment classification (100, 10-99, 0, other)
Avg_Ratio_Area	Average enrichment ratio (%)
Std_Ratio_Area	Standard deviation of ratio
Ratio_1, 2, 3	Individual replicate ratios (%)
Avg_Area_Sub	Average area difference (pellet - supernatant avg)
Std_Area_Sub	Standard deviation of area difference
Sub_1, 2, 3	Individual replicate differences


Excel Summary

File: example_library_Enrichment_Score-summary.xlsx
Sheets (one per group):
    100 - Highly enriched peptides (100% ratio)
    10-99 - Moderately enriched peptides
    0 - Not enriched peptides
    other - Edge classes (0-10)

Each sheet contains:
    Rank (1, 2, 3, ...)
    Peptide sequence
    Clean sequence
    Enrichment metrics
    Individual replicate values

WebLogo Visualizations

Files:
    example_library_ratio_weblogo_group_100.png
    example_library_ratio_weblogo_group_10-99.png
    example_library_ratio_weblogo_group_0.png


Enrichment Ratio, Equation:
Enrichment_Ratio = (Pellet / (Pellet + Avg_Supernatant)) × 100

Where:
    Pellet = Sum of Areas for peptide in pellet sample
    Avg_Supernatant = Average Area across all supernatant replicates

Range: 0 to 100%
    100% = Only in pellet (highly enriched)
    50% = Equal in pellet and supernatant
    0% = Only in supernatant (not enriched)

Area Difference, Equation:

Area_Difference = Pellet - Avg_Supernatant

Absolute difference between pellet and average supernatant levels.
