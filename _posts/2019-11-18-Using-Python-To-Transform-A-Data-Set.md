---
layout: single
title: 'Using Python To Transform and Clean a Data Set'
description: 'This post discusses how to use Python to transform and clean a data set into a data set that more easily consumed by Tableau or Power BI.' 
date: '2019-11-18'
classes: wide

---

--------------------------------------------

Often it is necessary to transform a data set from its original format to a format that is easier to work with data visualization software. For this post we demonstrate how to modify a readily available data set that contains GDP data as of 2018 into a data set that is more easily consumed by a data visualization tools such as Power BI or Tableau. 

## Prerequisites:

1. Python 3.x is needed and ideally you have a virtual install.
2. Working install and knowledge of Jupyter Notebook, preferably to a virtual Python environment. 
3. Notepad++, use it to convert a file created on windows to ANSI or UTF-8
4. Working knowledge of Python loops, lists, dictionaries, Pandas
5. Ability to install additional Python libraries to virtual Python environment. 




## Step 1

Navigate to the following web page. 

[https://data.worldbank.org/indicator/NY.GDP.MKTP.CD](https://data.worldbank.org/indicator/NY.GDP.MKTP.CD)

## Step 2 

Download the .xls file 

If you open this file you will notice that the data points from 1960 to 2018 are pivoted across the workbook from left to right.  This is fine for reviewing the data but it is not a great format to consume using Tableau or Power BI.  In the steps below we will transform and clean this file so that it is in a format that can be more easily consumed by Tableau or Power BI.  

## Step 3

Copy the downloaded file to a new name called GDP 2018.xls.  

## Step 4

Open the file and perform a few clean up tasks.  

a.  Remove the first three rows from the GDP 2018.xls file.  Next remove the two columns called "Indicator Name" and "Indicator Code".

b.  Next select all the cells with numeric values, some cells will look like  "1.5343E+11" but this value is not very data visualization friendly. To convert to a regular numeric value select all the cells under the dates from 1960 to 2018 and then right click on the selected cells and choose Format Cells.  Change the data type to number using the default.   
c.  Once the preliminary clean up is done, copy the content of the excel sheet to a new excel workbook.  
d.  Select all the data cells that contain numbers and format as a number. 
d.  Save the new workbook as a .csv file "GDP 2018.csv".  Note: It if the .csv is created with windows and your data transformation is done using Linux you may need to convert the GDP 2018.csv file from UTF-8 to ANSI.  You can use notepad++ to do the conversion.  
e.  Open the original data file and copy the Metadata - Countries to a new empty workbook and save the file as a .csv called "GDP 2018 metadata".  This file will be used to merge with the data in the first .csv called "GDP 2018.csv".  

## Step 5

Now it's time to do the transformation of the data set.  Notice that the data set contains data from left to right with the column headings of 1960 to 2018.  This is great for reviewing data that has been collected but it doesn't work well with with Tableau or Power BI which expect a data set / model that is dimensional in nature.  A complete discussion of dimensions and facts is outside the scope of the post to learn more see. 

Open Jupyter Notebook and create a new notebook called "GDP 2018".  Use the following code to connect to your .csv files. 

a. Connect to the GDP 2018.csv
```
opened_file = open('../data/GDP 2018.csv')
from csv import reader
read_file = reader(opened_file)
wb_gdp = list(read_file)
opened_file.close()
wb_gdp = wb_gdp[1:]
```
b. Connect to the GDP 2018 metadata.csv
```
opened_file = open('../data/GDP 2018 metadata.csv')
from csv import reader
read_file = reader(opened_file)
wb_gdp_md = list(read_file)
opened_file.close()
wb_gdp_md = wb_gdp_md[1:]
```
c.  Use the following code to loop through each .csv created above and then write a new .csv called "GDP 2018 clean.csv"

```
import pandas as pd

# create a years dictionary to loop through 

years = {1960:2,1961:3,1962:4,1963:5,1964:6,1965:7,1966:8,1967:9,1968:10,1969:11,1970:12,1971:13,1972:14, \
         1973:15,1974:16,1975:17,1976:18,1977:19,1978:20,1979:21,1980:22,1981:23,1982:24,1983:25,1984:26, \
         1985:27,1986:28,1987:29,1988:30,1989:31,1990:32,1991:33,1992:34,1993:35,1994:36,1995:37,1996:38,1997:39, \
         1998:40,1999:41,2000:42,2001:43,2002:44,2003:45,2004:46,2005:47,2006:48,2007:49,2008:50,2009:51,2010:52, \
         2011:53,2012:54,2013:55,2014:56,2015:55,2016:56,2017:57,2018:58}

records = []
record = []

for row in wb_gdp:
    
    country = row[0]    
    country = country.replace(",","")
    if country == "Gambia The":
        country = "Gambia"
        
    country_code = row[1] 
    
    region = ""
    income_group = ""
    
    # based on the country_code, fetch the region & income group info
    
    for code in wb_gdp_md:
        if code[0] == country_code:
            region = code[1]
            income_group = code[2]


    
    for year in years:        
               
        #print(country_code+','+country+','+region+','+income_group+','+str(year)+','+str(row[years[year]]))
        
        record = [country_code,country,region,income_group,str(year),str(row[years[year]])]
        if records != "":
            records.append(record)


      
df = pd.DataFrame(records)

df.to_csv('../data/GDP 2018 clean.csv', sep=',',index=False,header=False)
```

## Summary

This is a really simple example of how to use Python to transform and clean a data set.  Rather than spending hours using another tool or technique to modify the originally downloaded data set so it is easily consumed by Tableau or Power BI, you have been able to transform over 15,000 records into a format that is more easily consumed by Tableau or Power BI in less than an hour using Python.  The next step is loading the data into Tableau or Power BI to create a data visualization. If you would like to use the cleaned data set you can find it [here](../downloads/GDP 2018 clean.csv).  
