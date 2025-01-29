# Power-Bi-Plant-Co.-Performance-Report

## Overview
This Power BI project demonstrates a complete data preparation and modeling process. The project begins by preparing data using Power Query and Excel virtual tables. The next step involves building the data model by creating measures, slicers, and calculated columns using DAX (Data Analysis Expressions) to enhance the analytical capabilities of the report. Additionally, dynamic and visually appealing visuals are created to provide insightful, interactive data exploration for users.

## Data Source
The data for this project is sourced from the GitHub repository **"Plant Co"**. This dataset is used to perform various data analysis and visualizations in Power BI.


## Tools Used
- **Excel**: Used to create virtual tables and for initial data preparation.
- **Power BI**: Utilized for data loading, cleaning, modeling, and creating interactive visuals.
- **DAX Language**: Used for creating measures, calculated columns, and slicers to enhance the data model and interactivity.


  
## Data Cleaning Process
1. **Creating Virtual Tables**: Used Excel to create virtual tables, which were then imported into Power BI for further analysis.
2. **Loading Data**: Imported the data into Power BI and opened Power Query to begin the cleaning process.
3. **Removing Duplicates**: Identified and removed any duplicate records to ensure data integrity.
4. **Column Headers**: Checked the column headers for clarity and corrected any inaccuracies or inconsistencies.
5. **Column Types**: Verified and adjusted column data types to ensure they matched the intended values (e.g., dates, numbers).
6. **Renaming Tables**: Renamed the tables for better clarity and easier understanding of the data model.


## Data Modeling and DAX Implementation

### Date Dimension Table (dim_date)
To ensure effective time-based comparisons, I created a date dimension table using the following DAX expression:

```DAX
Dim_Date = CALENDAR(
    DATE(2022, 1, 1),
    DATE(2024, 12, 31)
)

```

### Creating the "Inpast" Column (Past 12 Months Flag)
To determine whether a date is within the past 12 months from the most recent sale, I created the following DAX expression:

```DAX
Inpast = 
   VAR lastsalesdate = MAX(Fact_Sales[Date_Time])
   VAR lastsalesdatepy = EDATE(lastsalesdate, -12)
   RETURN
   Dim_Date[Date] <= lastsalesdatepy

```

To support the analysis and enable dynamic slicing, I created several measures, which were grouped into a folder called "Base Measures" for better organization. These measures are used for calculations like gross profit, quantity, sales, and gross profit percentage.

### For Example
The **Gross Profit** measure is calculated using the formula:

```DAX
Gross Profit = [Sales] - [Cost_of_goods]

```

### Prior Year-to-Date Gross Profit (PYTD Gross Profit)

The PYTD measures were created to enable year-over-year comparisons by calculating the values for Gross Profit, Quantity, and Sales for the same period in the prior year.


To calculate the **Prior Year-to-Date Gross Profit**, I used the following DAX expression:

```DAX
PYTD_Gross Profit = 
CALCULATE(
    [Gross Profit],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)

```

###  YTD (Year-to-Date) measures 

are used to calculate the total for a specific metric (such as Gross Profit, Quantity, or Sales) from the beginning of the current year up to the selected date.

```DAX
YTD_Gross Profit = 
TOTALYTD(
    [Gross Profit],
    Fact_Sales[Date_Time]
)
```

###  Slicers for YTD and PYTD

To allow users to dynamically switch between YTD and PYTD values, a slicer was created using a custom DAX expression.

```DAX
S_PYTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(
    selected_value,
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_Gross Profit],
    BLANK()
)
RETURN result
```

### Values Table for Slicers

To make the slicers dynamic, a Values Table was created. This table contains the possible metrics that users can select from the slicer (Sales, Quantity, Gross Profit). 

## Visualizations

1. Treemap: Bottom 10 YTD vs PYTD by Country

    A Treemap visualization was created to display the bottom 10 countries in terms of Sales, Quantity, and Gross Profit for YTD vs PYTD.
   
2. Waterfall Chart: Quantity YTD and PYTD by Month, Country, and Product
  A Waterfall Chart visualization was created to show the Quantity performance for YTD and PYTD across different months, countries, and products

3. Line and Stacked Column Chart: Value YTD and PYTD by Month and Product Type

   A Line and Stacked Column chart was used to display Value (Sales) for YTD and PYTD by Month and Product Type.
   
4. Slicer for Year and Dynamic Report Title
5. Cards for Key Metrics


## Skills Demonstrations
Throughout this project, various skills were demonstrated, including:

### Data Preparation and Cleaning:

- Successfully imported and cleaned data using Power Query.
- Handled issues like removing duplicates, correcting column headers, and adjusting data types for accurate analysis.
- Data Modeling and Relationships:

- Created relationships between tables to establish a solid data model.
- Implemented DAX measures, slicers, and calculated columns to facilitate detailed analysis.
- Dynamic Analysis with DAX:

- Developed advanced DAX expressions for YTD, PYTD, and dynamic slicers.
- Created calculated columns and measures to track performance over different time periods and metrics (Sales, Quantity, Gross Profit).
- Visualization Design:

- Designed interactive and insightful visualizations, including Treemap, Waterfall Chart, and Line & Stacked Column Chart.
- Used slicers and dynamic titles to create a flexible and user-friendly dashboard.
- Slicing and Dicing Data:

- Utilized dynamic slicers and created a Values Table for switching between different metrics (Sales, Quantity, Gross Profit).
- Provided an interactive report experience by allowing users to filter data by Year and Country.
- Performance Comparison:

- Enabled year-over-year comparisons using YTD and PYTD measures, helping users identify trends and performance differences.

## Conclusion
This Power BI project demonstrates the ability to transform raw sales data into actionable insights through the use of Power BI, Excel, and DAX. By leveraging data preparation, modeling, and visualization techniques, the project provides a comprehensive overview of sales performance. The dynamic slicers, calculated columns, and measures allow users to explore data interactively, compare performance over time, and make informed decisions.

This project highlights not only technical skills in Power BI and DAX but also the ability to create engaging, user-friendly reports that provide clear insights into complex data sets.   
      
