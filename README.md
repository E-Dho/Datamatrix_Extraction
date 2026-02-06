This project makes use of the EIGENSOFT Package and implements several additional functions to the smartpca that allow for the extraction of the matrices X and XTX with (individuals × SNP) and (individuals × individuals), respectively, and functions for sparse matrix storage and sparsity analysis.

## Matrix Output Functions

### dumpx: Writes the X matrix (individuals × SNPs) to a file
  **Usage**: Set `xoutname:` parameter in parameter file to specify output file
  
  **Format Options**:
  - `xoutformat: csv` (default): Writes matrix as CSV file
    - First row contains SNP IDs, first column contains individual IDs
    - All matrix values are written (dense format)
  - `xoutformat: sparse`: Writes matrix in COO (Coordinate) sparse format
    - Creates multiple files:
      - `{xoutname}.sparse`: Main file with non-zero values (format: `row_index col_index value`)
      - `{xoutname}.ind`: Individual IDs (one per line, order corresponds to row indices)
      - `{xoutname}.snp`: SNP IDs (one per line, order corresponds to column indices)
      - `{xoutname}.info`: Metadata file (format, dimensions, number of non-zeros, sparsity ratio)
    - Only values with absolute value >= epsilon (default: 1e-10) are stored
    - Ideal for sparse matrices (e.g., aDNA data with high missingness)
    - Significantly reduces file size for sparse data (e.g., 99.98% sparsity reduces ~20GB to ~90KB)
  
  **Mode**: Set `xonly: YES` to only write X matrix and exit (skips PCA computation)

### dumpxtx: Writes the XTX matrix (individuals × individuals) to a CSV file
  **Usage**: Set `xtxoutname:` parameter in parameter file to specify output file
  
  **Format**: CSV file with headers
  - First row and column contain individual IDs
  - Symmetric matrix (individuals × individuals)
  
  **Mode**: Set `xtxonly: YES` to only write XTX matrix and exit (skips PCA computation)

## Sparsity Analysis Functions

### analyze_sparsity_partial: Analyzes sparsity of X matrix during computation
  **Usage**: Set `sparsity_check: YES` in parameter file to enable sparsity analysis
  
  **Parameters**:
  - `sparsity_check: YES`: Enable sparsity analysis (default: NO)
  - `max_cols_for_sparsity: <number>`: Number of columns to analyze (default: 1000)
    - Analysis is performed after this many columns have been computed
    - Allows early assessment without computing full matrix
  - `sparsity_only: YES`: Exit after sparsity analysis (default: NO)
    - Only computes first `max_cols_for_sparsity` columns
    - Significantly reduces computation time and memory usage
    - Useful for quick sparsity assessment on large datasets
  
  **Output**: Prints detailed statistics to stdout:
  - Number of exact zeros and near-zeros
  - Sparsity ratio (percentage of zero values)
  - Min/Max/Average of non-zero values
  - Value distribution across different ranges
  - Estimated statistics for full matrix
  
  **Example**: For aDNA data with 99.98% sparsity, this function helps determine
  whether sparse storage format should be used to save disk space.

## Example Parameter File Usage

```
# Write X matrix in sparse format
xoutname: output_X
xoutformat: sparse

# Analyze sparsity after 1000 columns and exit
sparsity_check: YES
max_cols_for_sparsity: 1000
sparsity_only: YES

# Write XTX matrix
xtxoutname: output_XTX
```
