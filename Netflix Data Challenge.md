This is a simple dashboard I created using Tableau to display information about Netflix data.

![image](https://github.com/NMangi1/NMangi1/assets/169910014/6a66f2c7-2c83-4b7e-acc9-c5c2dff915d3)

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
