# Agrani Bank plc: Last 5 Years Performance Analysis

## Project Overview:
This project involves comprehensive data aggregation, cleaning, analysis, and visualization of Agrani Bank Plc's branch performance across Bangladesh over the last five years. The dataset includes detailed information about branches, operating Thanas, divisions, addresses, and the bank's financial performance in terms of profit, revenue, and expenses. The primary focus of the project is to provide insights into Agrani Bank's financial performance across these years, with visualizations depicting trends, patterns, and comparisons across branches, districts, divisions, and Thanas.

## Objective:
The key objective was to collect and analyze data from Agrani Bank’s annual reports, focusing on:

- Revenue, Expense, and Profit (2019-2023): Collection of total revenue, expense, and profit amounts for five years and distributing these by the percentage of Branches within each division.

- Thana Population Data: Manual collection of the population data for 434 Thanas, and random distribution of this population data to allocate five-year financial figures (profit, revenue, expense) to 941 branches.

- Geospatial Mapping: Manually adding branch addresses and their corresponding map coordinates.

- Performance Analysis: Visualizing various financial and operational KPIs such as profit margin, expense ratio, cost efficiency, and how these metrics evolved across different years, divisions, districts, and branches.

- Comparison by Region: Identifying regional trends in revenue, expense, and profit, with detailed visual comparisons between high and low-performing regions such as Dhaka, Chattogram, Khulna (highest performing) and Rangpur, Sylhet, Barishal (lowest performing).

## Data Sources:
Agrani Bank Data: The dataset of Agrani Bank Plc, consisting of 941 rows and 8 columns, was collected from the Bangladesh Bank branch list PDF. Agrani Bank-specific data was extracted by filtering and copying relevant information, then connected to Google Sheets via a web link connection and exported to Excel for further analysis.

Annual Report: Financial data (revenue, expense, and profit) for the years 2019-2023 was gathered from Agrani Bank’s 2023 Annual Report, specifically from page 174. The report can be accessed here: Agrani Bank Annual Report 2023.

Thana Population Data: Population data for 434 Thanas was manually sourced from:
- Wikipedia

- Banglapedia

- CityPopulation

- The Bangladesh Network (thebangladeshnetwork.com)

- Portal.govt.bd

Branch Address and Map Coordinates: Address details and map coordinates of Agrani Bank branches were retrieved from BanksBD Agrani Bank Branch List.

## Tools Used:

- Google Sheets: Used to convert data from PDF to Sheets for easier manipulation and analysis.

- Excel: Connected to Agrani Bank’s filtered data from Google Sheets for further data processing.

- Google Search: Utilized to search and download relevant Thana population and address data.

- Google Maps: Employed for retrieving branch map coordinates.

- Power BI: Used for data visualization, creating insightful charts, and presenting the findings.

## Data Aggregation:

### Step-1

- Pivot Chart: A pivot chart was created with rows for division, thana, and branch, and values based on the count of Thanas.

- Branch Ratio Table: A separate table was created by division name, total branches and the ratio of branches, calculated as (branch count/total branches) * 100% and each years Revenue, expense and profit column.  Example
```Excel
=E4/$E$12*100%
```

- Financial Calculations: The total profit, revenue, and expense for each of the 5 years were placed in the grand total row. The total profit, revenue, and expense for each branch were calculated by distributing the total financial figures according to the branch 
ratio per division, i.e., total revenue/expense/profit * ratio of branches by division total. Example:
```Excel
=$G$12*F4
```

### Step-2

- On the Population Data-1 page, 8 division-wise population table was created, containing the following columns: Thana Names, Total Branches, Population, and Source. Population data for each division was manually collected from various sources, including:

- ChatGPT (for a few Thanas in Barishal division)

- Wikipedia

- Banglapedia

- CityPopulation

- Portal.gov.bd

- The Bangladesh Network & more. 

### Step-3

- On the Branch Address-1 page, the branch address details (including Branch Name, District, and Address) were collected from the previously provided source link. The addresses were matched with the main dataset using VLOOKUP. However, due to spelling errors in the branch names, the VLOOKUP did not work for most of the matches.

To resolve this, I first attempted to match the data using Thana Names, followed by another attempt using Branch Names. This approach successfully matched most of the addresses creating 2 columns named (Vlookup_by_branch_name, Vlookup_by_thana_name), then these 2 columns data was concatinated into a single column  by this code:
```Excel
=IF(LEN([@[Vlookup_by_thana name]])=0,[@[vlookup_by_branch name]],[@[Vlookup_by_thana name]])
```
but a few remained unmatched. For these, I manually collected the branch addresses from additional sources such as:

- Banks.info

- AgraniBank.org

- BangladeshBranches

- DesiGuide.com

- BusinessDirectory

- Google Maps

- Infoisinfo.com

Finally, map coordinates for these branch addresses were manually gathered through Google Maps searches.

## Revenue || Expense || Profit Allocation:

- A pivot table was created with Division Name and Branch Name as rows. Thana population was added using VLOOKUP from the Population-2 page. Example:
```Excel
=VLOOKUP(A3,'Branch Population-2'!$A$2:$B$943,2,0)
```
The population values were checked with the formula =N() and converted to actual values using =VALUE() where necessary. The sum of each Thana’s population was calculated and placed next to the division name.

