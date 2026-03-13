# SNP Analysis in R

This repository contains an R-based replication and extension of a UNIX SNP-processing workflow using the datasets `fang_et_al_genotypes.txt` and `snp_position.txt`. The project loads, cleans, reshapes, and joins genotype and SNP position data, generates chromosome-specific output files for maize and teosinte samples, and visualizes SNP distribution and genotype composition.

## Repository contents

- `SNP_Analysis_in_R.Rmd`  
  Main R Markdown file containing all code, workflow descriptions, and visualizations.

- `SNP_Analysis_in_R.html`  
  Knitted output report generated from the R Markdown file.

- `fang_et_al_genotypes.txt`  
  Genotype dataset containing SNP calls for maize, teosinte, and Tripsacum individuals.

- `snp_position.txt`  
  SNP annotation dataset containing SNP identifiers, chromosome numbers, genomic positions, and additional metadata.

- `output/`  
  Folder containing chromosome-specific output files for maize and teosinte.

## Analysis overview

### Part I: Data inspection and processing

The analysis involves loading the two input datasets into R and inspecting their structure. The SNP position dataset is reduced to three required fields: `SNP_ID`, `Chromosome`, and `Position`. Chromosome values are restricted to chromosomes 1–10, and rows with missing chromosome or position values are removed.

The genotype dataset is reshaped from wide to long format using `pivot_longer()`, so that SNP markers can be joined to the SNP position table by `SNP_ID`.

The joined dataset is then split into:

- **Maize**: `ZMMIL`, `ZMMLR`, `ZMMMR`
- **Teosinte**: `ZMPBA`, `ZMPIL`, `ZMPJA`

For each taxon, 20 chromosome-specific files are generated:

- 10 files with SNPs sorted by increasing position and missing data encoded as `?`
- 10 files with SNPs sorted by decreasing position and missing data encoded as `-`

This produces **40 output files total**. These are present in the output files.

### Part II: Visualization

The report includes several visualizations created with `ggplot2`:

- overall SNP marker counts per chromosome
- SNP marker counts per chromosome for maize versus teosinte
- polymorphic SNP counts per chromosome for maize versus teosinte
- proportions of homozygous, heterozygous, and missing genotype calls per sample
- proportions of homozygous, heterozygous, and missing genotype calls by group

## Output file structure

The generated files are organized as follows:

```text
output/
├── maize/
│   ├── inc/
│   └── dec/
└── teosinte/
    ├── inc/
    └── dec/
```

Each output file contains:

1. `SNP_ID`
2. `Chromosome`
3. `Position`
4. genotype columns for all maize or teosinte samples in that subset

## How to run the analysis

1. Place `fang_et_al_genotypes.txt` and `snp_position.txt` in the same folder as the `.Rmd` file.
2. Open `SNP_Analysis_in_R.Rmd` in RStudio.
3. Run the document from a clean environment or knit the file to HTML.
4. The script will automatically generate the `output/` folder and all 40 chromosome files.

## Required R packages

This analysis uses:

- `tidyverse`

The package can be installed with:

```r
install.packages("tidyverse")
```

## Notes

- Missing genotype calls in the original data are coded as `?/?`.
- In increasing-order output files, missing values are recoded as `?`.
- In decreasing-order output files, missing values are recoded as `-`.
- The script includes checks to verify file count, column order, and SNP sorting direction which can be run to check on the analysis.