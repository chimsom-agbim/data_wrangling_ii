Strings and Factors
================
2024-10-28

## Strings and regex

Okay! So a string is a vector of characters.That’s the best definition!
You can use the str_detect function to find a pattern between the
character and other components of the vector. Also make sure you run the
libraries above so the functions below work! For example, below, the
result told me hey, there’s a jeff in the 4th position. Regex just
stands for regular expression.

``` r
string_vec = c("my", "name", "is", "jeff")
str_detect(string_vec, "jeff")
```

    ## [1] FALSE FALSE FALSE  TRUE

I can also search for the letter “e” in the vector. There are 2
characters in the vector where this is the case and that is what it is
putting out.

``` r
string_vec = c("my", "name", "is", "jeff")
str_detect(string_vec, "e")
```

    ## [1] FALSE  TRUE FALSE  TRUE

Another try. This time it’s all false because there is no capital J!

``` r
string_vec = c("my", "name", "is", "jeff")
str_detect(string_vec, "J")
```

    ## [1] FALSE FALSE FALSE FALSE

You can also replace components of the string. You enter the first
character you have and then the character that you want to replace it
with! If I wanted to remove that character, I could just put and empty
quotes “” as the argument in the replacement column

``` r
string_vec = c("my", "name", "is", "jeff")
str_replace(string_vec, "jeff", "Jeff")
```

    ## [1] "my"   "name" "is"   "Jeff"

You can also detect phrases in different locations.

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )
str_detect(string_vec, "i think")
```

    ## [1] TRUE TRUE TRUE TRUE

What if I only want phrases that START with “i think”? I can specify
that as well. The carrot says only pick phrases that start with this
value.

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )
str_detect(string_vec, "^i think")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

I can also check for the case where “i think” is at the end of the
string.

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )
str_detect(string_vec, "i think$")
```

    ## [1] FALSE FALSE FALSE  TRUE

We have another example. Here, we see that there are many expressions
with the word Bush in it. BUT since the `str_detect` function is case
sensitive, how do I detect all versions? Use brackets saying what the
options are for the first letter.

``` r
string_vec = c("Y'all remember Pres. HW Bush?", 
               "I saw a green bush", 
               "BBQ and Bushwalking at Molongo Gorge", 
               "BUSH -- LIVE IN CONCERT!!")
str_detect(string_vec, "[Bb]ush")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

I can also code for more complicated cases. Here, I tell the code to
find all cases where I start with a number and then have a letter
adjacent. I list the range of numbers and then the range of letters and
specify whether it’s capitalized or not.

``` r
string_vec = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )

str_detect(string_vec, "[0-9][a-zA-Z]")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

This case is a bit confusing because here the syntax for r conflicts
with some of the characters in the code (it could look misleading), but
the code below is written to detect cases where there is 7, an adjacent
character, and then a 1. That’s how r is reading what I’ve typed. And
that’s why it returns true true false true.

Alternatively, I could try to enter the brackets used above.

``` r
string_vec = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )

str_detect(string_vec, "7.11")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

But what if I wanted 7.11? \\ says treat this dot like a dot, not a
place holder for a special character like above.

``` r
string_vec = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )

str_detect(string_vec, "7\\.11")
```

    ## [1] FALSE FALSE FALSE  TRUE

## Factors

So the `factor()` function takes a vector of characters and assigns
levels(aka numbers) to it based on the alphabetical order of those
characters. It placed female first because it starts with f. But it also
assigns numbers based on this order using the `as.numeric` function.

``` r
vec_sex = factor(c("male", "male", "female", "female"))
vec_sex
```

    ## [1] male   male   female female
    ## Levels: female male

``` r
as.numeric(vec_sex)
```

    ## [1] 2 2 1 1

I can also tell R to make the first level to be “male”.

``` r
vec_sex = factor(c("male", "male", "female", "female"))
vec_sex = fct_relevel(vec_sex, "male")
vec_sex
```

    ## [1] male   male   female female
    ## Levels: male female

``` r
as.numeric(vec_sex)
```

    ## [1] 1 1 2 2

## NSDUH

This is an online database from the national survey on drug use and
health comparison. Wow I was finally able to extract the first table
from this data set! If you look at the set, the 12+, 12-17, 18-25
corresponds to the age range.

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

table_marj = 
  read_html(url) |> 
  html_table() |> 
  first() |>
  slice(-1)

table_marj
```

    ## # A tibble: 56 × 16
    ##    State     `12+(2013-2014)` `12+(2014-2015)` `12+(P Value)` `12-17(2013-2014)`
    ##    <chr>     <chr>            <chr>            <chr>          <chr>             
    ##  1 Total U.… 12.90a           13.36            0.002          13.28b            
    ##  2 Northeast 13.88a           14.66            0.005          13.98             
    ##  3 Midwest   12.40b           12.76            0.082          12.45             
    ##  4 South     11.24a           11.64            0.029          12.02             
    ##  5 West      15.27            15.62            0.262          15.53a            
    ##  6 Alabama   9.98             9.60             0.426          9.90              
    ##  7 Alaska    19.60a           21.92            0.010          17.30             
    ##  8 Arizona   13.69            13.12            0.364          15.12             
    ##  9 Arkansas  11.37            11.59            0.678          12.79             
    ## 10 Californ… 14.49            15.25            0.103          15.03             
    ## # ℹ 46 more rows
    ## # ℹ 11 more variables: `12-17(2014-2015)` <chr>, `12-17(P Value)` <chr>,
    ## #   `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>, `18-25(P Value)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `26+(P Value)` <chr>,
    ## #   `18+(2013-2014)` <chr>, `18+(2014-2015)` <chr>, `18+(P Value)` <chr>

