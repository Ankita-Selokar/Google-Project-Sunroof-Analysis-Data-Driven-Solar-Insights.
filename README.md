# Data-Driven Solar Insights: A Project Sunroof Analysis

## Introduction
The increasing demand for sustainable and cost-effective energy sources has led to a growing interest in solar power. However, assessing the feasibility and potential benefits of solar panels can be challenging for homeowners and businesses due to various factors.

### About Project Sunroof
Project Sunroof, a groundbreaking initiative from Google, addresses this challenge by leveraging advanced technology and data analysis. By utilising Google Maps data, Project Sunroof provides users with accurate estimates of the solar energy potential of their rooftops.
This innovative tool offers valuable information such as roof size, sunlight exposure, and potential energy output, empowering individuals and organisations to make informed decisions about investing in solar power.

## Aim
This project aims to leverage Project Sunroof data to conduct a comprehensive data analysis of solar energy potential within a specific region. By applying advanced data analytics techniques, I will identify key trends, patterns, and insights that can inform policymakers, businesses, and homeowners. Through this analysis, I will also demonstrate my proficiency in data analytics and my understanding of the data life cycle, from data acquisition and cleaning to analysis and visualisation. Showcasing how data can be effectively used to drive sustainable energy solutions.

## Data Life Cycle

### 1. Plan: 
In this initial phase, the types of data needed for the Project Sunroof analysis are determined. Key datasets include Google Project Sunroof data available in Google BigQuery, along with supplementary data from the U.S. Census Bureau for population insights. Data management protocols are established, responsibilities for data handling are assigned, and strategies for data governance throughout the project lifecycle are outlined.
Google BigQuery: For accessing and extracting Project Sunroof data and U.S. Census Bureau Data.

### 2. Capture: 
During the capture phase, data is collected from various sources. The primary dataset comes from Google BigQuery, which provides access to Project Sunroof data. Since this dataset lacks population data, the U.S. Census Bureau dataset will be incorporated, specifically joined via the zip code column. Once the relevant datasets are identified and extracted from the BigQuery Marketplace, they will be stored locally for further analysis. Following are the address for datasets:

[**Project Sunroof Data:**]
(https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=sunroof_solar&page=dataset&project=propane-purpose-437105-j9&ws=!1m4!1m3!3m2!1sbigquery-public-data!2ssunroof_solar)

[**Census Bureau Data:**]
(https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=census_bureau_usa&page=dataset&project=propane-purpose-437105-j9&ws=!1m4!1m3!3m2!1sbigquery-public-data!2scensus_bureau_usa)


### 3. Manage: 
In the management phase, the focus is on the care and maintenance of the captured data. This includes organising the datasets, determining appropriate storage solutions, and selecting tools for effective data management. The datasets will be stored on a local system to facilitate easy access during analysis. Best practices for data management will be implemented to ensure data integrity and security throughout the project.


### 4. Analyse: 
The analysis phase involves utilising the cleaned and organised data to address research questions, solve problems, and support business goals related to Project Sunroof. Data cleaning will be conducted using Python, checking for null values, missing values, and outliers. The cleaned datasets will be saved in CSV format and imported into SQL Server Management Studio using the SQLAlchemy library for further analysis. SQL will be employed for querying the data, while Tableau will be used for visualisation, enabling insights to be derived and findings presented effectively. 


#### 4.1. Data Cleaning: 

The data cleaning process is critical for ensuring the accuracy and reliability of the datasets used for Project Sunroof analysis. The primary dataset consists of:
* 32 columns
* 11,497 rows
Key activities involved in the data cleaning process include:
Evaluated the raw datasets, including Google Project Sunroof data and the U.S. Census Bureau dataset, focusing on key columns for joining.
Checked for null values and missing entries using Pandas, applying strategies for addressing missing values, including imputation or removal of affected records, and identified outliers in numerical data to ensure they did not adversely affect results.
Conducted a final review of the cleaned dataset to confirm successful implementation of all cleaning tasks.
Saved the cleaned dataset in CSV format for import into SQL Server Management Studio for further analysis.

#### 4.2. Data Analysis:

Following the data cleaning process, the analysis phase focuses on utilising  the cleaned and organised datasets to derive insights for Project Sunroof. The key activities involved in this phase include:
Imported the cleaned dataset into SQL Server Management Studio for comprehensive data querying and analysis, leveraging SQL to execute complex queries that support decision-making and problem-solving.
Creating interactive dashboards in Tableau to present findings through charts and maps that illustrate trends, facilitating informed decision-making on solar energy initiatives.


