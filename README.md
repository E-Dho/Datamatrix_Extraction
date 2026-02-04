This project makes use of the EIGENSOFT Package and implements two further functions to the smartpca that allow for the extraction of the matrices X and XTX with (individuals x SNP) and (individuals x individuals), respectively.

- dumpx: Writes the X matrix (individuals × SNPs) to a CSV file
  Usage: Set 'xoutname:' parameter in parameter file to specify output file
  Mode: Set 'xonly: YES' to only write X matrix and exit (skips PCA computation)

- dumpxtx: Writes the XTX matrix (individuals × individuals) to a CSV file
  Usage: Set 'xtxoutname:' parameter in parameter file to specify output file
  Mode: Set 'xtxonly: YES' to only write XTX matrix and exit (skips PCA computation)

Both functions write CSV files with headers:
- dumpx: First row contains SNP IDs, first column contains individual IDs
- dumpxtx: First row and column contain individual IDs
