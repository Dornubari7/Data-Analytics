# SQL portfolio
welcome to my data analysis portfolio! this space showcases SQL projects focused on data cleaning, EDA, and advanced sql techniques.
## projects
- [layoffs Analysis](./Layoffs_Analysis)


-- Layoffs Data Cleaning Project (SQL)

-- Project Overview
# in this project i will clean and prepare a rea-world layoffs dataset using SQL. I will tackle common data  ceaning task like

# Removing duplicates
# Standardizing text 
# Handling missing values
# Changing data types 
# Removing unnecessary columns
# Creating new columns for analysis

select *
from layoffs_staging2
limit 10;

-- Before cleaning, understand the dataset 
-- Check table structure
describe layoffs_staging2;

-- check the total number of rows and columns
select count(*) as total_rows
from layoffs_staging2;

-- check for the  distinct and the unicque values in the key column
select distinct industry 
from layoffs_staging2;

select distinct stage
from layoffs_staging2;

-- check for the  null values
select count(stage)
from layoffs_staging2
where stage is null;

select count(*) as null_stage
from layoffs_staging2
where stage is null;

-- check for duplicates
select company, `date`, count(*)
from layoffs_staging2
group by company, `date`
having count(*) > 1;

-- 2. Removing duplicates
delete l1
from layoffs  l1
join layoffs l2
	on l1.company = l2.company
    and l1.date = l2.date
    and l1.id > l2.id; -- keeps the  first entry only]
    
select *
from layoffs_staging2;

select 
	sum(case when company is null then 1 else 0 end) as null_company,
	sum(case when location is null then 1 else 0 end) as null_location,
	sum(case when industry is null then 1 else 0 end) as null_industry,
    sum(case when total_laid_off is null then 1 else 0 end) as null_total_laid_off,
    sum(case when percentage_laid_off is null then 1 else 0 end) as null_total_laid_off,
    sum(case when `date` is null then 1 else 0 end) as null_date,
    sum(case when stage is null then 1 else 0 end) as null_stage,
    sum(case when country is null then 1 else 0 end) as null_country,
    sum(case when funds_raised_millions is null then 1 else 0 end) as null_funds_raised_millions
from layoffs_staging2;

update layoffs_staging2
set company = concat (
upper(substring(company, 1,1)),
lower(substring(company, 2))
);

update layoffs_staging2
set total_laid_off = 1000
where total_laid_off is null;

select *
from layoffs_staging2;

update layoffs_staging2
set percentage_laid_off = 0.06
where percentage_laid_off is null;


update layoffs_staging2
set company = concat (
upper(substring(company, 1,1)),
lower(substring(company, 2))
);

update layoffs_staging2
set industry = trim(industry);

select 
sum(case when industry is null then 1 else 0 end) as missing_industry
from layoffs_staging2;

update layoffs_staging2
set industry = 'unknown'
where industry is null;

select *
from layoffs_staging2;

update layoffs_staging2
set stage = 'unknown'
where stage is null;

-- Create new columns
alter table layoffs_staging2
add column year INT;


update layoffs_staging2
set year = year(`date`);


-- Final Cleaned Dataset;
-- Have no duplicates 
-- stsandsardized texts 
-- no missing values in critical columns
-- proper data type
-- addtional columns for better analysis
