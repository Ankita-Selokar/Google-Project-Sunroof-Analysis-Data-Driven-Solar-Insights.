# Data-Driven Solar Insights: A Project Sunroof Analysis

## Introduction
The increasing demand for sustainable and cost-effective energy sources has led to a growing interest in solar power. However, assessing the feasibility and potential benefits of solar panels can be challenging for homeowners and businesses due to various factors.
About Project Sunroof
Project Sunroof, a groundbreaking initiative from Google, addresses this challenge by leveraging advanced technology and data analysis. By utilising Google Maps data, Project Sunroof provides users with accurate estimates of the solar energy potential of their rooftops.
This innovative tool offers valuable information such as roof size, sunlight exposure, and potential energy output, empowering individuals and organisations to make informed decisions about investing in solar power.

## Problem Statement
This project aims to leverage Project Sunroof data to conduct a comprehensive data analysis of solar energy potential within a specific region. By applying advanced data analytics techniques, I will identify key trends, patterns, and insights that can inform policymakers, businesses, and homeowners. Through this analysis, I will also demonstrate my proficiency in data analytics and my understanding of the data life cycle, from data acquisition and cleaning to analysis and visualisation. Showcasing how data can be effectively used to drive sustainable energy solutions.

## Data Life Cycle

1. Which high-population regions receive an average yearly sunlight exposure greater than 1300 kWh per kW, indicating areas with strong solar potential for large-scale adoption of solar installations?
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

2. Which regions with moderate solar coverage (between 35% and 50%) have the highest potential for new solar customers based on population size?
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

* **Georgia**, **Michigan**, and **Florida** rank as the top three states for potential customers, based on a combination of solar coverage and population, making them key targets for solar marketing and installation efforts.
* **California** has the largest population (286,519) with 41.48% solar coverage, making it a prime target for expansion. Its 46.05% coverage suggests steady growth in adoption.
* **North Carolina** and Texas both have 41.48% solar coverage and large populations, offering strong potential for increased adoption due to untapped customer bases.
* **Ohio and Alabama** show moderate adoption, with solar coverage between 42.5% and 43.5%, signalling potential for growth with targeted efforts.
* **Michigan** has a good balance of population and 44.34% solar coverage, making it a promising region for further solar expansion.
