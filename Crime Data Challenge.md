This is a simple dashboard I created using Tableau to depict Crime Index by Country.

![image](https://github.com/NMangi1/NMangi1/assets/169910014/22fc4fef-756e-402b-8e69-77550d954285)

[Link to Tableau Dashboard](https://public.tableau.com/app/profile/nicholas.mangione1725/viz/CrimeByCountry/Dashboard1?publish=yes)

This is the code I used in SQL to gather and organize the data:

After reading an online database containing countries and their populations into SQL, I changed the column names and altered the Population datatype in order
to express population without respresenting it in thousands of people, i.e. 1,000,000 was entered as 1,000 prior.

```SQL
ALTER TABLE dbo.WorldPopulation
ALTER COLUMN column4 BIGINT;

SELECT 
	column2 AS 'Rank',
	column3 AS 'Country',
	column1 AS 'Abbrv',
	column4*1000 AS 'Population'
INTO dbo.WorldPop
FROM dbo.WorldPopulation

SELECT * FROM dbo.WorldPop
```

Next, I altered the names from a dataset conatining information about the GDP of each country, using the following code:
```SQL
SELECT 
	Entity AS 'Country',
	Code AS 'Abbrv',
	GDP_constant_2015_US AS 'GDP'
INTO dbo.GDP
FROM dbo.GDPByCountry WHERE Year = 2021

SELECT * FROM dbo.GDP
```

I also changed the names of a dataset containing crime statistic information for each country:
```SQL
SELECT
	country AS Country,
	crimeRateByCountry_crimeIndex AS CrimeIndexPer100k,
	CrimeRate_OverallCriminalityScoreGOCI AS OverallCrimeScore,
	CrimeRate_CriminalMarketsScore AS CriminalMarketsScore,
	CrimeRate_ResilienceScore AS ResilianceScore,
	CrimeRateSafetyIndex AS SafetyIndex
INTO dbo.CrimeRatesByCountry
FROM dbo.CrimeRate
```

Lastly, I combined all three tables into one master table that I could export into Tableau.
```SQL
SELECT
	dbo.WorldPop.Rank AS PopulationRank,
	dbo.WorldPop.Country,
	dbo.WorldPop.Abbrv,
	dbo.WorldPop.Population,
	dbo.GDP.GDP,
	dbo.CrimeRatesByCountry.CrimeIndexPer100k,
	dbo.CrimeRatesByCountry.OverallCrimeScore,
	dbo.CrimeRatesByCountry.CriminalMarketsScore,
	dbo.CrimeRatesByCountry.ResilianceScore,
	dbo.CrimeRatesByCountry.SafetyIndex
INTO dbo.WorldData
FROM 
	dbo.WorldPop
JOIN
	dbo.GDP ON dbo.WorldPop.Country = dbo.GDP.Country
JOIN
	dbo.CrimeRatesByCountry ON dbo.WorldPop.Country = dbo.CrimeRatesByCountry.Country

SELECT * FROM dbo.WorldData
```

I realzed that when I mapped the Crime Index in Tableau, there were a lot of extra countries that didn't have data for the crime index variable. This is because I used the default Join function, which uses an inner join, meaning that I needed to use a full outer join. I also assigned countries without a GDP entry the population rank of -1, to "keep them out of the way", so to speak.

```SQL
SELECT
    COALESCE(WP.Rank, -1) AS PopulationRank,
    COALESCE(WP.Country, CRC.Country) AS Country,
    WP.Abbrv,
    WP.Population,
    GDP.GDP,
    CRC.CrimeIndexPer100k,
    CRC.OverallCrimeScore,
    CRC.CriminalMarketsScore,
    CRC.ResilianceScore,
    CRC.SafetyIndex
INTO dbo.WorldData2
FROM 
    dbo.WorldPop WP
FULL OUTER JOIN
    dbo.GDP ON WP.Country = dbo.GDP.Country
FULL OUTER JOIN
    dbo.CrimeRatesByCountry CRC ON WP.Country = CRC.Country;

SELECT * FROM dbo.WorldData2
```
