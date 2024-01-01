# Modeling production capability using Pandas and Numpy
This exercise involves analyzing production data from various factories across multiple cities. The goal is to develop a model that can probabilistically predict future production amounts. The given data is for two factories: Brussels (BRU) and Stockholm (STO)

 # Folder structure:
 
 ## data folder
  In the data/daily-production folder we have two directories with JSON data from both factories which we will use as input for our analysis. 

 ## output
  I created an data/output folder which writes the excel files with the various data we want to output

 ## docs
  Files with the original assignment in markdown format

 ## trials
  Here are some Jupyter notebooks with trials of code before 'final.ipynb' was created 

 ## Jupyter notebook
  The 'final.ipynb' notebook has all the codes needed for this project

 ## production.yml 
  This is the conda environment file with necessary packages to run 'final.ipynb'

  


# Code structure: 
## 1. Reading raw data
 The `read_json_data` function in this code is designed to read JSON files from a specified folder and convert them into a pandas DataFrame. If a dataset name (like 'BRU' or 'STO') is provided as a parameter, it reads files from the corresponding subfolder within 'data/daily_production'. If no name is provided, it prompts the user to input a dataset name. The function then iterates through all JSON files in the specified subfolder, reads each file, and appends its contents to a list. This list of dictionaries is finally converted into a DataFrame, which is returned by the function. If the specified folder does not exist, it informs the user.

## 2. Defining useful functions
 The provided code is a set of Python functions for data cleaning and transformation:

1. `convert_date(date_str)`: Converts a date string to a pandas datetime object using a specific format (`%m-%d-%Y %H:%M:%S.%f`).

2. `remove_rows_with_mv(dataframe)`: Removes rows from a DataFrame where any column contains the string `'#MV'`. It replaces `'#MV'` with `NaN` and then drops rows with `NaN` values.

3. `clean_data(data, exclude_zero=False)`: Cleans and processes data. If `data` is a string, it reads a JSON file into a DataFrame using `read_json_data(data)`; if `data` is already a DataFrame, it proceeds directly. It applies `remove_rows_with_mv` to remove missing values, and if `exclude_zero` is `True`, it also removes rows where the `production` value is zero. The function formats date, hour, and minute columns, converts certain columns to float, sorts by date, and resets the index.

4. `no_zero(dataframe)`: Removes rows from a DataFrame where the `production` value is zero.


## 3. Saving data to Excel

The `save_to_excel` function is designed for processing and exporting cleaned data to an Excel file:

1. It takes `data_name` as a parameter, representing the name of the dataset to be processed (e.g., "BRU" or "STO").

2. The function first ensures that the output folder (specified by `folder_path`) exists, creating it if necessary.

3. It then reads raw JSON data using the `read_json_data` function, providing `data_name` as an argument to specify the dataset.

4. The raw data is cleaned using the `clean_data` function, which removes rows with missing values and zeros, formats date and time fields, and sorts the data.

5. The cleaned data is saved to an Excel file named `clean_data_{data_name}.xlsx` in the specified output folder. For example, if `data_name` is "BRU", the output file will be `clean_data_BRU.xlsx`.

6. The function is demonstrated with calls to save cleaned data for both "BRU" and "STO" datasets to Excel files.

This function streamlines the process of data cleaning and exporting, making it easy to handle different datasets by simply changing the `data_name` argument.

## 4. Comparing mean and median values with and without zero production days

Here we do a quick test if the data has skews to the left or to the right and how far off the data is for both factories

1. **Data Preparation:**
   - `bru_with_zero` and `sto_with_zero` are DataFrames created by cleaning the raw data for Brussels (BRU) and Stockholm (STO), respectively.
   - `bru_no_zero` and `sto_no_zero` are derived by removing rows with zero production from `bru_with_zero` and `sto_with_zero`.

2. **Calculation of Statistics:**
   - For both Brussels and Stockholm data, the median and mean production values are calculated for two cases: including zero production values (`with Zero`) and excluding zero production values (`without Zero`).

3. **Results Aggregation:**
   - The calculated statistics are aggregated into a dictionary named `results`, which is then converted into a pandas DataFrame `results_df`. This DataFrame contains columns for the location, median with and without zero production, and mean with and without zero production.

4. **Displaying Results:**
   - The DataFrame `results_df` is printed in a table format, providing a clear comparison of median and mean production for Brussels and Stockholm under both scenarios.

This structured approach allows for an insightful comparison of production statistics across different conditions, highlighting the impact of including or excluding zero production values in the analysis.

## 5. Functions for plotting data

The `plot_data` function visualizes production data, optionally filtering out records where maintenance is scheduled. It reads data from a JSON file (or asks for user input if not specified), cleans it, and then plots production against the day of the week, adjusting the display based on maintenance activity.

## 6. Fitting data distribution code

The `find_best_fit_distribution_continuous` function identifies the most suitable probability distribution for a given dataset. It tests a predefined list of distributions, like Normal, Gamma, Exponential, and more, fitting each to the data and evaluating based on the sum of squared errors. The function returns the best-fit distribution and its parameters, while handling and ignoring any runtime warnings or exceptions during the fitting process. This approach enables efficient and robust statistical modeling of diverse data types.

## 7. Simulation code 

The `simulation` function predicts total production for a specified number of days at a given factory. It optionally uses a provided distribution or finds the best-fit one based on cleaned factory data, considering zero values based on the `normal_production` flag. The function then generates random samples from this distribution to estimate total production for the desired duration.

## 8. Test plot code

The `fit_test_plot` function visually compares the original data distribution with its best-fit statistical model. It plots a histogram of the original data and overlays the probability density function (PDF) of the best-fit distribution, determined by `best_distribution` and `best_params`. This graphical representation helps in assessing the accuracy and suitability of the chosen statistical model against the actual data.

## 9. Main code 

- **Data Preparation:**
  - Generates sample production data.
  - Cleans data for Brussels (BRU) and Stockholm (STO).

- **Distribution Analysis:**
  - Identifies best-fit distributions for production data, with and without zero values.
  - Specifically tests Normal, Logistic, and Weibull distributions for Stockholm.

- **Visualization:**
  - Plots production data against best-fit distributions to visualize the fit.

- **Production Prediction:**
  - Simulates total production for a 30-day period for both locations.
  - Includes scenarios with standard and normalized (excluding zeros) production.

- **Results Display:**
  - Summarizes best-fit distributions and simulation results in two DataFrames.
  - Clearly displays statistical analysis and production predictions. 

This structured approach combines statistical analysis, data visualization, and predictive modeling to provide comprehensive insights into production data.


