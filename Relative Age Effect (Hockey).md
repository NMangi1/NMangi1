This is a Tableu story that I created regarding the Relative Age Effect in Hockey.

![image](https://private-user-images.githubusercontent.com/169910014/337406771-ab2b38f2-33a0-43f9-b9a0-f8fe8a9bca15.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTc3MDE3MzMsIm5iZiI6MTcxNzcwMTQzMywicGF0aCI6Ii8xNjk5MTAwMTQvMzM3NDA2NzcxLWFiMmIzOGYyLTMzYTAtNDNmOS1iOWEwLWY4ZmU4YTliY2ExNS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNjA2JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDYwNlQxOTE3MTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yOWE0OTk0OTJkZWViZjUxMDU5Zjk4MTA5Y2UzMDE0OGNhMDA5ODVmYzJlMWFiY2M4ZjhjZTk2OTUxZTFhOTVjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.yI1owTNEti6Rm2tHHuWmEMbgsqYmovk3eI75tBXD7O0)

![image](https://private-user-images.githubusercontent.com/169910014/337406844-60d43e24-1c33-468a-bf66-11d895adef37.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTc3MDE4MTcsIm5iZiI6MTcxNzcwMTUxNywicGF0aCI6Ii8xNjk5MTAwMTQvMzM3NDA2ODQ0LTYwZDQzZTI0LTFjMzMtNDY4YS1iZjY2LTExZDg5NWFkZWYzNy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNjA2JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDYwNlQxOTE4MzdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iZGNiNTM2NzRhNmUwOGMzZWE4NmY0NDg0Y2I1ODY4ZjZjZDg0YzRkNTBmMWI4NjZjOTdkZjBiZjBjN2IwNmQ2JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.lTgun5XeGFRb6IEy33SGXk3o3lWqddVo7UdPhxJ6JGU)

[Link to Tableau Story](https://public.tableau.com/app/profile/nicholas.mangione1725/viz/HockeyRAE/Story1)

In many youth hockey leagues, eligibility for age groups is determined by calendar year, typically January 1st. This means that players born in the early months of the year (e.g., January to March) are among the oldest in their age group, while those born later (e.g., October to December) are among the youngest. The older players may have physical and developmental advantages over their younger counterparts, leading to increased opportunities for selection and development. However, the relative age effect does not seem to go away as players enter adulthood. Remnants of the RAE in Ice Hockey can be seen all the way up to the NHL, as shown above.

To begin, I gathered data on OHL Players and their birthdays. I previewed in Excel, and had no issues putting it into Tableau.

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

Now I can join the tables.

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
