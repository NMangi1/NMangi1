To ensure that the relative age effect was indeed impacting hockey players according to their birth months, I conducted exploratory tests to find out if the distribution of successful hockey players according to their birth month was statistically significant.

To begin, I collected NHL data and read it into R. I assigned column names and previewed.

```R
file_path <- "C:/Users/NikMa/Downloads/NHLDATA.csv"
NHLData <- read.csv(file_path, header = FALSE)
head(NHLData)

colnames(NHLData) = c("PlayerID", "FirstName", "LastName", "Nationality", "BirthCity", "Position", 
                      "Birthdate", "StateOrProvince", "Height", "Height_cm", "Weight", "Handedness")

head(NHLData)
```

Next I converted the Date of Birth into the format I wanted it to be in.

```R
NHLData$Birthdate <- ymd_hms(NHLData$Birthdate)
```

I then extracted the month from the birthdate, and sorted the data by birth month.

```R
NHLData$BirthMonth <- month(NHLData$Birthdate)
head(NHLData$BirthMonth)

NHLData_sorted <- NHLData %>%
  arrange(BirthMonth)

head(NHLData_sorted)
```

I grouped players by birthmonth and counted the amounts in each month.

```R
birth_month_counts <- NHLData %>%
  group_by(BirthMonth) %>%
  summarise(count = n())

print(birth_month_counts)
```
| BirthMonth | Count |
| ---------- | ----- |
|     1      |  407  |
|     2      |  406  |
|     3      |  398  |
|     4      |  379  | 
|     5      |  377  |
|     6      |  345  |
|     7      |  333  |
|     8      |  258  |
|     9      |  272  |
|    10      |  271  |
|    11      |  225  |
|    12      |  251  |

