# Data-Driven Solar Insights: A Project Sunroof Analysis

## Introduction
The increasing demand for sustainable and cost-effective energy sources has led to a growing interest in solar power. However, assessing the feasibility and potential benefits of solar panels can be challenging for homeowners and businesses due to various factors.
About Project Sunroof
Project Sunroof, a groundbreaking initiative from Google, addresses this challenge by leveraging advanced technology and data analysis. By utilising Google Maps data, Project Sunroof provides users with accurate estimates of the solar energy potential of their rooftops.
This innovative tool offers valuable information such as roof size, sunlight exposure, and potential energy output, empowering individuals and organisations to make informed decisions about investing in solar power.

## Problem Statement
This project aims to leverage Project Sunroof data to conduct a comprehensive data analysis of solar energy potential within a specific region. By applying advanced data analytics techniques, I will identify key trends, patterns, and insights that can inform policymakers, businesses, and homeowners. Through this analysis, I will also demonstrate my proficiency in data analytics and my understanding of the data life cycle, from data acquisition and cleaning to analysis and visualisation. Showcasing how data can be effectively used to drive sustainable energy solutions.

## Data Life Cycle

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
 
