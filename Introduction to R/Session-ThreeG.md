R Basics - Session 3: Data Processing / Simulation and Structuring
================
Cristhian Rodriguez Revilla

- [Data Processing in Finance](#data-processing-in-finance)
  - [Embracing Data Simulation](#embracing-data-simulation)
    - [Brownian Motion for Simulating Stock Prices and
      Returns](#brownian-motion-for-simulating-stock-prices-and-returns)
    - [Task: Simulating Prices and
      Returns](#task-simulating-prices-and-returns)
  - [Data Manipulation dplyr tdyr](#data-manipulation-dplyr-tdyr)
    - [Task Design: Enhancing Financial Data
      Analysis](#task-design-enhancing-financial-data-analysis)
  - [Embracing Data Processing: Unstructured
    Approach](#embracing-data-processing-unstructured-approach)
    - [Tailoring map Functions to Data
      Types](#tailoring-map-functions-to-data-types)
    - [Organizing Financial Data with data
      frames](#organizing-financial-data-with-data-frames)
    - [Conditional Mapping](#conditional-mapping)
    - [Task: Analyzing Loan Portfolio
      Risk](#task-analyzing-loan-portfolio-risk)

<style>
.img-watermark {
    position: fixed;
    bottom: 10px;
    right: 10px;
    opacity: 0.3; /* Adjust the opacity as needed */
    font-size: 20px;
    transform: rotate(-45deg);
    z-index: 9999;
    max-width: 100px; /* Adjust size as needed */
    max-height: auto;
}
</style>

<img src="logo.png" alt="Watermark" class="img-watermark"/>

# Data Processing in Finance

Data processing stands as the bedrock upon which insightful financial
analysis is built. Before delving into complex statistical models or
investment strategies, understanding and preparing your data is crucial.
This session will guide you through data simulation and manipulation,
focusing on key financial metrics such as returns and risk, to enhance
decision-making processes in finance.

## Embracing Data Simulation

In the realm of finance, predicting the behavior of key variables like
EBITDA, stock returns, or portfolio risk involves grappling with
uncertainty and market efficiency. Simulation plays a pivotal role,
enabling us to forecast potential outcomes and stress-test financial
models under various scenarios.

To simulate stock returns, we often rely on defining specific
parameters: the mean (μ) representing the expected return level, and the
standard deviation (σ) capturing price volatility.

![\Delta \\ P =\mu +\sigma \epsilon, \\ \epsilon \sim N (0,1)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5CDelta%20%5C%25%20P%20%3D%5Cmu%20%2B%5Csigma%20%5Cepsilon%2C%20%5C%3B%20%5Cepsilon%20%5Csim%20N%20%280%2C1%29 "\Delta \% P =\mu +\sigma \epsilon, \; \epsilon \sim N (0,1)")

``` r
# Lets simulate returns
mu=0; sigma=0.075
set.seed(10) # To have similar number (only for replication purposes)
R=mu+sigma*rnorm(5) # Normal distribution
print(R)
```

    ## [1]  0.001405963 -0.013818941 -0.102849791 -0.044937579  0.022090884

R generates random variables by starting with uniformly distributed
seeds. These seeds are essentially the initial inputs for algorithms
that produce sequences of numbers that appear random. From these uniform
distributions, R can transform the numbers into virtually any
distribution we require for our simulations, most commonly the normal
distribution. This transformation is facilitated by what’s known as a
Distribution Function Kernel, allowing us to simulate real-world
phenomena where outcomes follow a predictable pattern or distribution.

### Brownian Motion for Simulating Stock Prices and Returns

One of the cornerstone methods for simulating stock prices and returns
is through [Brownian
Motion](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_equation).
This concept might sound complex, but it’s essentially a mathematical
model used to describe random motion, like the unpredictable movement of
a particle in a fluid. In finance, [Brownian
Motion](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_equation)
helps model the seemingly erratic behavior of stock prices over time.

[Brownian
Motion](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_equation) is
described by a stochastic process that accounts for two critical aspects
of financial assets: drift (average rate of return) and volatility (the
degree of variation in returns over time). These elements are
incorporated into the model using the following formula, where the
randomness comes from a [wiener
process](https://en.wikipedia.org/wiki/Wiener_process).

![\Delta Log(S)=(r-\sigma^{2}/2)\Delta t + \sigma \sqrt{\Delta t} N(0,1)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5CDelta%20Log%28S%29%3D%28r-%5Csigma%5E%7B2%7D%2F2%29%5CDelta%20t%20%2B%20%5Csigma%20%5Csqrt%7B%5CDelta%20t%7D%20N%280%2C1%29 "\Delta Log(S)=(r-\sigma^{2}/2)\Delta t + \sigma \sqrt{\Delta t} N(0,1)")

Here, r stands for the risk-free rate (growing drift), σ for volatility,
and N(0,1) denotes a standard normal distribution. The term Δt
represents a small step in time, and together, these variables simulate
the uncertain future path of a stock’s price.

![S\_{t}=S\_{o}\times \exp((r-\sigma^{2}/2)t + \sigma\sqrt{t} N(0,1))](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;S_%7Bt%7D%3DS_%7Bo%7D%5Ctimes%20%5Cexp%28%28r-%5Csigma%5E%7B2%7D%2F2%29t%20%2B%20%5Csigma%5Csqrt%7Bt%7D%20N%280%2C1%29%29 "S_{t}=S_{o}\times \exp((r-\sigma^{2}/2)t + \sigma\sqrt{t} N(0,1))")

### Task: Simulating Prices and Returns

This task is designed to deepen your understanding of financial
simulations by applying the functional form of Returns and Prices for
the Black-Scholes model. This practical application bridges your finance
knowledge with R programming, enhancing your analytical skills in
financial data analysis.

1.  Create a function that simulates Prices based on Black and Scholes
    model.

- Parameters are :
  - r: The risk-free interest rate, representing the cost of capital or
    the return on a risk-free investment over a specified period.

  - σ: Volatility of the stock’s returns, indicating the degree to which
    the stock price is expected to fluctuate.

  - dt: The time increment for each simulation step. The total
    simulation time, calculated as
    (![\tau=n\times dt](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Ctau%3Dn%5Ctimes%20dt "\tau=n\times dt")),
    represents the simulation’s time horizon.

  - So: The initial Price represents the current or recent value of a
    stock.

  - n: Number of simulation wiener observations.
- In the function include the initial values So for prices as t=0, and
  **NA** for returns.

2.  You are tasked with simulating the future behavior of stock prices
    and returns for four different stocks over a 5-month period. Assume
    a monthly risk-free rate of 2% and use the provided stock parameters
    for the simulation assuming dt=1, n=5. Hint: Use mapply function or
    default to for loop.

- Stock Parameters (Inferred or Estimated Using Historical Data). The
  Volatility (sigma) is monthly—the initial Price So (Current Price
  dollars).

  - **StockA**: So=111, sigma=5%

  - **StockB**: So=90, sigma=8%

  - **StockC**: So=70, sigma=14%

  - **StockD**: So=50, sigma=18%

3.  Organize the simulated data into a coherent data frame for analysis.
    This structure should facilitate easy access to the simulated prices
    and returns for each stock, enabling further analysis or reporting.

## Data Manipulation dplyr tdyr

In the fast-paced world of finance, the ability to swiftly manipulate
and analyze historical data can significantly enhance decision-making
processes. The R programming language, particularly with packages like
dplyr and tidyr, offers powerful tools for managing financial datasets.
This section aims to introduce these tools through practical examples,
reflecting scenarios you might encounter as a financial analyst.

``` r
library(DescTools)
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
# Libraries fo data manipulation and Processing
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
library(tidyr)
```

Imagine you’ve simulated financial data or extracted it from a database.
Your dataset, structured as a two-dimensional table or data frame,
contains vital information but requires further processing for analysis.
Let’s explore how to enhance and manipulate this dataset.

First, we’ll add a date column to represent the time series aspect of
our financial data, critical for any temporal analysis.

``` r
# Adding a 'Time' column with monthly intervals
Data <- Data %>%
  group_by(Stock) %>%
  mutate(Time = seq.Date(as.Date("2024-04-28"), by = "month", length.out = 6)) %>%
  ungroup()

# To avoid problem with future transformation always ungroup()
print(Data)
```

    ## # A tibble: 24 × 4
    ##    Stock  Price   Returns Time      
    ##    <chr>  <dbl>     <dbl> <date>    
    ##  1 StockA  111  NA        2024-04-28
    ##  2 StockA  115.  0.0382   2024-05-28
    ##  3 StockA  111. -0.0417   2024-06-28
    ##  4 StockA  111.  0.000566 2024-07-28
    ##  5 StockA  104. -0.0626   2024-08-28
    ##  6 StockA  105.  0.00593  2024-09-28
    ##  7 StockB   90  NA        2024-04-28
    ##  8 StockB  103.  0.135    2024-05-28
    ##  9 StockB  114.  0.0971   2024-06-28
    ## 10 StockB  112. -0.0123   2024-07-28
    ## # ℹ 14 more rows

Next, we leverage lubridate to extract the month from our ‘Time’ column,
facilitating seasonal or monthly analyses.

``` r
# Extracting the 'Month' from 'Time'
Data <- Data %>%
  mutate(Month = month(Time))
print(Data)
```

    ## # A tibble: 24 × 5
    ##    Stock  Price   Returns Time       Month
    ##    <chr>  <dbl>     <dbl> <date>     <dbl>
    ##  1 StockA  111  NA        2024-04-28     4
    ##  2 StockA  115.  0.0382   2024-05-28     5
    ##  3 StockA  111. -0.0417   2024-06-28     6
    ##  4 StockA  111.  0.000566 2024-07-28     7
    ##  5 StockA  104. -0.0626   2024-08-28     8
    ##  6 StockA  105.  0.00593  2024-09-28     9
    ##  7 StockB   90  NA        2024-04-28     4
    ##  8 StockB  103.  0.135    2024-05-28     5
    ##  9 StockB  114.  0.0971   2024-06-28     6
    ## 10 StockB  112. -0.0123   2024-07-28     7
    ## # ℹ 14 more rows

Financial datasets often require categorization based on certain
conditions. Here, we classify our data into seasons, an approach that
could be useful for seasonal performance analysis. You can create
variables based on conditionals. Here the conditions fits to the syntax
of the dplyr package: `ifelse (logical, TRUE, FALSE)` like in excel.

``` r
Data=Data%>%mutate(Season=ifelse(Month %in% c(12, 1, 2), "Winter", 
                                 ifelse(Month %in% c(3,4,5), "Spring",
                                        ifelse(Month %in% c(6,7,8), "Summer",
                                               "Autumn"))))

# Another approach  (not nest conditions)

Data <- Data %>%
  mutate(Season = case_when(
    Month %in% c(12, 1, 2) ~ "Winter",
    Month %in% c(3, 4, 5) ~ "Spring",
    Month %in% c(6, 7, 8) ~ "Summer",
    TRUE ~ "Autumn" # the last logical (TRUE by default: else)
  ))

print(Data)
```

    ## # A tibble: 24 × 6
    ##    Stock  Price   Returns Time       Month Season
    ##    <chr>  <dbl>     <dbl> <date>     <dbl> <chr> 
    ##  1 StockA  111  NA        2024-04-28     4 Spring
    ##  2 StockA  115.  0.0382   2024-05-28     5 Spring
    ##  3 StockA  111. -0.0417   2024-06-28     6 Summer
    ##  4 StockA  111.  0.000566 2024-07-28     7 Summer
    ##  5 StockA  104. -0.0626   2024-08-28     8 Summer
    ##  6 StockA  105.  0.00593  2024-09-28     9 Autumn
    ##  7 StockB   90  NA        2024-04-28     4 Spring
    ##  8 StockB  103.  0.135    2024-05-28     5 Spring
    ##  9 StockB  114.  0.0971   2024-06-28     6 Summer
    ## 10 StockB  112. -0.0123   2024-07-28     7 Summer
    ## # ℹ 14 more rows

For focused analyses, such as examining summer performance, filtering
and selecting relevant data subsets is essential.

``` r
# Filtering data for 'Summer' season
SummerData <- Data %>%
  filter(Season == "Summer") %>%
  select(Time, Stock, Price)
print(SummerData)
```

    ## # A tibble: 12 × 3
    ##    Time       Stock  Price
    ##    <date>     <chr>  <dbl>
    ##  1 2024-06-28 StockA 111. 
    ##  2 2024-07-28 StockA 111. 
    ##  3 2024-08-28 StockA 104. 
    ##  4 2024-06-28 StockB 114. 
    ##  5 2024-07-28 StockB 112. 
    ##  6 2024-08-28 StockB 127. 
    ##  7 2024-06-28 StockC  63.3
    ##  8 2024-07-28 StockC  62.2
    ##  9 2024-08-28 StockC  71.5
    ## 10 2024-06-28 StockD  30.5
    ## 11 2024-07-28 StockD  27.1
    ## 12 2024-08-28 StockD  18.6

Grouping and summarizing data allows for a quick assessment of financial
metrics by category. Here, we calculate average returns and volatility
for each season.

``` r
# Calculating average returns and volatility by 'Season'
SeasonalStats <- Data %>%
  group_by(Season) %>%
  summarise(AvgReturn = mean(Returns, na.rm = TRUE),
            Volatility = sd(Returns, na.rm = TRUE))
# Remember, that statistic functions are sensitive to NA.

print(SeasonalStats)
```

    ## # A tibble: 3 × 3
    ##   Season AvgReturn Volatility
    ##   <chr>      <dbl>      <dbl>
    ## 1 Autumn   -0.0112     0.147 
    ## 2 Spring    0.0231     0.0980
    ## 3 Summer   -0.0652     0.171

As you become more familiar with these tools, you’ll find they
significantly streamline the process of analyzing financial data in R,
aligning with the dynamic needs of the financial industry.

### Task Design: Enhancing Financial Data Analysis

In the dynamic world of finance, mastering data manipulation is key to
uncovering insights that drive strategic decisions. As future financial
analysts, consultants, or managers, you’re often required to process
vast amounts of historical data swiftly and accurately. Imagine you are
part of an economic advisory team tasked with reviewing the performance
of a diverse portfolio. The portfolio comprises stocks across various
sectors, and your objective is to analyze the historical performance
over the last year. This analysis will involve calculating the average
monthly returns, identifying the best-performing stocks each month, and
summarizing the overall volatility.

1.  Download historical data using the **quantmode** library; you can
    consult the tickers of companies in [Yahoo
    Finance](https://finance.yahoo.com/lookup/?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAABvok0n94XCMiLs0FAovhmEnvwNU4MS65yebA9EscGzYfF-J2qVS_FTm3QZroWdVpPpfEyby2JgPbtASpjNM__3kBZHvuzlRGlP0Pf-tmTPUGafztnCZJ-TonxHL5IX4GaYWbESeB83A39rxuzPPjguxLqWxrjv0litnonROj5oK).
    Download the Closing price using the **Cl()** function for 10
    companies of your interest.

- Your data should have the following columns, Date, Ticker, Price,

2.  Data Preparation:

- Generate a **`DateM`** column to represent the monthly Data for each
  trading day.

- Calculate the monthly returns for each stock. Hint: Monthly returns
  can be approximated by comparing the closing prices at the start and
  end of each month.

- For each month, identify the stock with the highest average return.

- Calculate the mean, and standard deviation of daily returns for each
  stock across the year to assess volatility. Then, calculate the Sharpe
  ratio and identify the best stock.

## Embracing Data Processing: Unstructured Approach

The purr package, a part of the tidyverse, is a functional programming
toolkit in R. It enhances R’s capability to manage lists and functions,
making your data analysis more intuitive, expressive, and less
error-prone. Its primary goal is to make your code more readable,
efficient, and easier to debug and maintain. The use of purr becomes
significantly important when dealing with complex list-like structures
or when you need to apply functions iteratively over elements or lists
without resorting to explicit loops, making code more succinct and
expressive.

``` r
library(purrr) # Load the purrr package
```

Consider a scenario where you’re tasked with analyzing the dividends
distributed by companies across different sectors throughout the fiscal
year 2023. The data is unstructured, residing in list form, which is
where `purrr` shines.

``` r
# Example dividends (in millions)
dividends <- list(Tech = c(2, 4, 6, 8, 10), Oil = c(1, 3, 5, 7, 9))

# Calculating the average dividend using map
average_dividends <- map(dividends, mean, na.rm = TRUE)
print(average_dividends) # Output: The mean dividends for Tech and Oil sectors
```

    ## $Tech
    ## [1] 6
    ## 
    ## $Oil
    ## [1] 5

This example illustrates how the `map` function simplifies the process
of applying a function (mean in this case) to each element within a
list.

### Tailoring map Functions to Data Types

**purrr** is versatile, offering variations of the map function to cater
to specific data types, enhancing the flexibility of your data analysis
routines.

``` r
# Defining assets preferences
assets_preference <- list("JP Morgan" = "Equity", "Vanguard" = "Fixed Income")

# Adding a flair to each preference
assets_flair <- assets_preference |>
  map_chr(function(asset) paste(asset, "rocks!"))

print(assets_flair) # Outputs a character vector celebrating each asset preference
```

    ##             JP Morgan              Vanguard 
    ##       "Equity rocks!" "Fixed Income rocks!"

In this snippet, `map_chr` is employed to ensure the output is a
character vector, demonstrating purrr’s ability to tailor its approach
based on the expected outcome.

### Organizing Financial Data with data frames

The `map_dfc` function from the purrr package is instrumental in
iterating over elements (such as stock symbols) and compiling the
results into a dataframe, with each element’s output arranged as a
column.

``` r
# Loading necessary library
library(quantmod)
```

    ## Loading required package: xts

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## 
    ## ######################### Warning from 'xts' package ##########################
    ## #                                                                             #
    ## # The dplyr lag() function breaks how base R's lag() function is supposed to  #
    ## # work, which breaks lag(my_xts). Calls to lag(my_xts) that you type or       #
    ## # source() into this session won't work correctly.                            #
    ## #                                                                             #
    ## # Use stats::lag() to make sure you're not using dplyr::lag(), or you can add #
    ## # conflictRules('dplyr', exclude = 'lag') to your .Rprofile to stop           #
    ## # dplyr from breaking base R's lag() function.                                #
    ## #                                                                             #
    ## # Code in packages is not affected. It's protected by R's namespace mechanism #
    ## # Set `options(xts.warn_dplyr_breaks_lag = FALSE)` to suppress this warning.  #
    ## #                                                                             #
    ## ###############################################################################

    ## 
    ## Attaching package: 'xts'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     first, last

    ## Loading required package: TTR

    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo

``` r
# Define stock symbols and date range for analysis
symbols <- c("AAPL", "MSFT", "GOOGL")
start_date <- as.Date("2024-01-01")
end_date <- as.Date("2024-03-15")

# Retrieve closing prices for each stock and organize by columns
stock_data <- map_dfc(symbols, function(symbol) {
  daily_data <- getSymbols(symbol, src = "yahoo", from = start_date, to = end_date, auto.assign = FALSE)
  closing_prices <- data.frame(symbol = Cl(daily_data)) 
  names(closing_prices) <- paste(symbol) # Label column with the stock symbol
  return(closing_prices)
})

# Display the last 5 entries of the compiled stock data
print(tail(stock_data, 5))
```

    ##              AAPL   MSFT  GOOGL
    ## 2024-03-08 170.73 406.22 135.41
    ## 2024-03-11 172.75 404.52 137.67
    ## 2024-03-12 173.23 415.28 138.50
    ## 2024-03-13 171.13 415.10 139.79
    ## 2024-03-14 173.00 425.22 143.10

While `map_dfc` is ideal for column-wise compilation, `map_dfr`
aggregates data row-wise, making it suitable for creating summary
statistics tables or consolidating outcomes across different entities or
timeframes.

``` r
# Retrieve and summarize closing prices for stocks
stats_data <- map_dfr(symbols, function(symbol) {
  daily_data <- Cl(getSymbols(symbol, src = "yahoo", from = start_date, to = end_date, auto.assign = FALSE))
  summary_df <- data.frame(Ticker = symbol, Mean = mean(daily_data, na.rm = TRUE), SD = sd(daily_data, na.rm = TRUE))
  return(summary_df)
})

# Display the compiled summary statistics for each stock
print(stats_data)
```

    ##   Ticker     Mean        SD
    ## 1   AAPL 183.5600  6.787166
    ## 2   MSFT 401.3524 14.021370
    ## 3  GOOGL 141.9761  5.107439

### Conditional Mapping

In sentiment analysis, converting numeric scores into meaningful
categories is a common requirement. Here, we use conditional mapping to
categorize sentiment scores based on a threshold, demonstrating how
purrr can handle more complex, element-wise operations within lists.

``` r
# Sentiment scores for different regions
sentiment_scores <- list(USA = c(0.2, 0.34, 0.6, 0.08, 0.9), EUR = c(0.1, 0.3, 0.45, 0.77, 0.95))

# Map numeric scores to "Positive" or "Negative" categories
categorized_sentiment <- map(sentiment_scores, ~map_if(.x, ~.x >= 0.5, 
                                                       ~paste("Positive:", .x), 
                                                       .else = ~paste("Negative:", .x)))

# Display the categorized sentiment analysis results
print(categorized_sentiment)
```

    ## $USA
    ## $USA[[1]]
    ## [1] "Negative: 0.2"
    ## 
    ## $USA[[2]]
    ## [1] "Negative: 0.34"
    ## 
    ## $USA[[3]]
    ## [1] "Positive: 0.6"
    ## 
    ## $USA[[4]]
    ## [1] "Negative: 0.08"
    ## 
    ## $USA[[5]]
    ## [1] "Positive: 0.9"
    ## 
    ## 
    ## $EUR
    ## $EUR[[1]]
    ## [1] "Negative: 0.1"
    ## 
    ## $EUR[[2]]
    ## [1] "Negative: 0.3"
    ## 
    ## $EUR[[3]]
    ## [1] "Negative: 0.45"
    ## 
    ## $EUR[[4]]
    ## [1] "Positive: 0.77"
    ## 
    ## $EUR[[5]]
    ## [1] "Positive: 0.95"

### Task: Analyzing Loan Portfolio Risk

As part of the risk management team at a financial institution, you’re
tasked with assessing the default risk of a portfolio of loans. Each
loan in the portfolio has associated risk factors based on historical
data. Your goal is to categorize these loans into “High”, “Medium”, or
“Low” risk categories based on their risk scores and calculate the
proportion of loans in each category to present in the next team
meeting.

You have a list named `loan_risk_scores`, where each element represents
a loan identified by its ID (e.g., “Loan1”, “Loan2”, …), and each
element is a numerical value indicating the loan’s risk score. Risk
scores range from 0 (lowest risk) to 1 (highest risk).

1.  Categorize each loan based on its risk score:

- High Risk”: scores \> 0.7

- “Medium Risk”: scores between 0.3 and 0.7

- “Low Risk”: scores \< 0.3

2.  Calculate the proportion of loans in each risk category within the
    portfolio.
