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
I coded the solution in Pyspark. The basic approach is as follows: 
1. Read the files  as a Spark dataframe.
2. First, I counted the total number of rows (4820022).
3. In order to do a basic "fuzzy" approach, I removed all commas, spaces, and periods from the 'recipient_names' column. Before doing this, there were 47266 unique recipients. Afterwards, there are 45996. Furthermore, I noticed some ampersands, and thought that this might cause some differences (e.g., 'M & J' vs. 'M AND J'). So I first removed ' AND ' (spaces to avoid removing parts of company names ('rAND corporation'). The result is 45961 unique recipient names, so maybe this step is not so important (I did not check if ' AND ' even occurs in place of '&').
4. Comparing domestic

