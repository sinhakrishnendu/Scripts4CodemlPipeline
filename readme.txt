lnL_np_extractor.sh and LRT_test.ipynb are to be used successively. Consider the .sh file as the initial file of the pipeline whose output will be used as the input for the .ipynb.

Here are detailed instructions on how to use the Python script `LRT_test.py` for performing likelihood ratio tests (LRT) on codon models, including the branch-site and two-ratio branch models. These instructions assume you have a basic understanding of Python and have the required dependencies installed.

### **Purpose of the Script:**
The script performs LRTs to compare the likelihood of different evolutionary models for codon usage. Specifically, it compares:
1. **Branch-site models** (_BS vs. _BS_NULL):** This tests whether there is positive selection in specific branches of the phylogenetic tree.
2. **Two-ratio branch models** (_B vs. M0):** This tests whether specific branches have different evolutionary rates compared to the entire phylogenetic tree (M0 model).

### **What the Script Does:**
1. Reads a CSV file containing the likelihood values (lnL) and the number of parameters (np) from different codon models (such as branch-site models and two-ratio branch models).
2. For the **branch-site model**, it calculates the LRT by comparing the lnL values of the **_BS model** with its corresponding **_BS_NULL model**.
3. For the **two-ratio branch model**, it calculates the LRT by comparing the lnL values of the **_B model** with the **M0 model**.
4. For both LRTs, the script compares the computed test statistics with critical chi-square values at multiple significance levels: 0.05, 0.005, 0.0005, 0.00025, and 0.00005.
5. Outputs the results to an Excel file with two separate sheets: one for branch-site LRT results and one for branch model LRT results.

### **Prerequisites:**
- **Python** (version 3.7 or higher).
- The following Python libraries need to be installed:
  - `pandas` for data manipulation and Excel writing.
  - `numpy` for numerical calculations.
  - `scipy` for statistical functions (chi-square distribution).
  - `openpyxl` for Excel file handling.

You can install these dependencies using the following command:
```bash
pip install pandas numpy scipy openpyxl
```

### **Input File Requirements:**
The script expects a CSV file (`lnL_np_values.csv`) containing the following columns:
- **Folder**: The name of the folder, which encodes the gene name and the model type (e.g., `Glyma.02G007500.1_BS`, `Glyma.02G007500.1_BS_NULL`, `Glyma.02G007500.1_B`, or `M0`).
- **lnL**: The log-likelihood value of the model.
- **np**: The number of parameters used in the model.

#### Example of the CSV file:
```
Folder,lnL,np
Glyma.02G007500.1_BS,-31765.565240,71
Glyma.02G007500.1_BS_NULL,-31770.102300,69
Glyma.02G007500.1_B,-31760.459870,65
M0,-31762.985610,60
```

### **How to Use the Script:**

1. **Prepare Your Input File:**
   - Ensure your input CSV file is named `lnL_np_values.csv`.
   - Make sure the folder names in the `Folder` column include the gene name and the model type (e.g., `Glyma.02G007500.1_BS`, `Glyma.02G007500.1_BS_NULL`, `Glyma.02G007500.1_B`, or `M0`).

2. **Run the Script:**
   - Save the Python script as `lrt_test.py`.
   - Ensure that `lnL_np_values.csv` is in the same directory as the Python script.
   - Run the script using Python:
     ```bash
     python lrt_test.py
     ```

3. **Output:**
   - The script will generate an Excel file named `lrt_results_multiple_significance.xlsx`.
   - The Excel file will contain two sheets:
     - **LRT for branchsite model**: This sheet shows the LRT comparison for _BS vs _BS_NULL models.
     - **LRT for branch model**: This sheet shows the LRT comparison for _B vs M0 models.

### **Details of the Output File:**
The Excel file will contain the following columns:
1. **Gene**: The name of the gene being tested.
2. **lnL_BS** or **lnL_B**: The log-likelihood of the _BS model (for branch-site) or _B model (for branch model).
3. **lnL_NULL** or **lnL_M0**: The log-likelihood of the null model (_BS_NULL for branch-site or M0 for branch model).
4. **LRT**: The calculated likelihood ratio test statistic.
5. **Critical_Chi2_[significance level]**: The chi-square critical value at each significance level.
6. **Significant_[significance level]**: Whether the LRT result is statistically significant at the given significance level ("Yes" or "No").

#### Example of **LRT for branchsite model** sheet:
| Gene              | lnL_BS     | lnL_NULL  | LRT     | Critical_Chi2_0.05 | Significant_0.05 | Critical_Chi2_0.005 | Significant_0.005 |
|-------------------|------------|-----------|---------|---------------------|------------------|----------------------|-------------------|
| Glyma.02G007500.1 | -31765.565 | -31770.102| 8.3     | 3.841               | Yes              | 7.879                | Yes               |

#### Example of **LRT for branch model** sheet:
| Gene              | lnL_B     | lnL_M0    | LRT     | Critical_Chi2_0.05 | Significant_0.05 | Critical_Chi2_0.005 | Significant_0.005 |
|-------------------|-----------|-----------|---------|---------------------|------------------|----------------------|-------------------|
| Glyma.02G007500.1 | -31760.460| -31762.986| 5.052   | 3.841               | Yes              | 7.879                | No                |

### **Explanation of Calculations:**

1. **LRT Calculation**:
   \[
   \text{LRT} = 2 \times (\text{lnL of Null Model} - \text{lnL of Tested Model})
   \]
   - For **branch-site models**, the null model is the `_BS_NULL` model, and the tested model is the `_BS` model.
   - For **branch models**, the null model is the `M0` model, and the tested model is the `_B` model.

2. **Chi-square Comparison**:
   - The LRT statistic is compared with critical chi-square values for 1 degree of freedom.
   - The script checks significance at various levels: 0.05, 0.005, 0.0005, 0.00025, and 0.00005.

### **Troubleshooting:**

- **Missing Entries:** If a gene does not have both a branch-site model and a null model (or a branch model and M0 model), the script will print a message like:
  ```
  Missing BS or BS_NULL data for gene: Glyma.02G007500.1
  ```

- **Index Errors:** Ensure that the folder names in the CSV file are properly formatted and include the gene name with the appropriate suffix (_BS, _BS_NULL, _B, or M0).

### **Modifications:**
- You can change the critical significance levels or add new ones by modifying the `significance_levels` list:
  ```python
  significance_levels = [0.05, 0.005, 0.0005, 0.00025, 0.00005]
  ```

- If you want to change the input or output file names, modify these lines:
  ```python
  df = pd.read_csv('lnL_np_values.csv')  # Input CSV file
  with pd.ExcelWriter('lrt_results_multiple_significance.xlsx', engine='openpyxl') as writer:  # Output Excel file
  ```

By following these instructions, anyone should be able to use and understand the purpose of this script, making it easier to replicate or extend the analysis in the future.
