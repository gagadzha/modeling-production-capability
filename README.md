# modeling-production-capability
 This exercise involves analyzing production data from various factories across multiple cities. The goal is to develop a model that can probabilistically predict future production amounts. The given data is for two factories: Brussels (BRU) and Stockholm (STO)

 # Folder structure 
 ## data folder
  In the daily-production folder we have two directories with JSON data from both factories which we will use as input for our analysis. 

 ## output
  I created an ouput folder which writes the excel files with the various data we want to output


# Code structure: 
## 1. Reading raw data
 The `read_json_data` function in this code is designed to read JSON files from a specified folder and convert them into a pandas DataFrame. If a dataset name (like 'BRU' or 'STO') is provided as a parameter, it reads files from the corresponding subfolder within 'data/daily_production'. If no name is provided, it prompts the user to input a dataset name. The function then iterates through all JSON files in the specified subfolder, reads each file, and appends its contents to a list. This list of dictionaries is finally converted into a DataFrame, which is returned by the function. If the specified folder does not exist, it informs the user.