You can also remove columns. Here I am removing the P value column.

``` r
data_marj = 
  table_marj |>
  select(-contains("P Value")) 
data_marj
```

    ## # A tibble: 56 × 11
    ##    State `12+(2013-2014)` `12+(2014-2015)` `12-17(2013-2014)` `12-17(2014-2015)`
    ##    <chr> <chr>            <chr>            <chr>              <chr>             
    ##  1 Tota… 12.90a           13.36            13.28b             12.86             
    ##  2 Nort… 13.88a           14.66            13.98              13.51             
    ##  3 Midw… 12.40b           12.76            12.45              12.33             
    ##  4 South 11.24a           11.64            12.02              11.88             
    ##  5 West  15.27            15.62            15.53a             14.43             
    ##  6 Alab… 9.98             9.60             9.90               9.71              
    ##  7 Alas… 19.60a           21.92            17.30              18.44             
    ##  8 Ariz… 13.69            13.12            15.12              13.45             
    ##  9 Arka… 11.37            11.59            12.79              12.14             
    ## 10 Cali… 14.49            15.25            15.03              14.11             
    ## # ℹ 46 more rows
    ## # ℹ 6 more variables: `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `18+(2013-2014)` <chr>,
    ## #   `18+(2014-2015)` <chr>

Remember the pivot function? We can use that to reorganize the columns
so it’s more palatable and I’m going to organize everything that’s not
state (hence the -state), and I want the column name to be age_year
because that’s how it’s titled.

``` r
data_marj = 
  table_marj |>
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent")
data_marj
```

    ## # A tibble: 560 × 3
    ##    State      age_year         percent
    ##    <chr>      <chr>            <chr>  
    ##  1 Total U.S. 12+(2013-2014)   12.90a 
    ##  2 Total U.S. 12+(2014-2015)   13.36  
    ##  3 Total U.S. 12-17(2013-2014) 13.28b 
    ##  4 Total U.S. 12-17(2014-2015) 12.86  
    ##  5 Total U.S. 18-25(2013-2014) 31.78  
    ##  6 Total U.S. 18-25(2014-2015) 32.07  
    ##  7 Total U.S. 26+(2013-2014)   9.63a  
    ##  8 Total U.S. 26+(2014-2015)   10.25  
    ##  9 Total U.S. 18+(2013-2014)   12.87a 
    ## 10 Total U.S. 18+(2014-2015)   13.41  
    ## # ℹ 550 more rows

Now let’s say I want to separate the age_year column a little. It
actually made that one column into 2 columns, and I told it to separate
when it got to the open parenthesis.

``` r
data_marj = 
  table_marj |>
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(")
data_marj
```

    ## # A tibble: 560 × 4
    ##    State      age   year       percent
    ##    <chr>      <chr> <chr>      <chr>  
    ##  1 Total U.S. 12+   2013-2014) 12.90a 
    ##  2 Total U.S. 12+   2014-2015) 13.36  
    ##  3 Total U.S. 12-17 2013-2014) 13.28b 
    ##  4 Total U.S. 12-17 2014-2015) 12.86  
    ##  5 Total U.S. 18-25 2013-2014) 31.78  
    ##  6 Total U.S. 18-25 2014-2015) 32.07  
    ##  7 Total U.S. 26+   2013-2014) 9.63a  
    ##  8 Total U.S. 26+   2014-2015) 10.25  
    ##  9 Total U.S. 18+   2013-2014) 12.87a 
    ## 10 Total U.S. 18+   2014-2015) 13.41  
    ## # ℹ 550 more rows

I can also code to get rid of the closed parenthesis by telling R to
replace it with nothing.