**1. Which high-population regions receive an average yearly sunlight exposure greater than 1300 kWh per kW, indicating areas with strong solar potential for large-scale adoption of solar installations?**
   
This question seeks to identify regions that not only have a high population but also receive abundant sunlight, making them prime candidates for solar energy initiatives.

```SQL
SQL : 
SELECT 
    ps.region_ZipCode,
    ps.state_name,
    ps.yearly_sunlight_kwh_kw_threshold_avg,
    pop.population
FROM 
    Project_Sunroof ps
JOIN
    Population pop ON ps.region_ZipCode = pop.region_ZipCode
WHERE 
    ps.yearly_sunlight_kwh_kw_threshold_avg > 1300 
ORDER BY 
    pop.population DESC; 
```

The analysis reveals that **California**, with a population of **9.36 million** and sunlight potential of **520,147 kWh per kW**, offers the highest opportunity for solar energy adoption due to its large population and abundant sunlight. **Arizona**, **Nevada**, and **Texas** also show strong potential with their high population numbers and ample solar exposure. These states are prime candidates for solar initiatives, presenting excellent opportunities for expanding solar installations to meet energy and sustainability goals

**2. Which regions with moderate solar coverage (between 35% and 50%) have the highest potential for new solar customers based on population size?**
   
This identifies regions with moderate solar coverage and calculates the potential solar customers by multiplying the population by the percentage of solar coverage. It helps focus marketing efforts on areas that may have a sizable customer base but are not yet fully utilising solar energy.

```SQL
SQL:
    SELECT 
    ps.region_ZipCode,
    ps.state_name,
    ps.percent_covered,
    pop.population,
    (pop.population * ps.percent_covered / 100) AS potential_solar_customers
FROM 
    Project_Sunroof ps
JOIN 
    Population pop ON ps.region_ZipCode = pop.region_ZipCode
WHERE 
    ps.percent_covered BETWEEN 35 AND 50   -- Focus on regions with lower solar coverage
ORDER BY 
    potential_solar_customers DESC;  -- Identify areas with the highest potential customer base
```

Insight:
* **Georgia**, **Michigan**, and **Florida** rank as the top three states for potential customers, based on a combination of solar coverage and population, making them key targets for solar marketing and installation efforts.
* **California** has the largest population (286,519) with 41.48% solar coverage, making it a prime target for expansion. Its 46.05% coverage suggests steady growth in adoption.
* **North Carolina** and Texas both have 41.48% solar coverage and large populations, offering strong potential for increased adoption due to untapped customer bases.
* **Ohio and Alabama** show moderate adoption, with solar coverage between 42.5% and 43.5%, signalling potential for growth with targeted efforts.
* **Michigan** has a good balance of population and 44.34% solar coverage, making it a promising region for further solar expansion.

**3. Which states have contributed the most to carbon offset through solar installations, and how does the number of existing installations correlate with their carbon offset contributions?**

To identify states with the highest environmental impact by calculating the total carbon offset from existing solar installations. States with the largest number of solar installations and their total carbon offset will be ranked to determine the top contributors to reducing carbon emissions.

```SQL
SQL:
SELECT  
    ps.state_name,  -- Grouping by state name
    SUM(ps.carbon_offset_metric_tons) AS total_carbon_offset,  -- Summing carbon offset for each state
    SUM(ps.existing_installs_count) AS total_existing_installs  -- Summing existing installations for each state
FROM 
    Project_Sunroof ps
GROUP BY 
    ps.state_name  -- Grouping by state name
HAVING 
    SUM(ps.existing_installs_count) > 0  -- Ensure states have at least 1 existing installation
ORDER BY 
    total_carbon_offset DESC; 
```


Insight:

* **Florida** leads with the highest **carbon offset** (**84.5 million** metric tons) and over **105,000 installations**.
* **Texas** ranks second with 79.8 million metric tons of carbon offset but fewer installations, indicating high efficiency.
* **California** has the highest number of installations (331,000+) but a slightly lower carbon offset at 66.8 million metric tons, suggesting room for optimization.
* Mid-range states like Ohio, Illinois, and Michigan show moderate solar adoption and significant growth potential.
* **Underdeveloped** states like Wyoming and Vermont have very low adoption rates, offering opportunities for solar expansion.


