Lab 04 - Visualizing spatial data
================
Zi Li
Feb 5/ due Feb 10

### Load packages and data

``` r
# dsbox installed.

library(tidyverse) 
library(devtools)
# install.packages("devtools")
# devtools::install_github("rstudio-education/dsbox")
library(dsbox)
```

``` r
states <- read_csv("data/states.csv")
```

### Exercise 1

``` r
print(ncol(dennys))
```

    ## [1] 6

``` r
print(nrow(dennys))
```

    ## [1] 1643

# answer for Ex 1:we have 6 columns and 1643 rows. Each row represents one Denny’s location. the variables include latitude, longitude, city, state, and address.

### Exercise 2

``` r
# What are the dimensions of the La Quinta’s dataset? 
print(ncol(laquinta))
```

    ## [1] 6

``` r
print(nrow(laquinta))
```

    ## [1] 909

# answer for Ex 2:we have 6 columns and 909 rows. Each row represents one La Quinta’s location. the variables include latitude, longitude, city, state, and address.

### Exercise 3

``` r
# Take a look at the websites that the data come from.
# answer for Ex.3: Are there any La Quinta’s locations outside of the US? based on the link, I think, no, there's no La Quinta outside the us. 
# What about Denny’s? based on the link, I think there's no Denny's outside the US? but I Google it instead, and it shows there are some Denny's outside of the US, like Canada and Mexico. 
```

…

### Exercise 4

``` r
# Now take a look at the data. What would be some ways of determining whether or not either establishment has any locations outside the US using just the data (and not the websites).

# we can use "filter" function, and I think it's a most straightforward way to use. 
```

…

### Exercise 5

``` r
# filter the Denny’s locations for observations where state is not in states$abbreviation.

dennys %>%
  filter(!(state %in% states$abbreviation))
```

    ## # A tibble: 0 × 6
    ## # ℹ 6 variables: address <chr>, city <chr>, state <chr>, zip <chr>,
    ## #   longitude <dbl>, latitude <dbl>

``` r
# there's no Denny's location outside the US, based on my data. 
```

…

### Exercise 6

``` r
# Add a country variable to the Denny’s dataset and set all observations equal to "United States". 
dennys_us <- dennys %>%
  mutate(country = "United States")
print(ncol(dennys_us))
```

    ## [1] 7

``` r
print(nrow(dennys_us))
```

    ## [1] 1643

…

### Exercise 7

``` r
# Find the La Quinta locations that are outside the US, and figure out which country they are in.

laquinta_outside_us <- laquinta %>%
  filter(!(state %in% states$abbreviation))
# I filter out the locations that are outside the US.
print(ncol(laquinta_outside_us))
```

    ## [1] 6

``` r
print(nrow(laquinta_outside_us))
```

    ## [1] 14

``` r
print(laquinta_outside_us)
```

    ## # A tibble: 14 × 6
    ##    address                                  city  state zip   longitude latitude
    ##    <chr>                                    <chr> <chr> <chr>     <dbl>    <dbl>
    ##  1 Carretera Panamericana Sur KM 12         "\nA… AG    20345    -102.     21.8 
    ##  2 Av. Tulum Mza. 14 S.M. 4 Lote 2          "\nC… QR    77500     -86.8    21.2 
    ##  3 Ejercito Nacional 8211                   "Col… CH    32528    -106.     31.7 
    ##  4 Blvd. Aeropuerto 4001                    "Par… NL    66600    -100.     25.8 
    ##  5 Carrera 38 # 26-13 Avenida las Palmas c… "\nM… ANT   0500…     -75.6     6.22
    ##  6 AV. PINO SUAREZ No. 1001                 "Col… NL    64000    -100.     25.7 
    ##  7 Av. Fidel Velazquez #3000 Col. Central   "\nM… NL    64190    -100.     25.7 
    ##  8 63 King Street East                      "\nO… ON    L1H1…     -78.9    43.9 
    ##  9 Calle Las Torres-1 Colonia Reforma       "\nP… VE    93210     -97.4    20.6 
    ## 10 Blvd. Audi N. 3 Ciudad Modelo            "\nS… PU    75010     -97.8    19.2 
    ## 11 Ave. Zeta del Cochero No 407             "Col… PU    72810     -98.2    19.0 
    ## 12 Av. Benito Juarez 1230 B (Carretera 57)… "\nS… SL    78399    -101.     22.1 
    ## 13 Blvd. Fuerza Armadas                     "con… FM    11101     -87.2    14.1 
    ## 14 8640 Alexandra Rd                        "\nR… BC    V6X1…    -123.     49.2

