This is a simple dashboard I created using Tableau to display information about Netflix data.

![image](https://private-user-images.githubusercontent.com/169910014/337406313-21726f8d-7803-4549-9c6c-2c73b6ae401c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTc3MDE2MTgsIm5iZiI6MTcxNzcwMTMxOCwicGF0aCI6Ii8xNjk5MTAwMTQvMzM3NDA2MzEzLTIxNzI2ZjhkLTc4MDMtNDU0OS05YzZjLTJjNzNiNmFlNDAxYy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNjA2JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDYwNlQxOTE1MThaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lZDg2YThhMWJmM2E2YTAwYzg3ODNmMTJjMTY4ZDBhZWFkZWZhNTViMDRjMTdmYWZlYmE3NjU5MmM0YjkyNzZjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.EV1vbonCfTSM4t09l2Pi54RreXp_Pf43TACAGxgprSk)

[Link to Tableau Dashboard (Please View in Fullscreen)](https://public.tableau.com/views/NetflixData_17163010300100/Dashboard2?:language=en-US&publish=yes&:sid=&:display_count=n&:origin=viz_share_link)

This is the code I used in SQL to gather and organize the data:


Displaying data and searching for null entries:

```SQL
SELECT * FROM  dbo.Netflix

SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo' AND TABLE_NAME = 'Netflix';
```

There were quite a lot of null entries for age certification, and as I didn't want to delete all the 
entries, I deleted the age certification column completely to reduce
entries with null values, since I didn't need the data for that anyway.

```SQL
SELECT * FROM dbo.Netflix 
WHERE 
    [description] IS NULL OR 
    [index] IS NULL OR 
    [id] IS NULL OR 
    [title] IS NULL OR 
    [type] IS NULL OR 
    [release_year] IS NULL OR 
    [age_certification] IS NULL OR 
    [runtime] IS NULL OR 
    [imdb_id] IS NULL OR 
    [imdb_score] IS NULL OR 
    [imdb_votes] IS NULL;



ALTER TABLE dbo.Netflix
DROP COLUMN age_certification;
```
Now I wanted to see what else had null entries, not that the age_certification column no longer appeared.

```SQL
SELECT * FROM dbo.Netflix 
WHERE 
    [description] IS NULL OR 
    [index] IS NULL OR 
    [id] IS NULL OR 
    [title] IS NULL OR 
    [type] IS NULL OR 
    [release_year] IS NULL OR
    [runtime] IS NULL OR 
    [imdb_id] IS NULL OR 
    [imdb_score] IS NULL OR 
    [imdb_votes] IS NULL;
```
A few random foreign films with no description appear. I want to know what they are.

```SQL
SELECT *
FROM dbo.Netflix
WHERE [description] IS NULL;
```
5 random foreign films appear, and I got rid of them.

```SQL
DELETE FROM dbo.Netflix
WHERE [description] IS NULL;
```
