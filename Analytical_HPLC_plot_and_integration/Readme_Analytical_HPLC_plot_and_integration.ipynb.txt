This script generates png image exports for UPLC measurements.

1. Create this folder structure:

your-project/
├── process_UPLC_data.py
├── data/
│   ├── input/           (put your sample CSV files here)
│   └── blanks/          (put your blank CSV files here)
└── results/             (output files go here)

2. Place your CSV files in the appropriate folders
 CSV files are exported from Agilent ChemStation software and contain
  - Column 1: Time (in minutes)
  - Column 2: Absorbance (in AU - Absorbance Units)

### File Naming Convention
- Files ending in `_DAD1A.CSV` → 214 nm wavelength
- Files ending in `_DAD1B.CSV` → 280 nm wavelength

3. Run the script
4. Expected output is PNG images for each sample:
- `Filename.png` - Full chromatogram (entire time range)
- `Filename_SHORT.png` - Zoomed view (e.g. 3-15 min time range)
