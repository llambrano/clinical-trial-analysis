# clinical-trial-analysis

###### by Libardo Lambrano

## Overview <a name="top"></a>

Using Python Matplotlib to analyze potential treatments for squamous cell carcinoma (SCC), a commonly occurring form of skin cancer. The purpose of this study was to compare the performance of Pymaceuticals' drug of interest, Capomulin, versus the other treatment regimens.

* [Assignment Instructions / Tasks](#intro)
* [Solution](#solution)
* [Link to Jupyter Notebook](solution/clinical-trial-analysis.ipynb)

---
![](images/Laboratory.jpg)
## Clinical Trial Analysis

---
<a name="intro"></a>
> **Instructions / Tasks**

* Check the data for any mouse ID with duplicate time points and remove any data associated with that mouse ID. [view solution](#01)

* Generate a summary statistics table consisting of the mean, median, variance, standard deviation, and SEM of the tumor volume for each drug regimen. [view solution](#02)

* Generate a bar plot using both Pandas's DataFrame.plot() and Matplotlib's pyplot that shows the number of total mice for each treatment regimen throughout the course of the study. [view solution](#03)

* Generate a pie plot using both Pandas's DataFrame.plot() and Matplotlib's pyplot that shows the distribution of female or male mice in the study. [view solution](#04)

* Calculate the final tumor volume of each mouse across four of the most promising treatment regimens: Capomulin, Ramicane, Infubinol, and Ceftamin. Calculate the quartiles and IQR and quantitatively determine if there are any potential outliers across all four treatment regimens. [view solution](#05)

* Using Matplotlib, generate a box and whisker plot of the final tumor volume for all four treatment regimens and highlight any potential outliers in the plot by changing their color and style. [view solution](#06)

* Select a mouse that was treated with Capomulin and generate a line plot of time point versus tumor volume for that mouse. [view solution](#07)

* Generate a scatter plot of mouse weight versus average tumor volume for the Capomulin treatment regimen. [view solution](#08)

* Calculate the correlation coefficient and linear regression model between mouse weight and average tumor volume for the Capomulin treatment. Plot the linear regression model on top of the previous scatter plot. [view solution](#09)

* Look across all previously generated figures and tables and write at least three observations or inferences that can be made from the data. Include these observations at the top of notebook. [view solution](#10)

---
<a name="solution"></a>
> **Solution**

<a name="01"></a>
**Remove mouse ID with duplicate time points**

![remove duplicated data](images/steps/01.png)

<details><summary>click here to view steps</summary>

1. Import dependencies, read and combine CSV files

    ```
    # Dependencies and Setup
    import matplotlib.pyplot as plt
    import pandas as pd
    import scipy.stats as st
    import numpy as np

    # Study data files
    mouse_metadata_path = '../data/Mouse_metadata.csv'
    study_results_path = '../data/Study_results.csv'

    # Read the mouse data and the study results
    mouse_metadata = pd.read_csv(mouse_metadata_path)
    study_results = pd.read_csv(study_results_path)

    # Combine the data into a single dataset
    mouse_study_results = study_results.merge(mouse_metadata, on = 'Mouse ID')
    ```
2. Get all the data for the duplicate mouse ID. 
    ```
    mouse_id_dups = mouse_study_results[mouse_study_results[['Mouse ID', 'Timepoint']].duplicated() == True]
    ```
3. Create a clean DataFrame by dropping the duplicate mouse by its ID & Timepoint mix.
    ```
    mouse_study_results.drop_duplicates(subset=['Mouse ID', 'Timepoint'])
    ```
4. Removing the mouse with duplicated data completelly
    ```
    mouse_study_results = mouse_study_results[~mouse_study_results['Mouse ID'].str.match('g989')]
    ```

    [Back to output](#01)
</details>

[Back to tasks](#intro) or [Back to the top](#top) 

<a name="02"></a>
**Summary statistics table**

![remove duplicated data](images/steps/02.png)

<details><summary>click here to view steps</summary>

1. Method 1 - creating multiple series and putting them all together at the end
    
    ```
    mean = mouse_study_results.groupby('Drug Regimen')['Tumor Volume (mm3)'].mean()
    median = mouse_study_results.groupby('Drug Regimen')['Tumor Volume (mm3)'].median()
    variance = mouse_study_results.groupby('Drug Regimen')['Tumor Volume (mm3)'].var()
    standard_deviation = mouse_study_results.groupby('Drug Regimen')['Tumor Volume (mm3)'].std()
    SEM = mouse_study_results.groupby('Drug Regimen')['Tumor Volume (mm3)'].sem()

    # This method is the most straighforward, creating multiple series and putting them all together at the end.

    summary_statistics_1 = pd.DataFrame({'mean': mean,
                                        'median': median, 
                                        'var': variance, 
                                        'std': standard_deviation, 
                                        'sem': SEM})
    ```

2. Method 2 -  Generate a summary statistics table using a single groupby function
    
    ```
    summary_statistics_2 = mouse_study_results.groupby('Drug Regimen').agg({'Tumor Volume (mm3)': ['mean', 'median', 'var', 'std', 'sem']})
    summary_statistics_2
    ```

    [Back to output](#02)
</details>

[Back to tasks](#intro) or [Back to the top](#top) 

