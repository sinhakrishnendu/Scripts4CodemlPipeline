This project performs likelihood ratio tests (LRT) on codon models using two different files:
lnL_np_extractor.sh and LRT_test.ipynb are to be used successively. Consider the .sh file as the initial file of the pipeline whose output will be used as the input for the .ipynb.
Shell Script (scrape_lnL_values.sh): This script scrapes multiple folders to find output.txt files, extracts specific log-likelihood (lnL) values and number of parameters (np), and saves the data in a CSV file for further analysis.
Jupyter Notebook (lrt_test.ipynb): This notebook performs LRT calculations using the extracted data, compares the test statistic to critical chi-square values, and outputs the results into an Excel file.
