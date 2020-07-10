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

* Generate a summary statistics table consisting of the mean, median, variance, standard deviation, and SEM of the tumor volume for each drug regimen.

* Generate a bar plot using both Pandas's DataFrame.plot() and Matplotlib's pyplot that shows the number of total mice for each treatment regimen throughout the course of the study.

* Generate a pie plot using both Pandas's DataFrame.plot() and Matplotlib's pyplot that shows the distribution of female or male mice in the study.

* Calculate the final tumor volume of each mouse across four of the most promising treatment regimens: Capomulin, Ramicane, Infubinol, and Ceftamin. Calculate the quartiles and IQR and quantitatively determine if there are any potential outliers across all four treatment regimens.

* Using Matplotlib, generate a box and whisker plot of the final tumor volume for all four treatment regimens and highlight any potential outliers in the plot by changing their color and style.

* Select a mouse that was treated with Capomulin and generate a line plot of time point versus tumor volume for that mouse.

* Generate a scatter plot of mouse weight versus average tumor volume for the Capomulin treatment regimen.

* Calculate the correlation coefficient and linear regression model between mouse weight and average tumor volume for the Capomulin treatment. Plot the linear regression model on top of the previous scatter plot.

* Look across all previously generated figures and tables and write at least three observations or inferences that can be made from the data. Include these observations at the top of notebook.

---
<a name="solution"></a>
> **Solution**

<a name="01"></a>
<a name="01_top"></a>
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

[Back to output](#01_top)
</details>

[Back to the Top](#01_top)

