================================================================================
ENRICHMENT ANALYSIS that reports log enrichments scores with replicate uncertainty as volcano plot analyses 
================================================================================

--------------------------------------------------------------------------------
1. WHAT THE NOTEBOOK DOES
--------------------------------------------------------------------------------
The notebook analyses de novo peptide identifications from enrichment analysis screens of synthetic combinatorial peptide libraries.

Step 1 - "Peptide Finder" filter
  Keeps only de novo sequences that are consistent with the designed library:
    - exact peptide length
    - exact set of modified (PTM-carrying) positions
    - fixed residues at defined positions
    - allowed residues at the variable positions
    - minimum PEAKS ALC (%) score
  Duplicate entries are deliberately kept, because they are counted in step 2.

Step 2 - Enrichment score per peptide and per LC-MS run
    EnrichmentScore= Area / mean(Area) + Count / mean(Count)
  where
    Area  = mean MS1 peak area of that peptide in the run
            (zero areas are excluded from the average)
    Count = number of spectral matches for that peptide
            (counted on 'Source File'; falls back to 'Scan' if absent)
    mean(...) = run-wide mean over all peptides of the run
  Normalising to the run-wide mean makes scores comparable between runs with
  different total ion current.

Step 3 - Replicate merging, enrichment and statistics
  Enrichment scores are pivoted into a peptide x (condition, replicate) table.
  Peptides not identified in a given run are assigned a score of 0
  (explicit zero-fill, rows are NOT dropped).
    Rel_Enrichment  = sum(sample scores) / sum(control scores)
    Log2_Enrichment = log2(Rel_Enrichment)
  If one of the two sums is zero, a pseudocount of 0.1 is used in its place so
  that peptides detected in only one condition still receive a finite value.
  p-values: two-sided Student's t-test (equal variances) across replicates.
  If there is no variance within either group, p = 0.01 when the group means
  differ and p = 1.0 otherwise. p-values are capped to the range [1e-10, 1.0].

Step 4 - Visualisation
  - Sum_Control vs Sum_Sample scatter, coloured by relative enrichment
  - volcano plots (with peptide labels)
  - interactive volcano plot (Plotly, HTML)
  - sequence logos (WebLogo) of the significantly enriched peptides
  - amino-acid frequency heatmap (row-normalised, annotated in %)
  - amino-acid N-fold deviation heatmap (observed vs. expected frequency)

Peptide-selection thresholds used throughout the plots:
    p-value         < 0.2
    Rel_Enrichment  > 2    -> enriched  ("red" points)
    Rel_Enrichment  < 0.5  -> depleted  ("blue" points)


--------------------------------------------------------------------------------
2. REQUIREMENTS
--------------------------------------------------------------------------------
Python >= 3.9 with:

    numpy
    pandas
    scipy
    matplotlib
    logomaker      (sequence logos)
    plotly         (interactive volcano plot)
    jupyter / notebook or JupyterLab (or VS Code with the Jupyter extension)

Install with:

    pip install numpy pandas scipy matplotlib logomaker plotly jupyter

Tested with Python 3.11, pandas 2.x, matplotlib 3.8, scipy 1.11.


--------------------------------------------------------------------------------
3. INPUT DATA AND FOLDER STRUCTURE
--------------------------------------------------------------------------------
Every LC-MS run (replicate) needs its own sub-folder inside the base directory,
and each sub-folder must contain the de novo result exported from PEAKS Studio
as a CSV file named 'merged_peaks_output.csv' containing the EIC area for sequenced peptides.

Required columns in that CSV:

    Peptide      de novo sequence; modifications in parentheses, e.g. K(+42.01)
    Scan         scan identifier
    Area         MS1 peak area
    ALC (%)      average local confidence score

Optional column:

    Source File  used for spectral counting when present

Expected layout (control = supernatant, sample = pellet, 3 replicates each):

    data/
      supernatant-1/merged_peaks_output.csv
      supernatant-2/merged_peaks_output.csv
      supernatant-3/merged_peaks_output.csv
      pellet-1/merged_peaks_output.csv
      pellet-2/merged_peaks_output.csv
      pellet-3/merged_peaks_output.csv