**4. Which regions have the highest potential for solar installations, with more than 50% of buildings qualified for solar, and what is the estimated number of potential solar customers in these regions?**

To identify regions where a significant portion of buildings are suitable for solar installations, providing insights into target markets with high solar potential.
Also provide the total potential solar customers for each state based on the suitability of buildings for solar installation.

**-4.1**
```SQL
SQL:
	SELECT 
	ps.state_name,	
    pop.population,
    ps.percent_qualified,  -- Percentage of buildings suitable for solar installations
    ROUND(pop.population * (ps.percent_qualified / 100),0) AS potential_solar_customers  -- Estimated number of potential solar customers
FROM 
    Project_Sunroof ps
JOIN 
    Population pop ON ps.region_ZipCode = pop.region_ZipCode
WHERE 
    ps.percent_qualified > 50  -- Focus on areas where more than 30% of buildings are suitable for solar
ORDER BY 
    potential_solar_customers DESC;
```


**-4.2**
```SQL
SELECT 
    ps.state_name,  
    SUM(ROUND(pop.population * (ps.percent_qualified / 100), 0)) AS total_potential_solar_customers 
FROM 
    Project_Sunroof ps
JOIN 
    Population pop ON ps.region_ZipCode = pop.region_ZipCode
WHERE 
    ps.percent_qualified > 50  
GROUP BY 
    ps.state_name  
ORDER BY 
    total_potential_solar_customers DESC;
```

Insight:

* **High population** states with significant **solar potential**, such as **California**, **Texas**, and **Florida**, should be the primary focus for solar expansion.
* Urbanised states like **New York**, **Illinois**, and **New Jersey** offer strong potential for targeted solar initiatives in dense population centres.
* Regions with moderate populations but high solar suitability, such as **Arizona**, **Colorado**, and **North Carolina**, could see increased adoption with the right policies and incentives.
  
 
**5. Which regions have the highest number of installed solar panels across different orientations (North, South, East, West, Flat)?**
 To identify regions leading in solar panel installations and the preferred orientations for solar panel placement.
 
```SQL 
SELECT 
    ps.region_ZipCode,
	ps.state_name,
    ps.number_of_panels_n AS panels_north,
    ps.number_of_panels_s AS panels_south,
    ps.number_of_panels_e AS panels_east,
    ps.number_of_panels_w AS panels_west,
    ps.number_of_panels_f AS panels_flat,
    (ps.number_of_panels_n + ps.number_of_panels_s + ps.number_of_panels_e + ps.number_of_panels_w + ps.number_of_panels_f) AS total_panels
FROM 
    Project_Sunroof ps
WHERE 
    ps.existing_installs_count > 0  
ORDER BY 
    total_panels DESC;
```

Insights:
* From **11,497** unique region Zip Codes, **8,249** regions were identified after filtering for regions with **existing installations** (ps.existing_installs_count > 0).
* The total count of solar panels across these regions is **3,279,590**.
* Top states with the highest number of total solar panels include: **Tennessee, California, Texas, Illinois. Florida and Texas** (appearing multiple times for different regions).
* The state with the **least number** of total solar panels installed is **Utah**, with 166 panels.


**6. What is the optimal orientation for solar panels (North, South, East, or West) in different regions based on the highest yearly sunlight (kWh), and which regions receive the maximum sunlight to maximise energy generation?**

This query determines the best orientation (north, south, east, or west) for solar panels in each region based on the highest yearly sunlight (kWh). It helps optimise solar installations for maximum energy generation, guiding efficient placement strategies for homeowners and businesses.

