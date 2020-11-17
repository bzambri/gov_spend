# Tamr Coding Challenge: Government spending data analysis submission for Brian Zambri (brian.zambri@gmail.com)

## Table of Contents
1. [Problem](README.md#problem)
1. [Approach](README.md#steps-to-submit-your-solution)
1. [Testing the code](README.md#testing-your-code)
1. [Instructions](README.md#instructions)

## Problem
This a real world problem in distinguishing suppliers: how many distinct suppliers are there in the USA spend dataset?
This is a dataset published by [USASpending.gov](http://usaspending.gov), that covers every “federal contract, grant, loan, and other financial assistance awards of more than $25,000”. The full dataset has millions of records and many columns, but can be reduced in size by selecting the interesting columns and doing an exact match on those. Interesting numbers to see:
* Number of initial records.
* Number of records after reducing based on exact supplier/vendor matches.
* Number of records after reducing further based on “fuzzy” matching criteria. This should
group together records where the same supplier had slightly different names (such as “W.W. Grainger” and “WW Grainger” or “IBM” and “International Business Machines”). Some of the fuzzy matching logic might also mean matching across columns such as matching vendorname with vendoralternatename. Fields like phonenumber, streetaddress, city, state, and dunsnumber can also provide useful signals.
* Some measure(s) of accuracy with explanations.

Once you have grouped together suppliers what questions does this enable you to answer that you couldn’t answer as well/accurately before performing this curation?

Final output should include a short summary of your solution to the problem and what you found in the data (how many suppliers you found), the script you used (python, Hive, or otherwise) in order to generate clusters of suppliers, and any other interesting results/conclusions.

## Approach
I coded the solution in Python (3.8). The basic approach is as follows: 
1. `consumer_complaints.py` opens the input file and calls the functions below.
2. `get_complaints` reads all the complaints and outputs lists containing the financial products, the years, and the companies of all the complaints. Within `get_complaints`, `validator.complaint_is_valid` makes sure that none of these three fields are empty, and that the first four characters of 'Date received' are numeric. The second check is done because the first four characters should be the year of the complaint (YYYY), which is necessary to complete the task.
3. `file_complaints` takes the information from step 2 and writes the desired information to `output/report.csv`. Within `file_complaints`, the function `file_product_year` outputs what ends up being each line of `report.csv`. `file_product_year` is called from within `file_complaints` so as to enable multithreading via the `multiprocessing.Pool()` function in Python. Multithreading at the year level speeds up the program by about 33% (based on `test_2`; ~1.5 million complaints) and enables better scaling to large datasets.

## Testing the code
To be honest, I did not have a lot of time to create a bunch of different test inputs. in the `insight_testsuite` directory, `test_1` is the 5-line example file that was given in the coding challenge. `test_2` was the moderate-sized dataset for which the zip file was provided, but I did not include this in my repo because of the large file size, and also the instructions said that you would be using that as a test anyway. Tests 3–5 were used to see that the code does the right thing in the event of a blank 'Date received,' 'Product,' or 'Company' field. They work fine, but I ended up covering this in the unit test, so maybe it's a bit redundant.

I implemented a unit test, which can be run from the top-most level of this repository with the command: 

`python3.8 -m unittest src/test_validator.py`

`test_validator` contains the following tests:
1. Return false if date is blank.
2. Return false if the first four characters of date are nonnumeric.
3. Return false if product is blank.
4. Return false if company is blank.
5. Return true otherwise.

This is intended to make sure that the complaint will be skipped in cases 1–4, and processed in case 5.

## Instructions
Working in the top-most level of this repository, move the desired `complaints.csv` file into `input/` and execute the run script: 

`./run.sh`

Output will be in `output/report.csv`.

