# OMOP Code Snippets: R, STATA, and SPSS

This repository provides code snippets for basic tasks in OMOP CDM (Observational Medical Outcomes Partnership Common Data Model) using **R**, **STATA**, and **SPSS**. These examples include tasks like reading data, joining tables, and filtering based on specific criteria. Adjust file paths and variable names according to your dataset and needs.

### Additional Resources:
- For more examples of code snippets from All of Us, check out: [All of Us Workbench Snippets](https://github.com/all-of-us/workbench-snippets/tree/main/dataset-snippets).

---

## Table of Contents

- [R Code Snippets](#r-code-snippets)
  - [Reading CSV Files into R Data Frames](#reading-csv-files-into-r-data-frames)
  - [Joining Tables Based on `person_id`](#joining-tables-based-on-person_id)
  - [Joining the Person-Visit Table with the Condition Occurrence Table](#joining-the-person-visit-table-with-the-condition-occurrence-table)
  - [Search by a List of `person_ids`](#search-by-a-list-of-person_ids)
  - [Search by a Specific Condition Concept Code](#search-by-a-specific-condition-concept-code)
  - [Search by a Date Range](#search-by-a-date-range)
  
- [STATA Code Snippets](#stata-code-snippets)
  - [Reading CSV Files into STATA](#reading-csv-files-into-stata)
  - [Joining Tables Based on `person_id`](#joining-tables-based-on-person_id-1)
  - [Joining the Person-Visit Table with the Condition Occurrence Table](#joining-the-person-visit-table-with-the-condition-occurrence-table-1)
  - [Search by a List of `person_ids`](#search-by-a-list-of-person_ids-1)
  - [Search by a Specific Condition Concept Code](#search-by-a-specific-condition-concept-code-1)
  - [Search by a Date Range](#search-by-a-date-range-1)
  
- [SPSS Code Snippets](#spss-code-snippets)
  - [Reading CSV Files into SPSS](#reading-csv-files-into-spss)
  - [Joining Tables Based on `person_id`](#joining-tables-based-on-person_id-2)
  - [Joining the Person-Visit Table with the Condition Occurrence Table](#joining-the-person-visit-table-with-the-condition-occurrence-table-2)
  - [Search by a List of `person_ids`](#search-by-a-list-of-person_ids-2)
  - [Search by a Specific Condition Concept Code](#search-by-a-specific-condition-concept-code-2)
  - [Search by a Date Range](#search-by-a-date-range-2)

---

## R Code Snippets

### Reading CSV Files into R Data Frames
```r
# Read the CSV files into R data frames
person_df <- read.csv("path_to_person_table.csv", header=TRUE, stringsAsFactors=FALSE)
visit_occurrence_df <- read.csv("path_to_visit_occurrence_table.csv", header=TRUE, stringsAsFactors=FALSE)
condition_occurrence_df <- read.csv("path_to_condition_occurrence_table.csv", header=TRUE, stringsAsFactors=FALSE)
```

### Joining Tables Based on `person_id`
```r
# Join person with visit_occurrence on 'person_id'
person_visit_df <- merge(person_df, visit_occurrence_df, by="person_id")
```

### Joining the Person-Visit Table with the Condition Occurrence Table
```r
# Join the person-visit result with condition_occurrence on both 'person_id' and 'visit_occurrence_id'
full_df <- merge(person_visit_df, condition_occurrence_df, by=c("person_id", "visit_occurrence_id"))
```

### Search by a List of `person_ids`
```r
# Define a list of person_ids to search for
search_person_ids <- c(1, 2, 3, 4, 5)

# Filter the data frame to only include rows with person_ids in the list
filtered_by_person_df <- subset(full_df, person_id %in% search_person_ids)
```

### Search by a Specific Condition Concept Code
```r
# Define a specific condition concept code to search for
search_condition_concept_id <- 1234567

# Filter the data frame to only include rows with the specified condition concept code
filtered_by_condition_df <- subset(full_df, condition_concept_id == search_condition_concept_id)
```

### Search by a Date Range
```r
# Define a date range to search for
start_date <- as.Date("2020-01-01")
end_date <- as.Date("2020-12-31")

# Filter the data frame to only include rows within the date range
filtered_by_date_df <- subset(full_df, visit_start_date >= start_date & visit_start_date <= end_date)
```

---

## STATA Code Snippets

### Reading CSV Files into STATA
```stata
* Read the CSV files into STATA
import delimited "path_to_person_table.csv", clear
rename (old_name1 old_name2 old_name3) (person_id gender birth_year)
save "person.dta", replace

import delimited "path_to_visit_occurrence_table.csv", clear
rename (old_name1 old_name2 old_name3) (person_id visit_occurrence_id visit_start_date)
save "visit_occurrence.dta", replace

import delimited "path_to_condition_occurrence_table.csv", clear
rename (old_name1 old_name2 old_name3) (person_id visit_occurrence_id condition_concept_id)
save "condition_occurrence.dta", replace
```

### Joining Tables Based on `person_id`
```stata
* Join person with visit_occurrence on 'person_id'
use "person.dta", clear
merge 1:m person_id using "visit_occurrence.dta"
save "person_visit.dta", replace
```

### Joining the Person-Visit Table with the Condition Occurrence Table
```stata
* Join the person-visit result with condition_occurrence on both 'person_id' and 'visit_occurrence_id'
use "person_visit.dta", clear
merge 1:m person_id visit_occurrence_id using "condition_occurrence.dta"
save "full_data.dta", replace
```

### Search by a List of `person_ids`
```stata
* Define a list of person_ids to search for
local search_person_ids 1 2 3 4 5

* Filter the data frame to only include rows with person_ids in the list
use "full_data.dta", clear
keep if inlist(person_id, `search_person_ids')
save "filtered_by_person.dta", replace
```

### Search by a Specific Condition Concept Code
```stata
* Define a specific condition concept code to search for
local search_condition_concept_id 1234567

* Filter the data frame to only include rows with the specified condition concept code
use "full_data.dta", clear
keep if condition_concept_id == `search_condition_concept_id'
save "filtered_by_condition.dta", replace
```

### Search by a Date Range
```stata
* Define a date range to search for
local start_date = date("2020-01-01", "YMD")
local end_date = date("2020-12-31", "YMD")

* Filter the data frame to only include rows within the date range
use "full_data.dta", clear
keep if visit_start_date >= `start_date' & visit_start_date <= `end_date'
save "filtered_by_date.dta", replace
```

---

## SPSS Code Snippets

### Reading CSV Files into SPSS
```spss
* Read the CSV files into SPSS
GET DATA /TYPE=TXT /FILE="path_to_person_table.csv" /DELCASE=LINE /DELIMITERS="," 
  /ARRANGEMENT=DELIMITED /FIRSTCASE=2 /IMPORTCASE=ALL /VARIABLES=person_id F1.0 gender A8 birth_year F4.0.
EXECUTE.

GET DATA /TYPE=TXT /FILE="path_to_visit_occurrence_table.csv" /DELCASE=LINE /DELIMITERS="," 
  /ARRANGEMENT=DELIMITED /FIRSTCASE=2 /IMPORTCASE=ALL /VARIABLES=person_id F1.0 visit_occurrence_id F1.0 visit_start_date DATE9.
EXECUTE.

GET DATA /TYPE=TXT /FILE="path_to_condition_occurrence_table.csv" /DELCASE=LINE /DELIMITERS="," 
  /ARRANGEMENT=DELIMITED /FIRSTCASE=2 /IMPORTCASE=ALL /VARIABLES=person_id F1.0 visit_occurrence_id F1.0 condition_concept_id F1.0.
EXECUTE.
```

### Joining Tables Based on `person_id`
```spss
* Join person with visit_occurrence on 'person_id'
MATCH FILES /FILE=* 
  /TABLE='path_to_visit_occurrence_table.sav' 
  /BY person_id.
EXECUTE.
```

### Joining the Person-Visit Table with the Condition Occurrence Table
```spss
* Join the person-visit result with condition_occurrence on both 'person_id' and 'visit_occurrence_id'
MATCH FILES /FILE=* 
  /TABLE='path_to_condition_occurrence_table.sav' 
  /BY person_id visit_occurrence_id.
EXECUTE.
```

### Search by a List of `person_ids`
```spss
* Define a list of person_ids to search for
DATASET DECLARE person_ids.
BEGIN DATA.
1
2
3
4
5
END DATA.
EXECUTE.

* Filter the data frame to only include rows with person_ids in the list
MATCH FILES /FILE=* /TABLE=person_ids /RENAME (VAR1=person_id) /BY person_id.
SELECT IF $CASENUM NE 0.
EXECUTE.
```

### Search by a Specific Condition Concept Code
```spss
* Define a specific condition concept code to search for
COMPUTE search_condition_concept_id = 1234567.

* Filter the data frame to only include rows with the specified condition concept code
SELECT IF condition_concept_id = search_condition_concept_id.
EXECUTE.
```

### Search by a Date Range
```spss
* Define a date range to search for
COMPUTE start_date = DATE.DMY(1,1,2020).
COMPUTE end_date = DATE.DMY(31,12,2020).

* Filter the data frame to only include rows within the date range
SELECT IF visit_start_date >= start_date AND visit_start_date <= end_date.
EXECUTE.
```

---

