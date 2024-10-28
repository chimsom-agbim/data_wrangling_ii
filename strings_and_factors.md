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