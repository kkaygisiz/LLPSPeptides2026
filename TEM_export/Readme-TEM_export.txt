TEM Data to PNG Converter

Convert Thermo Scientific .emd (Electron Microscopy Data) files to PNG images.

1. Input Data Format
    File type: .emd (Electron Microscopy Data format)
    Source: Thermo Scientific TEM/SEM instruments

2. Data Organization
Place all .emd files in the ./data/input/ folder:

python script
data/input/
├── sample1.emd
├── sample2.emd
└── sample3.emd

3. Output Format
Each .emd file generates one PNG image with the same filename, e.g.:
sample1.emd	sample1.png
sample2.emd	sample2.png
sample3.emd	sample3.png

PNG files are saved in the same folder as input files (./data/input/)