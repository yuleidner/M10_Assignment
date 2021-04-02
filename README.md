# This contains the M10 Assignment 

In this assignment, I followed a few steps to modify a jupyter notebook to scrape some data from the web.

##Link to notebook##
## https://github.com/yuleidner/M10_Assignment/blob/master/scripts/M10_webscraper_assignment.ipynb

Steps:
1) I modified the files included to point to my local chrome driver.
2) I modified the pathname to an s3 bucket that I created
3) I loaded the dataframe into a csv in my s3 bucket
4) I added a cell to remove the first row in the df, which was empty
5) I uploaded a new csv into the s3 bucket


## Links to csv's in s3##
## 1st s3://m10-database-update-bucket-v2/charities_bureau_scrape_20210402164723.csv
## 2nd s3://m10-database-update-bucket-v2/charities_bureau_scrape_20210402165040.csv