This is a Tableu story that I created regarding the Relative Age Effect in Hockey.

![image](https://github.com/NMangi1/NMangi1/assets/169910014/dd472be1-0e3b-45fd-9229-272b2752f39c)


![image](https://github.com/NMangi1/NMangi1/assets/169910014/70d62141-9374-49a3-8dfd-d358d9b0dddb)


[Link to Tableau Story](https://public.tableau.com/app/profile/nicholas.mangione1725/viz/HockeyRAE/Story1)

In many youth hockey leagues, eligibility for age groups is determined by calendar year, typically January 1st. This means that players born in the early months of the year (e.g., January to March) are among the oldest in their age group, while those born later (e.g., October to December) are among the youngest. The older players may have physical and developmental advantages over their younger counterparts, leading to increased opportunities for selection and development. However, the relative age effect does not seem to go away as players enter adulthood. Remnants of the RAE in Ice Hockey can be seen all the way up to the NHL, as shown above.

To begin, I gathered data on OHL Players and their birthdays. I previewed and did some simple exploratory analysis using XLOOKUP and PivotTables in Excel, and had no issues putting it into Tableau.

The NHL data, however, gave me problems. The birthdays AND stats for the players could only be gotten from seperate tables. One table used one column for the name, while the other broke it into two (first, last).
To fix this issue, I made a new column that used the naming format of the first table in order to join the two tables on the name column.

```SQL
SELECT * FROM dbo.StatsCareer 

SELECT 
    CONCAT(FirstName, ' ', LastName) AS FullName FROM dbo.info

ALTER TABLE dbo.info
ADD FullName NVARCHAR(255);

UPDATE dbo.info
SET FullName = CONCAT(FirstName, ' ', LastName)
```

Now I can join the tables on players' last names.

```SQL
SELECT 
    i.FirstName, 
    i.LastName, 
    i.Birthdate, 
    c.P,
	c.GP,
	c.G,
	c.A,
	c.PIM,
	c.PM
INTO 
    dbo.PlayerInfo
FROM 
    dbo.info AS i
INNER JOIN 
    dbo.StatsCareer AS c
ON 
    i.FullName = c.Name
```
I also used a number of calculated fields in Tableu, in order to be able to work with the month formatting:

```SQL
DATENAME('month', MAKEDATE(2000, [Birth Month], 1))
```

And assign each month to its subsequent quarter:

```SQL
IF [Name of Month] = "January" THEN "Q1"
ELSEIF [Name of Month] = "February" THEN "Q1"
ELSEIF [Name of Month] = "March" THEN "Q1"
ELSEIF [Name of Month] = "April" THEN "Q2"
ELSEIF [Name of Month] = "May" THEN "Q2"
ELSEIF [Name of Month] = "June" THEN "Q2"
ELSEIF [Name of Month] = "July" THEN "Q3"
ELSEIF [Name of Month] = "August" THEN "Q3"
ELSEIF [Name of Month] = "September" THEN "Q3"
ELSEIF [Name of Month] = "October" THEN "Q4"
ELSEIF [Name of Month] = "November" THEN "Q4"
ELSEIF [Name of Month] = "December" THEN "Q4"
END
```

A more detailed exploration of the statistical analysis conducted can be found [here](https://github.com/NMangi1/NMangi1/blob/main/Relative%20Age%20Effect%20(R%20Tests).md).
