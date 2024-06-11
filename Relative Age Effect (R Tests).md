To ensure that the relative age effect was indeed impacting hockey players according to their birth months, I conducted exploratory tests to find out if the distribution of successful hockey players according to their birth month was statistically significant.

To begin, I collected the data by using a webscraper written in R:

```R
library(rvest)

url <- "https://www.nhl.com/hurricanes/roster"

page <- read_html(url)

roster_table <- page %>%
  html_nodes("table.table") %>%
  html_table(fill = TRUE)

roster_df <- as.data.frame(roster_table[[1]])

print(head(roster_df))
```

I repeated this with each teams roster, and read the scraped data into R. I assigned column names and previewed.

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
The output showed the following:

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

For the Chi-Square test, I want to create the expected frequency for each month, assuming a uniform distribution.

```R
total_players <- sum(birth_month_counts$count)
print(total_players)

expected_frequencies <- rep(total_players / 12, 12)
```
I performed the Chi-Square test:

```R
chi_square_test <- chisq.test(birth_month_counts$count, p = expected_frequencies / total_players)
```

I got the following output: 

X-squared = 154.05, df = 11, p-value < 2.2e-16

Overall, the output suggests that there is a significant deviation from the expected distribution of birth months in hockey, indicating the presence of a relative age effect (RAE) where players born earlier in the year are overrepresented compared to players born later in the year.

Lastly, before exporting the data to Tableau, I added a new column for month names and wrote the new file out to my desktop.

```R
month_names <- month.abb[unique(NHLData$BirthMonth)]
NHLData$MonthName <- month_names[NHLData$BirthMonth]

output_file_path <- "C:/Users/User1/Downloads/NHLData_with_MonthNames.csv"
write.csv(NHLData, file = output_file_path, row.names = FALSE)
```

Accompanying visualizations with SQL code can be found [here](https://github.com/NMangi1/NMangi1/blob/main/Relative%20Age%20Effect%20(Hockey).md).


