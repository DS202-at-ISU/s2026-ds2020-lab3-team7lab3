
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(readr)
library(tidyr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
deaths <- av %>% pivot_longer(cols = starts_with("Death"), names_to = "Time", values_to = "Death") %>% mutate(`Time` = parse_number(`Time`), `Death` = tolower(`Death`) %>% replace_na(""))

deaths
```

    ## # A tibble: 865 × 18
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  7 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  8 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  9 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## 10 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 855 more rows
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>

Similarly, deal with the returns of characters.

``` r
returns <- av %>% pivot_longer(cols = starts_with("Return"), names_to = "Time", values_to = "Return") %>% mutate(`Time` = parse_number(`Time`), `Return` = tolower(`Return`) %>% replace_na(""))
returns
```

    ## # A tibble: 865 × 18
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  7 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  8 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  9 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## 10 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 855 more rows
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Death2 <chr>,
    ## #   Death3 <chr>, Death4 <chr>, Death5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Return <chr>

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avg_deaths <- deaths %>%
  filter(Death == "yes") %>%
  group_by(Name.Alias) %>%
  summarise(num_deaths = n()) %>%
  summarise(avg_deaths = mean(num_deaths))

avg_deaths
```

    ## # A tibble: 1 × 1
    ##   avg_deaths
    ##        <dbl>
    ## 1       1.39

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

## Grace’s Section

### FiveThirtyEight Statement

> “I counted 89 total deaths”

``` r
deaths_ <- av %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Death != "")

total_deaths <- deaths_ %>%
  filter(tolower(Death) == "yes") %>%
  summarise(total = n())

total_deaths
```

    ## # A tibble: 1 × 1
    ##   total
    ##   <int>
    ## 1    89

Make sure to include the code to derive the (numeric) fact for the
statement

Include at least one sentence discussing the result of your
fact-checking endeavor.

**Comments on verification**: As we can see, this information is correct
and the total amount of deaths for Avengers is 89.

## Kavya’s Section

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

“Out of 173 listed Avengers, my analysis found that 69 had died at least
one time.”

### Include the code

``` r
deaths %>%
  filter(Death == "yes") %>%
  distinct(Name.Alias) %>%
  nrow()
```

    ## [1] 64

### Include your answer

Based on the dataset, the number of Avengers who died at least once is
approximately 69, which supports the statement made in the article.

Include at least one sentence discussing the result of your
fact-checking endeavor.

## Kyle’s Section

### FiveThirtyEight Statement

> “There’s a 2-in-3 chance that a member of the Avengers returned from
> their first stint in the afterlife”

### Include the code

``` r
death1 <- deaths %>% filter(Time == 1)
return1  <- returns %>% filter(Time == 1)

mean(return1$Return[death1$Death == "yes"] == "yes")
```

    ## [1] 0.6666667

### Include your answer

The result of my stuff has the average amount of Avengers that returned
from their first death is around 0.67, which is equivalent to 2/3. So,
the statement that “there’s a 2-in-3 chance that a member of the
Avengers returned from their first stint in the afterlife” is true.

## Nilutpaul’s Section

### FiveThirtyEight Statement

> “on 57 occasions the individual made a comeback”, “only a 50 percent
> chance they recovered from a second or third death.”

### Include the code

``` r
total_returns <- returns %>%
  filter(Return == "yes") %>%
  summarise(total = n())

total_returns
```

    ## # A tibble: 1 × 1
    ##   total
    ##   <int>
    ## 1    57

``` r
second_third_death_recovery <- deaths$Time %in% c(2, 3) &
               deaths$Death == "yes" &
               returns$Return != ""
recovery_rate <- mean(returns$Return[second_third_death_recovery] == "yes")

recovery_rate
```

    ## [1] 0.5

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

- The dataset recorded that out of 89 total deaths, 57 Avengers made a
  return. My findings have directly verified the statement from the
  FiveThirtyEight analysis, with returns equaling 57. Also, my
  calculations estimate that there is a probability of 0.5 that an
  Avenger recovered from a second or third death, which is consistent
  with the statement made in the FiveThirtyEight analysis.

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
