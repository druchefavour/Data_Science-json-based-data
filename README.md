# Data_Science-json-based-data

* The purpose of this project is to get familiar with packages for dealing with JSON strings and files

* First import the packages that are required for data manipulation
    * %matplotlib inline to enable creating plots inside the notebook 
	* import pandas as pd
	* import json and json_normalize
* Exercise 1: Using data in file 'data/world_bank_projects.json', find the 10 countries with most projects
	* Start by loading jsonas string : use json.load((open('data/world_bank_projects.json')))
	* Readn json (data) as pandas DataFrame ~ observe that '_id' the project id elements is nested
	* Use json_normalize to create tables from nested element : json_normalize(data, '_id')
	* Further populate tables created from nested element to include a column for countryname: json_normalize(data, '_id', 'countryshortname')
	* Sort projects by countries: projectsdf.sort_values(by='countryshortname')
	* Generate the frequencies of the projects per country: projects_countdf = projects_sorteddf.countryshortname.value_counts()
	* Call .head() method on 'projects_countdf' above to find the 10 countries with most projects: projects_countdf.head(10)

* Exercise 2: Using the same data, find the top 10 major project themes (using column 'mjtheme_namecode')
	* Start by creating a dataframe with 'mjtheme_namecode' as column: wbankdf[["countryshortname", "mjtheme_namecode"]]; where wbankdf is the variable name for the normalized world bank table created in exercise 1.
	* Observe that 'mjtheme_namecode' is nested ~ use json_normalize to create a table from the nested elements: mj_themes=json_normalize(data, 'mjtheme_namecode')
	* # further populate tables created from nested element: mj_themesdf = json_normalize(data, 'mjtheme_namecode', 'countryshortname')
	* Determine the major projects by calling value counts methode on 'code': mj_counts=mj_themesdf.code.value_counts()
	* Determine the 10 major projects code by calling .head() method on mj_counts.


* Exercise 3: In 2. Create a dataframe with the missing names associated with the codes filled in. 
	* Since numpy will be used to fill in the missing values do import numpy as np
	* Use regular expression to replace the blank spaces in mj_themes with NaN and call it 'nandf'
nandf=mj_themes.replace(r'\s+( +\.)|#',np.nan,regex=True).replace('',np.nan)
	* Sort 'nandf' by code to prepare the table for forward filling ~assign to variable 'nandf_sorted':
nandf_sorted=nandf.sort_values(by='code')
	* Forward fill the column: 'name' in order to replace the NaN with the name in the preceeding row: filldf = nandf_sorted.fillna(method='pad')
	* In exercise 2, the major projects sizes are determined using the .groupby () method~
mj_projects_sizes = filldf.groupby(['code', 'name']).size()
mj_projects_sizes
	* Determine the top ten major projects in ascending order are ~ with columns code and name displayed by calling the .tail() method on the dataframe for sorted values: top_ten_mjdf = mj_projects_sizes.sort_values().tail(10)
top_ten_mjdf

# ----------------------------- End --------------------

