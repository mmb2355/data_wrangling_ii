Strings and Factors
================
2026-10-26

## strings and regex

``` r
string_vec = c("my", "name", "is", "meghan")

str_detect(string_vec, "meghan")
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
str_replace(string_vec, "meghan", "Meghan")
```

    ## [1] "my"     "name"   "is"     "Meghan"

- `str_detect`: detects patterns of character vectors (words, letters,
  number) that is either true or false. CASE SENSITIVE!
- so meghan is true 1 out of the 4 vectors, m is true 3 out of 4, etc.
- `str_replace`: replaces character vector with whatever character
  word/letter/number

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

``` r
str_detect(string_vec, "i think$")
```

    ## [1] FALSE FALSE FALSE  TRUE

- `^` at the beginning of the phrase line, so vectors that start with “i
  think”
- `$` at the end of the phrase line, so vectors that ends with “i think”

``` r
string_vec = c(
  "Time for a Pumpkin Spice Latte!",
  "went to the #pumpkinpatch last weekend",
  "Pumpkin Pie is obviously the best pie",
  "SMASHING PUMPKINS -- LIVE IN CONCERT!!"
  )

str_detect(string_vec,"[Pp]umpkin")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

- `[]` grabs lower- and uppercase…NOT case sensitive!

``` r
string_vec = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )

str_detect(string_vec, "^[0-9][a-zA-Z]")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

- number first, followed by a letter after (ex. rankings)

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

``` r
str_detect(string_vec, "7\\.11")
```

    ## [1] FALSE FALSE FALSE  TRUE

- adding a `.` is a special character that captures a character between
  the specified number/letter
- `\\` is a special character and it gets the exact character, no other
  combinations

``` r
string_vec = c(
  'The CI is [2, 5]',
  ':-]',
  ':-[',
  'I found the answer on pages [6-7]'
  )

str_detect(string_vec, "\\[")
```

    ## [1]  TRUE FALSE  TRUE  TRUE

## factors

``` r
factor_vec = factor(c("male", "male", "female", "female"))

as.numeric(factor_vec)
```

    ## [1] 2 2 1 1

- `as.numeric`: converts character vector into values

what happens if i relevel?

``` r
factor_vec = fct_relevel(factor_vec, "male")

as.numeric(factor_vec)
```

    ## [1] 1 1 2 2

- `fct_relevel`: reassigns factor character to a position…before, values
  are automatically are ordered alphabetically