``` r
data_marj = 
  table_marj |>
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(") |>
 mutate(
    year = str_replace(year, "\\)", ""))

data_marj
```

    ## # A tibble: 560 × 4
    ##    State      age   year      percent
    ##    <chr>      <chr> <chr>     <chr>  
    ##  1 Total U.S. 12+   2013-2014 12.90a 
    ##  2 Total U.S. 12+   2014-2015 13.36  
    ##  3 Total U.S. 12-17 2013-2014 13.28b 
    ##  4 Total U.S. 12-17 2014-2015 12.86  
    ##  5 Total U.S. 18-25 2013-2014 31.78  
    ##  6 Total U.S. 18-25 2014-2015 32.07  
    ##  7 Total U.S. 26+   2013-2014 9.63a  
    ##  8 Total U.S. 26+   2014-2015 10.25  
    ##  9 Total U.S. 18+   2013-2014 12.87a 
    ## 10 Total U.S. 18+   2014-2015 13.41  
    ## # ℹ 550 more rows

Let’s do the same for the percent column. I want to get rid of those
extra letters. Recall, putting the dollar sign means anything that ends
with that character. (and carrot means starts with). I can also
simultaneously change the percent column to be a number (double) instead
of a character

``` r
data_marj = 
  table_marj |>
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(") |>
  mutate(
    year = str_replace(year, "\\)", ""),
    percent = str_replace(percent, "[a-c]$", ""), 
    percent = as.numeric(c(percent))
  )
data_marj
```

    ## # A tibble: 560 × 4
    ##    State      age   year      percent
    ##    <chr>      <chr> <chr>       <dbl>
    ##  1 Total U.S. 12+   2013-2014   12.9 
    ##  2 Total U.S. 12+   2014-2015   13.4 
    ##  3 Total U.S. 12-17 2013-2014   13.3 
    ##  4 Total U.S. 12-17 2014-2015   12.9 
    ##  5 Total U.S. 18-25 2013-2014   31.8 
    ##  6 Total U.S. 18-25 2014-2015   32.1 
    ##  7 Total U.S. 26+   2013-2014    9.63
    ##  8 Total U.S. 26+   2014-2015   10.2 
    ##  9 Total U.S. 18+   2013-2014   12.9 
    ## 10 Total U.S. 18+   2014-2015   13.4 
    ## # ℹ 550 more rows

Now! Let’s say I want to get rid of the Total U.S. rows and only have
the state-specific data. Use the filter function- you enter the `!`
syntax to say take this out, and we are accessing the state column, and
within those columns, you want to filter out the following row of
characters.

``` r
data_marj = 
  table_marj |>
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(") |>
  mutate(
    year = str_replace(year, "\\)", ""),
    percent = str_replace(percent, "[a-c]$", ""),
    percent = as.numeric(percent)) |>
  filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))

data_marj
```

    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    9.98
    ##  2 Alabama 12+   2014-2015    9.6 
    ##  3 Alabama 12-17 2013-2014    9.9 
    ##  4 Alabama 12-17 2014-2015    9.71
    ##  5 Alabama 18-25 2013-2014   27.0 
    ##  6 Alabama 18-25 2014-2015   26.1 
    ##  7 Alabama 26+   2013-2014    7.1 
    ##  8 Alabama 26+   2014-2015    6.81
    ##  9 Alabama 18+   2013-2014    9.99
    ## 10 Alabama 18+   2014-2015    9.59
    ## # ℹ 500 more rows

You can also use the filter function to INCLUDE only certain
information.

``` r
data_marj =
  data_marj %>% 
  filter(age == "12-17") 
data_marj
```

    ## # A tibble: 102 × 4
    ##    State      age   year      percent
    ##    <chr>      <chr> <chr>       <dbl>
    ##  1 Alabama    12-17 2013-2014    9.9 
    ##  2 Alabama    12-17 2014-2015    9.71
    ##  3 Alaska     12-17 2013-2014   17.3 
    ##  4 Alaska     12-17 2014-2015   18.4 
    ##  5 Arizona    12-17 2013-2014   15.1 
    ##  6 Arizona    12-17 2014-2015   13.4 
    ##  7 Arkansas   12-17 2013-2014   12.8 
    ##  8 Arkansas   12-17 2014-2015   12.1 
    ##  9 California 12-17 2013-2014   15.0 
    ## 10 California 12-17 2014-2015   14.1 
    ## # ℹ 92 more rows

Now, the data set lists (and would plot) the state variable in
alphabetical order. If I wanted to change the display order so it
reflected the percentage, as opposed to the name, I could use the factor
reorder function, telling it to reorganize state by percent in the plot.

``` r
data_marj =
  data_marj %>% 
  filter(age == "12-17") %>% 
  mutate(State = fct_reorder(State, percent)) %>% 
ggplot(aes(x = State, y = percent, color = year)) + 
    geom_point() + 
    theme(axis.text.x = element_text(angle = 90, hjust = 1))

data_marj
```

<img src="strings_and_factors_files/figure-gfm/unnamed-chunk-22-1.png" width="90%" />