``` r
# now, just search for these address, I can tell which country they are in. 
```

### Exercise 8

``` r
# Add a country variable to the La Quinta dataset. Use the case_when function to populate this variable.

laquinta_country <- laquinta %>%
  mutate(country = case_when(
   state %in% states$abbreviation ~ "United States",  
    state == "ON" ~ "Canada",  
    state == "BC" ~ "Canada",  
    state == "QC" ~ "Canada",  
    state == "NL" ~ "Canada",   
    state == "MX" ~ "Mexico",  
    TRUE ~ "Unknown"  
  ))
  
print(laquinta_country)
```

    ## # A tibble: 909 × 7
    ##    address                          city  state zip   longitude latitude country
    ##    <chr>                            <chr> <chr> <chr>     <dbl>    <dbl> <chr>  
    ##  1 793 W. Bel Air Avenue            "\nA… MD    21001     -76.2     39.5 United…
    ##  2 3018 CatClaw Dr                  "\nA… TX    79606     -99.8     32.4 United…
    ##  3 3501 West Lake Rd                "\nA… TX    79601     -99.7     32.5 United…
    ##  4 184 North Point Way              "\nA… GA    30102     -84.7     34.1 United…
    ##  5 2828 East Arlington Street       "\nA… OK    74820     -96.6     34.8 United…
    ##  6 14925 Landmark Blvd              "\nA… TX    75254     -96.8     33.0 United…
    ##  7 Carretera Panamericana Sur KM 12 "\nA… AG    20345    -102.      21.8 Unknown
    ##  8 909 East Frontage Rd             "\nA… TX    78516     -98.1     26.2 United…
    ##  9 2116 Yale Blvd Southeast         "\nA… NM    87106    -107.      35.1 United…
    ## 10 7439 Pan American Fwy Northeast  "\nA… NM    87109    -107.      35.2 United…
    ## # ℹ 899 more rows

``` r
# work with the data from the United States only
laquinta_us <- laquinta_country %>% 
 filter(country == "United States")
```

### Exercise 9

``` r
# Which states have the most and fewest Denny’s locations? What about La Quinta?

dennys %>%
  count(state) %>%
  inner_join(states, by = c("state" = "abbreviation"))
```

    ## # A tibble: 51 × 4
    ##    state     n name                     area
    ##    <chr> <int> <chr>                   <dbl>
    ##  1 AK        3 Alaska               665384. 
    ##  2 AL        7 Alabama               52420. 
    ##  3 AR        9 Arkansas              53179. 
    ##  4 AZ       83 Arizona              113990. 
    ##  5 CA      403 California           163695. 
    ##  6 CO       29 Colorado             104094. 
    ##  7 CT       12 Connecticut            5543. 
    ##  8 DC        2 District of Columbia     68.3
    ##  9 DE        1 Delaware               2489. 
    ## 10 FL      140 Florida               65758. 
    ## # ℹ 41 more rows

### Exercise 10

\`\`\`{r-Ex.10} \# Which states have the most Denny’s locations per
thousand square miles? What about La Quinta?

dennys \<- dennys %\>% mutate(establishment = “Denny’s”) laquinta \<-
laquinta %\>% mutate(establishment = “La Quinta”)

dennys_laquinta \<- bind_rows(dennys, laquinta)

ggplot(dennys_laquinta, mapping = aes( x = longitude, y = latitude,
color = establishment )) + geom_point()

    ### Exercise 11
    ```{r-Ex.11}
    # Filter the data for observations in North Carolina only, and recreate the plot. 

    dennys_laquinta %>% 
       filter(state == "NC") %>%
       ggplot(mapping = aes(
       x = longitude,
       y = latitude,
       color = establishment
     )) +
       geom_point(size = 2, alpha = 0.6) + 
       labs(title = "Location of Dennys vs. Laquinta in NC")

    # the joke may not hold true here, I think? based on this plot, I can only see 4 locations that the two are super close to each other.

### Exercise 12

\`\`\`{r-EX.12} \# Now filter the data for observations in Texas only,
and recreate the plot.

dennys_laquinta %\>% filter(state == “TX”) %\>% ggplot(mapping = aes( x
= longitude, y = latitude, color = establishment )) + geom_point(size =
2.5, alpha = 0.25) + labs(title = “Location of Dennys vs. Laquinta in
TX”)

# the joke hold true in TX, they do cluster more tightly here.

\`\`\`