Folder names must follow the pattern '<prefix>-<replicate number>', where the
prefixes are defined in the 'conditions' dictionary of the notebook.

--------------------------------------------------------------------------------
4. HOW TO RUN
--------------------------------------------------------------------------------
  1. Install the requirements (section 2).
  2. Open Volcano_Enrichment.ipynb in Jupyter or VS Code.
  3. In the second cell, set

         base_dir = Path('data')

     to the folder that holds your run sub-folders, and adjust 'conditions'
     and 'n_replicates' to your folder names / replicate number.
  4. In the third cell, set the library design (see section 5).
  5. Run the cells from top to bottom.

Notes
  - The notebook uses the '%matplotlib inline' magic; switch to
    '%matplotlib qt' for interactive windows.
  - The heatmap and WebLogo cells reuse variables created in the WebLogo cell
    (seqs_aligned, L_mode, probs), so run the cells in order.
  - If an older filtered table is present, adjust 'filtered_name' in the
    configuration cell accordingly.


--------------------------------------------------------------------------------
6. LIBRARY DESIGN SETTINGS
--------------------------------------------------------------------------------
All positions are 0-based.

    length_criteria     expected peptide length
    ptm_positions       exact set of positions that must carry a modification
    specific_letters    fixed residues, paired element-wise with
    specific_positions  the positions of those fixed residues
    allowed_letters     residues permitted at the variable positions
    allowed_positions   the variable positions
    alc_threshold       minimum PEAKS ALC (%) score
    amino_acids_to_track  residues displayed in the heatmaps

Example (library Lib24, as provided in the notebook):

    length_criteria      = 5
    ptm_positions        = [0, 4]
    specific_letters     = ['K']
    specific_positions   = [4]
    alc_threshold        = 0
    allowed_letters      = {'W', 'R', 'N', 'D'}
    allowed_positions    = {0, 1, 2, 3}
    amino_acids_to_track = ['W', 'R', 'N', 'D', 'K']

For a different library, only this cell has to be edited; the rest of the
notebook is design-agnostic.


--------------------------------------------------------------------------------
7. OUTPUT FILES
--------------------------------------------------------------------------------
Inside each run sub-folder:

    filtered-peptides-allowedletters.csv
        de novo entries matching the library design
    peptide_binding_scores.csv
        per-peptide ALC (%), Area, Count and BindingScore

Inside the base directory:

    binding_score_comparison_full.csv
        merged replicates with Control_1..3, Sample_1..3, Sum_Control,
        Sum_Sample, Rel_Enrichment, Log2_Enrichment, p-value, Neg_Log10_P
    volcano_red_points.csv
        significantly enriched peptides (p < 0.2 and Rel_Enrichment > 2)
    red_peptides_aa_frequency_percent.csv
        amino-acid frequency table (%) by position for the enriched peptides
    scatter_colored_above_2.png
    volcano_with_labels.png
    volcano_with_labels_journal.png
    volcano_plot.html
    weblogo_red_peptides.png
    weblogo_red_peptides_pale.png
    heatmap_red_peptides_row_normalized_swapped.png
    heatmap_red_peptides_nfold_journal_style.png


--------------------------------------------------------------------------------
8. REPRODUCIBILITY NOTES
--------------------------------------------------------------------------------
  - Figures are written as PNG at 300-600 dpi; the interactive volcano plot is
    written as a self-contained HTML file.
  - Sequence logos and heatmaps use only the enriched peptides whose length
    equals the modal length of that set, so that all sequences are aligned.
  - The N-fold deviation heatmap compares the observed residue frequency at each
    variable position with the frequency expected for an unbiased library
    (1 / number of allowed residues at that position). Values > 1 indicate
    over-representation; cells exactly equal to 1 are shaded light grey.


--------------------------------------------------------------------------------
9. CITATION
--------------------------------------------------------------------------------
If you use this code, please cite the corresponding manuscript.
================================================================================