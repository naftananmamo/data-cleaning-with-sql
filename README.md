Layoffs Dataset Cleaning
Overview

This SQL script cleans and standardizes the layoffs dataset. The cleaning process removes duplicates, fixes inconsistent values, handles nulls, and prepares the data for analysis. The final cleaned dataset is stored in layoffs_staging2.

Steps Performed

Remove Duplicates

A staging table layoffs_staging is created from the original layoffs table.

Duplicate rows are identified using a ROW_NUMBER() window function and removed in layoffs_staging2.

Standardize Data

Trim whitespace from company names.

Standardize industry names (e.g., all variations of Crypto% → Crypto).

Correct country names (e.g., 'United Sates.' → 'United States').

Convert date column from text to proper DATE type.

Handle Null or Blank Values

Identify rows with missing total_laid_off or percentage_laid_off.

Replace empty industry values with NULL.

Fill missing industry values by joining with other rows of the same company and location that have valid industry data.

Remove Unnecessary Columns or Rows

Delete rows where both total_laid_off and percentage_laid_off are NULL.

Drop helper columns like row_num that are no longer needed.

Outcome

The layoffs_staging2 table contains a clean, standardized dataset ready for analysis, free from duplicates, inconsistent values, and unnecessary data.