- Next, the ratio_by_population was calculated
```Excel
=D3/$D$2*100%
```
followed by the allocation of Revenue, Expense, and Profit per branch.

- For each year from 2019 to 2023, these financial figures were calculated by multiplying the total revenue, expense, and profit of each division by the population ratio of the respective division. Example:
```Excel
=$Y$3*E3
```

## Data Cleaning:
### On the Population Data-2 page, the following data cleaning steps were performed:

- Data Type Check: Populations were checked using the formula 
```Excel
=N()
```
 to determine if values were numeric or text.

- Comma Removal: Commas were removed from the population values using
```Excel
=SUBSTITUTE(E3, ",", "").
```

- Conversion to Numbers: Values were then converted to numeric format using 
```Excel
=VALUE().
```
- Handling Errors: Some rows had #VALUE! errors. To address this, a Value or Text column was created, showing text on the left and the cleaned value on the right.

- Error Checking: The =N() function was used in the Is_Number? column to verify that all values were numeric.

- Zero Removal: Zeros were removed from the dataset using
```Excel
=IF(J13=0, SUBSTITUTE(J13, "0", ""), J13) in the REM_ZEROS column.
```
- Manual Conversion: Any remaining zeros were manually converted into numbers.

- Concatenation: The CONCAT column combined the REM_ZEROS and MAN_CONCAT columns using 
```Excel
=CONCAT(K3, L3).
```
These steps ensured that all population data was accurately formatted and cleaned for further analysis.

### On the Population Data-3 page, the following data cleaning steps were carried out:

- Pivot Table Creation: A pivot table was created with Division, Thana, and Branch as rows and the count of branches as columns.

- Thana Population Addition: Thana populations were added next to each Thana name using
```Excel
=IFERROR(VLOOKUP(E3, $A$3:$B$452, 2, 0), "") to ensure accurate matching.
```
- Random Ratio Assignment: A Random Ratio column was created, with ratios assigned to branches randomly while ensuring that the total ratio always summed to 100%.

- Branch Population Calculation: The Branch Population column was calculated by multiplying the Thana population by the random ratio assigned to each branch.

- Concatenation: Thana_Pop and Branch_Pop columns were created, and the Concat column combined these values using =CONCAT(L3, M3).

These steps ensured that branch populations were correctly calculated and accurately aligned with the Thana populations.

### On the Branch Population-1 page, the following steps were taken:

- Branch Population Column Creation: A Branch Population column was created to capture the population for each branch.

- Population Assignment: The population values for each branch were populated using the formula
```Excel
=VLOOKUP([@[BRANCH_NAME]], $C$3:$D$1432, 2, 0)
```
which referenced the Thana+Branch+Pop data to ensure accurate assignment.

These steps ensured that each branch's population was correctly matched and assigned based on the Thana and branch data.

### Final Dataset was created with these columns:
- SERIAL NUMBER
  
- BANK NAME

- BANK ID

- BRANCH NAME

- BRANCH ADDRESS

- MAP COORDINATES

- BRANCH POPULATION

- POPULATION RATIO

- REVENUE 2023

- REVENUE 2022

- REVENUE 2021

- REVENUE 2020

- REVENUE 2019

- EXPENSE 2023

- EXPENSE 2022

- EXPENSE 2021

- EXPENSE 2020

- EXPENSE 2019

- PROFIT 2023

- PROFIT 2022

- PROFIT 2021

- PROFIT 2020

- PROFIT 2019

- BRANCH CODE

- DIVISION NAME

- DISTRICT NAME

- THANA NAME

## Dataset Transformation:
### For Power BI visualization, the dataset was transformed to streamline the analysis and Agrani csv dataset was created:

- Year Column: Consolidated financial data into a single "Year" column covering all five years (2019-2023).

- Profit Column: Aggregated profit data into a single column for ease of analysis.

- Revenue Column: Aggregated revenue data into a single column.

- Expense Column: Aggregated expense data into a single column.

This restructuring facilitates a more efficient and comprehensive analysis of financial trends and performance across the specified years.

## Data Analysis:
### The data visualization for this project provided insightful analysis of Agrani Bank Plc’s performance over the past five years. The key visualizations include:

- Total Revenue: 332.76 billion BDT

- Total Expense: 274.02 billion BDT

- Total Profit: 4.13 billion BDT

- Total Thanas: 434

- Total Branches: 941

- Total Districts: 64

- Profit Margin: 1.24

- Expense Ratio: .82

- Cost Efficiency: 66.37
  
- Revenue vs. Expense vs. Profit: A line chart illustrating the total revenue, expense, and profit across the years. It highlights that Dhaka, Chattogram, and Khulna have the highest figures for revenue, expense, and profit, whereas Rangpur, Sylhet, and Barishal show the lowest values.

Profit by Branch: A stacked bar chart that identifies Salna Bazar, Netrakona, and Savar as the branches with the highest profits. This chart helps pinpoint which branches are performing exceptionally well.