```SQL
SELECT 
    ps.region_ZipCode, 
    ps.state_name,
    CASE 
        WHEN ps.yearly_sunlight_kwh_n > ps.yearly_sunlight_kwh_s 
             AND ps.yearly_sunlight_kwh_n > ps.yearly_sunlight_kwh_e 
             AND ps.yearly_sunlight_kwh_n > ps.yearly_sunlight_kwh_w THEN 'North'
        WHEN ps.yearly_sunlight_kwh_s > ps.yearly_sunlight_kwh_n 
             AND ps.yearly_sunlight_kwh_s > ps.yearly_sunlight_kwh_e 
             AND ps.yearly_sunlight_kwh_s > ps.yearly_sunlight_kwh_w THEN 'South'
        WHEN ps.yearly_sunlight_kwh_e > ps.yearly_sunlight_kwh_n 
             AND ps.yearly_sunlight_kwh_e > ps.yearly_sunlight_kwh_s 
             AND ps.yearly_sunlight_kwh_e > ps.yearly_sunlight_kwh_w THEN 'East'
        ELSE 'West'
    END AS optimal_orientation,     GREATEST(ps.yearly_sunlight_kwh_n, ps.yearly_sunlight_kwh_s, ps.yearly_sunlight_kwh_e, ps.yearly_sunlight_kwh_w) AS max_yearly_sunlight_kwh  
FROM 
    Project_Sunroof ps
ORDER BY 
    max_yearly_sunlight_kwh DESC;
```


Insights:
* **South-facing** solar panels receive the **highest yearly sunlight** across most regions.
* **North-facing** panels consistently receive the **least sunlight**, indicating lower energy efficiency.
* Prioritising south-facing installations can maximise solar energy production.


**7. Which are the top 10 states with the highest number of south-facing solar panels installed, based on the total count of panels?**

```SQL 
SELECT TOP 10
    ps.state_name,
    SUM(ps.number_of_panels_s) AS total_south_facing_panels
FROM 
    Project_Sunroof ps
GROUP BY 
    ps.state_name
ORDER BY 
    total_south_facing_panels DESC;
```

**8. Which regions, if fully covered by solar installations (100% coverage), would result in the highest carbon offset?**

To Identify the regions (region_ZipCode) and states where 100% solar coverage is assumed and calculate the total carbon offset for each. By summing the carbon offset metric tons for regions with full solar coverage, the query provides insight into areas with the highest potential for carbon offset from solar energy adoption.


```SQL 
SELECT 
    ps.region_ZipCode,
    ps.state_name,
    SUM(ps.carbon_offset_metric_tons) AS total_carbon_offset
FROM 
    Project_Sunroof ps
WHERE 
    ps.percent_covered = 100  -- Assume 100% solar coverage
GROUP BY 
    ps.region_ZipCode, ps.state_name;
```


**9. If buildings in communities with 100% coverage all had solar roofs, how many metric tons of carbon would we offset?**

calculates the total potential carbon offset in metric tons for communities with 100% solar coverage and a population greater than zero. By summing up the carbon offsets in these regions, the query provides insight into the environmental benefits if all buildings in those areas were equipped with solar roofs.

```SQL
SELECT 
  ROUND(SUM(ps.carbon_offset_metric_tons), 2) AS total_carbon_offset_possible_metric_tons
FROM 
  Project_Sunroof ps
JOIN 
  Population pop 
ON 
  ps.region_ZipCode = pop.region_ZipCode
WHERE 
  ps.percent_covered = 100.0
  AND pop.population > 0;
```

Insights:
* **Total Carbon Offset**: If all buildings in regions with 100% solar coverage had solar roofs, the potential carbon offset would amount to **78,096.22 metric tons**.
* **Potential for Expansion**: These regions showcase the positive environmental benefits that can be realised through widespread solar adoption.


**10. What are the top regions where small-scale solar panel installations can still produce significant yearly sunlight output?**

This focuses on identifying regions with smaller roof areas (fewer than 50 panels) that still receive high yearly sunlight, helping to pinpoint the most efficient installations based on space and sunlight availability.


```SQL
SELECT 
    ps.region_ZipCode,
    ps.state_name,
    ps.number_of_panels_median,
    ps.yearly_sunlight_kwh_total
FROM 
    Project_Sunroof ps
WHERE 
    ps.number_of_panels_median < 50  -- Focus on efficient installations
ORDER BY 
    ps.yearly_sunlight_kwh_total DESC;
```



### 5. Archive:
 In this project, data is securely stored in BigQuery for large-scale querying and analysis, ensuring efficient access to solar data. Additionally, project versions and scripts are managed in GitHub, allowing for version control and collaboration, ensuring the data lifecycle is properly maintained for future use or reference.


### 6. Destroy: 

Data Sensitivity: The data utilised in this project is not sensitive, and there are no compliance or regulatory requirements necessitating its destruction.
Current Status: Given the ongoing relevance of the insights generated from the data, there is no immediate need for destruction. The data remains available for potential future analyses or updates to the project. 


