This script plots data obtained from Agilent LCMS. The original data format is converted to mzData.xml using Agilent Mass Hunter Software to process using this script.

1. Input Data Format
    mzData.xml (exported from Agilent MassHunter Software)
    Source: LC-MS chromatography analysis
    Contains: MS1 spectra, retention time information

2. Data Organization

Place your .mzdata.xml files in the ./data/input/ folder:

python script

data/input/
├── sample1.mzdata.xml
├── sample2.mzdata.xml
└── sample3.mzdata.xml

3. Retention Time Calibration

    Retention time is calculated from scan index
    Calibration: 900 scans = 10 minutes of analysis
    Time per scan ≈ 0.01111 minutes

4. Output Format

The script generates PNG images for each sample, e.g.:
MS1 Spectrum	Full mass spectrum from first scan	*_MS1_Spectrum.png
TIC (Full)	Total Ion Current (0-10 min)	*_TIC.png
TIC (Short)	Total Ion Current zoomed (3-9 min)	*_TIC_SHORT.png
BPC (Full)	Base Peak Current (0-10 min)	*_BPC.png
BPC (Short)	Base Peak Current zoomed (3-9 min)	*_BPC_SHORT.png

All plots are saved to ./data/input/ (same folder as input files)


Each .mzdata.xml file generates one interactive HTML file with the same filename, e.g.:
sample1.mzdata.xml	sample1_explorer.html	Interactive explorer for sample1
sample2.mzdata.xml	sample2_explorer.html	Interactive explorer for sample2

HTML files are saved in the same folder as input files (./data/input/)
The HTML files contain interactive features
Left Panel: Total Ion Chromatogram (TIC)
	 Click-to-select MS1 scans
Right Panel: MS1 Spectrum
Annotation:
    Click peaks in MS1 spectrum
    List of all annotations for current scan
    Click individual delete buttons or "Clear All"

HTML is a self-contained interactive HTML (no external data files), interactivity is provided by Plotly.js.
Browser compatibility: Works in all modern browsers (Chrome, Firefox, Safari, Edge)

