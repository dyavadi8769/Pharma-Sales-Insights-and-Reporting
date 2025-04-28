# Pharma Sales Insights and Reporting
In this ‘Data Analysis’ project, we’ll analyze a global Pharmaceutical Manufacturing Company's raw sales data and draw meaningful insights.

<img src="images\Pharma_homepage.jpg" width="1000" height="500" />

## Features
⚡PowerBI Desktop   
⚡PowerQuery Editor [For data-transformation/data-modeling]  
⚡PowerBI Service [For making the report accessible on the web without PowerBI login]  
⚡Multipage fully Interactive Report [For drawing insights and analysis] 

## Table of Contents
- [Introduction](#introduction) 
- [Objective](#objective)
- [Dataset](#dataset)
- [Solution Approach](#solution-approach)
- [How To Use](#how-to-use)
- [License](#license)
- [Get in touch](#get-in-touch)


## Introduction
* `Datamatrix-ml Pharmaceuticals` is one of the leading Pharmaceutical Manufacturing companies with a global presence. 
* Their Markets are divided into different regions across the world. One of those regions manages the German and Poland Markets. 
* Company does not sell directly to customers. Instead, they work with a couple of Distributors in all their regions. 
* They have an agreement with each distributor to share their Sales Data. This is to enable them to gain insights up to the retail level. This data is made available to them in CSV format.

## Objectives
The firm has asked us to perform in-depth data analysis to get insight into company sales performance. Specifically, below are the essential requirements to be satisfied…
|Requirement ID|For Whom|Requirement Description|
|:--|:---|:--|
DM-DA01-REQ-1|Executive Committee|A high-level overview showing `company’s overall sales performance by `year` by `month,` by `customer cities,` by `channel,` by `sub-channel .`Should be able to quickly see `top drug class by sales`, `top drug by sales`, `top customer city by sales`
DM-DA01-REQ-2|Sales Manager/Sales Rep|A detailed overview showing sales `by distributors and product,` `top 5 product, customer and cities`, sales numbers split by `channels and sub-channels.`
DM-DA01-REQ-3|Head of Sales|A detailed report of `sales by sales-team split by product` and `sales by sales-team split by product class.` <br> A detailed analysis showing `Top sales managers`, `Top sales reps,` `Top product split by sales team contributions` answering. <br> An ability to filter/slice data by `year` and `months.`   

***Table-1 : Requirements***

## Dataset
The dataset is sourced from each distributor. It contains Pharmaceutical Manufacturing Company’s, Wholesale-Retail Data. The field description of the raw data is given below. The cleaned dataset `pharma-data.csv` can be downloaded from [here](https://drive.google.com/file/d/1npKF_C2tG5psY-at4wvpEgh6T-7KHxEZ/view?usp=share_link)

|Field|Description|
|:---|:--|
|Distributor| Name of Wholesaler|
|Customer Name| Name of customer|
|City| Customer's city|
|Country| Customer's country|
|Latitude| Customer's Geo Latitude|
|Longitude| Customer's Geo Longitude|
|Channel|Class of buyer (Hospital, Pharmacy)|
|Sub-channel|Sector of the buyer (Government, Private, etc.)|
|Product Name|Name of Drug|
|Product Class|Class of Drug (Antibiotics, etc.)|
|Quantity|Quantity purchased|
|Price|Price product was sold for|
|Sales|Amount made from sale|
|Month|Month sale was made|
|Year|Year sale was made|
|Name of Sales Rep|Name of the Sales rep who facilitated the sale|
|Manager|Sales rep's Manager Name|
|Sales Team|Sale rep's team|
***Table-2 : Data Definition***

## Solution Approach 

|Requirement ID|Solution ID|Proposed Solution|
|:--|:---|:--|
|DM-DA01-REQ-1|DM-DA01-SOL-1|An `Executive Summary` PowerBI dashboard/report page will be built to show a high-level overview of sales data in interactive visuals per the requirements. A `year` filter will be provided to filter the data by a particular or combination of years |
|DM-DA01-REQ-2|DM-DA01-SOL-2|A `Distributor & Customer Analysis` PowerBI dashboard/report page will be provided with interactive visuals showing data as per the requirement|
|DM-DA01-REQ-3|DM-DA01-SOL-3|A `Sales Team Performance` PowerBI dashboard/report page will be provided with interactive visuals showing data as per the requirement. `year` and `month` slicers will be provided to slice/filter data by year and/or months|

***Table-3 : Proposed Solution***

### Exploratory Data Analysis (EDA) [pandas]
To understand, be familiar with and check the sanity of the given data, the first step is EDA. This project's initial data exploration has been carried out using the `pandas` python package. Here, in general, we are checking... 
 * Presence of any missing values 
 * Any unusual value (outliers) 
 * Incorrect values (e.g., sales column, we see -ve numbers)
 * Determine `categorical` and `numeric` columns
 * Number of Unique values present in each `categorical` column 
 * Determine dimensions of categorical columns and range of numeric columns
 * Distributions of numeric columns
 * Correlations between columns and visualizing them using HeatMap

Note that these steps can be performed using `PowerQuery Editor` and/or excel; however, `pandas` makes it much easier and faster; on top of that, `pandas` can handle massive datasets.

EDA steps can be found in the `EDA.ipynb` notebook.

### Data Cleaning and Transform [PowerQuery Editor]
Although the dataset was already well structured, we leveraged Power Query’s advanced capabilities and complemented them with robust DAX computations to deliver a truly end-to-end solution:

* Dynamic Parameterization & Conditional Logic: Created custom parameters and conditional columns to automatically adjust filters, splits, and aggregations as new data arrived.

* Unpivot & Pivot Operations: Transformed nested attribute columns into a normalized structure with unpivot/pivot steps to enable flexible reporting across multiple dimensions.

* Custom M-Function Development: Wrote reusable M-functions for complex parsing (e.g., splitting combined location codes, standardizing product naming conventions) and applied them across queries for consistency.

* Query Folding & Performance Optimization: Ensured all transformations folded back to the source where possible, significantly reducing load times; optimized step order and introduced query buffering for large tables.

* Merging & Reference Tables: Merged multiple distributor and geography lookup tables into the main fact query using fuzzy matching and robust key-mapping logic to enrich sales records with region and channel metadata.

* Error-Handling & Data Validation: Implemented try…otherwise and validation rules within Power Query to catch and correct negative sales values, malformed dates, and null geographical coordinates before they entered the model.

* DAX-Based Measures & Calculations: Developed complex DAX measures and calculated columns—such as YTD sales, month-over-month growth %, dynamic ranking by product and region, and advanced time-intelligence functions (TOTALYTD, SAMEPERIODLASTYEAR)—to power interactive KPIs and enable slice-and-dice analysis across the dashboards.

### Data Model Creation [PowerBI Desktop]
Starting from a single flat CSV, we've implemented a full star-schema design by separating facts from dimensions. Below is the README.md content for documenting this model.


1. Surrogate Keys

* Generated integer PKs for dimensions; used matching FKs in the fact table.

2. Relationships

* Defined one-to-many (1:*) links from each dimension into FACT-sales for optimal query performance.

3. Hidden & Sort Settings

* Hid technical key fields in the report view.

* Sorted Month names by Month_ID for correct chronology.

4. Data Categories & Time Intelligence

* Marked City/Country for map visuals.

* Enabled date hierarchies on Year/Month in the DIM-month table.

5. Performance Tuning

* Disabled Auto Date/Time.

* Ensured Power Query steps fold back to the source.

* Removed unused columns and tables from the model view.

<img src="images\data-model.png"/>

The tables with the prefix `DIM` are dimension tables, and `FACT` is the fact table.

### Report Creation [PowerBI Desktop]
Three interactive reports/dashboards (report pages) will be created to implement the proposed solution. Refer to [Table-3: Proposed Solution](#solution-approach) for detailed requirements and the corresponding proposed solution. 

#### 1. Executive Summary Report [DM-DA01-SOL-1]
This high-level report shows the overall sales figures and elements at a glance.

<img src="images\Pharma_Executive_Summary.jpg"/>

#### 2. Distributor & Customer Analysis Report [DM-DA01-SOL-2]
This more granular detailed report analyses data from the company distributors' and customers' perspectives. Sales by the distributor can be drilled down to specific product levels.
 
 <img src="images\Pharma_Distributor_and_Customer_Analysis.jpg"/>
 
 #### 3. Sales  Team Performance Report [DM-DA01-SOL-3] 
 This is another detailed report that analyses the performance of the company's sales team. Sales by the sales team can be drilled down to product class and specific product levels.
 
 <img src="images\Pharma_Sales_Team_Performance.jpg"/>

## How To Use


### Full access via PowerBI desktop
If you have PowerBI desktop installed, download the `pharma-sales-analysis.pbix` from the repo and open it using PowerBI desktop. There is no need to download the raw dataset; the `pbix` files contain the complete normalized data model, feel free to modify and experiment with it.   


## Get in touch
[![email](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:dyavadi324@gmail.com)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sai-kiran-reddy-dyavadi-a108011aa/)



[Back To The Top](#Pharma-Sales-Insights-and-Reporting)
