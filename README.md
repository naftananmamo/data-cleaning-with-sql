Data Cleaning: Layoffs Dataset
Overview

This SQL script performs data cleaning and standardization on the layoffs dataset. The goal is to remove duplicates, standardize values, handle nulls, and prepare the dataset for further analysis.

The cleaned dataset is stored in a new table called layoffs_staging2.

Steps in the Script
1. Remove Duplicates

Create a staging table layoffs_staging to work on a copy of the original data.

Identify duplicate rows based on all columns using a ROW_NUMBER() window function.

Insert unique rows into layoffs_staging2 and remove duplicates.

SQL Highlights:

ROW_NUMBER() OVER(PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions) AS row_num

2. Standardize Data

Remove leading and trailing spaces in the company column.

Standardize industry names, e.g., all variations of Crypto% → 'Crypto'.

Fix country names, e.g., 'United Sates.' → 'United States'.

Convert the date column from text to DATE type for proper date handling.

SQL Highlights:

UPDATE layoffs_staging2 SET company = TRIM(company);
UPDATE layoffs_staging2 SET industry = 'Crypto' WHERE industry LIKE 'Crypto%';
ALTER TABLE layoffs_staging2 MODIFY COLUMN `date` DATE;

3. Handle Null or Blank Values

Identify rows where total_laid_off and percentage_laid_off are NULL.

Replace empty strings in industry with NULL.

Use non-null values from other rows of the same company/location to fill missing industry values.

SQL Highlights:

UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2 ON t1.company = t2.company AND t1.location = t2.location
SET t1.industry = t2.industry
WHERE t1.industry IS NULL AND t2.industry IS NOT NULL;

4. Remove Unnecessary Columns or Rows

Delete rows where both total_laid_off and percentage_laid_off are NULL.

Remove helper columns such as row_num that are no longer needed.

SQL Highlights:

DELETE FROM layoffs_staging2 WHERE total_laid_off IS NULL AND percentage_laid_off IS NULL;
ALTER TABLE layoffs_staging2 DROP COLUMN row_num;
