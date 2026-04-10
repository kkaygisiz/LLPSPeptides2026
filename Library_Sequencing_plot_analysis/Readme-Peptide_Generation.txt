Peptide Library Generator

Generate combinatorial peptide libraries with fixed and variable amino acid positions.

Generated peptide list is saved as csv. 

Define a peptide pattern where:
    Fixed positions (e.g., K) = same amino acid in all sequences
    Variable positions (e.g., X) = multiple possible amino acids

Example
fixed_pattern = "XXK"
variable_positions = {
    'X': ['A', 'R', 'N', 'D', 'E', 'Q', 'G', 'H', 'L', 'K', 'M', 'F', 'P', 'S', 'T', 'W', 'Y', 'V']
}

Generates: 18^4 × 1 = 324 peptides