Profit by Division: A donut chart that reveals Dhaka, Chattogram, and Khulna as the divisions with the highest profits, while Sylhet, Barishal, and Mymensingh are the least profitable. This visualization provides a clear view of profitability across different divisions.

Profit by District: A stacked bar chart displaying the most profitable districts, with Dhaka, Chattogram, and Cumilla leading in profitability. This chart helps in understanding district-level financial performance.

Map of Branches: A geographic map showing the locations of all Agrani Bank branches, providing a spatial context to the distribution of branches.

### Yearly Financial Performance:

- Profit by Year: A stacked column chart showing that 2021 had the highest profit, while 2020 experienced a loss.

- Revenue by Year: A stacked column chart indicating that 2023 had the highest revenue and 2019 had the lowest.

- Expense by Year: A stacked column chart demonstrating that 2023 also had the highest expense, with 2019 having the lowest.

### Financial Ratios:

- Profit Margin vs. Expense Ratio: A clustered column chart providing insights into how the profit margin and expense ratio relate to each other over the years.

- Expense Ratio vs. Cost Efficiency: A line chart showing trends in expense ratio and cost efficiency, with notable performance in years other than 2020.

- Expense Ratio vs. Profit Margin: A line chart depicting the organization’s financial challenges in 2020, efforts to recover in 2021, and the decline in profit margin from 2022 onwards.

- Profit Margin vs. Cost Efficiency: A line chart illustrating that profit margin was consistently above cost efficiency before 2021 but fell below it in 2022 and 2023.

These visualizations provide a comprehensive view of Agrani Bank’s financial and operational performance, helping to identify trends, patterns, and areas for improvement.

## Feedback:

### Based on the data analysis and visualizations, here are targeted recommendations to enhance Agrani Bank’s performance:

1. Focus on High-Performing Regions:

- Leverage Strengths: Continue to invest in and expand operations in high-performing regions such as Dhaka, Chattogram, and Khulna, which demonstrate strong revenue, profit, and expense metrics. Explore opportunities to replicate successful strategies from these regions in other areas.

2. Address Underperforming Areas:

- Strategic Improvement Plans: Develop targeted strategies to improve performance in underperforming regions like Rangpur, Sylhet, and Barishal. Consider localized marketing campaigns, enhanced customer service, or branch operational changes to boost performance.

- Resource Allocation: Evaluate whether current resources and investments in these regions align with their performance. Adjust allocation as necessary to focus on areas with greater growth potential.

3. Enhance Profit Margins and Cost Efficiency:

- Cost Management: Review and optimize cost structures, especially in regions with declining profit margins. Implement cost control measures and seek ways to increase operational efficiency.

- Profit Margin Improvement: Analyze factors leading to profit margin decline since 2022 and develop strategies to address them, such as improving product offerings or pricing strategies.

4. Optimize Revenue and Expense Ratios:

- Revenue Growth: Explore avenues for increasing revenue, particularly in high-expense regions. This could include expanding product lines, enhancing customer engagement, or entering new market segments.

- Expense Management: Implement strategies to manage and reduce expenses, especially in regions where the expense ratio is high relative to revenue. This may involve streamlining operations or renegotiating supplier contracts.

5. Leverage Data-Driven Insights:

- Continuous Monitoring: Establish a regular monitoring process to track financial performance and key metrics. Use these insights to make informed decisions and quickly address any emerging issues.

- Actionable Insights: Use the insights from the financial ratios and trend analyses to refine strategic plans. For example, improving cost efficiency in years where expenses exceeded revenue growth could help stabilize overall performance.

6. Customer and Market Analysis:

- Customer Feedback: Collect and analyze customer feedback to identify areas for improvement in service delivery and customer satisfaction. Implement changes based on this feedback to enhance overall branch performance.

- Market Trends: Stay attuned to market trends and economic conditions that could impact performance. Adapt strategies in response to changing market dynamics to maintain competitiveness.

By focusing on these areas, Agrani Bank can enhance its overall performance, improve profitability, and better align resources with growth opportunities.

## Limitations:

### While the data analysis provides valuable insights into Agrani Bank Plc's performance, there are several limitations to consider:

1. Data Accuracy and Completeness:

- Data Gaps: Manual data collection and entry can introduce errors or inconsistencies, affecting the accuracy of the analysis. 
- Incomplete Data: Some data points, especially those manually collected, may be incomplete or outdated, potentially impacting the overall reliability of the analysis.
- Variations in data quality and format from different sources can affect the consistency and reliability of the information used for analysis.
  
2. Data Aggregation Challenges:

- Assumptions in Population Distribution: The random allocation of Thana population percentages may not fully represent the actual distribution, potentially skewing the financial distribution across branches and divisions.
- Approximation Issues: Financial data allocation based on estimated ratios and population figures may not capture all nuances of branch performance, leading to approximate rather than precise insights.
  
3. Historical Data constraints:

- Limited Historical Context: The analysis is based on historical data for the past five years, which may not account for recent changes or emerging trends that could impact current and future performance.
- Exclusion of External Factors: The analysis does not consider external factors such as economic shifts, regulatory changes, or competitive actions that might affect performance.








