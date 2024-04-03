R Basics - Session 1: Working with Objects and Understanding the
Environment
================
Cristhian Rodriguez Revilla

- [Understanding the R Environment](#understanding-the-r-environment)
  - [Global vs. Local Environments](#global-vs-local-environments)
    - [Local Environment](#local-environment)
  - [Libraries: Extending R’s
    Capabilities](#libraries-extending-rs-capabilities)
  - [Organise working flows](#organise-working-flows)
  - [Data Structures in R](#data-structures-in-r)
    - [Vectors](#vectors)
      - [Selecting elements:](#selecting-elements)
    - [TASK: Analyzing Investment
      Returns](#task-analyzing-investment-returns)
    - [Matrix](#matrix)
    - [Data Frames](#data-frames)
    - [Lists](#lists)
      - [Indexing list](#indexing-list)
  - [Importing Data](#importing-data)
    - [APIs](#apis)
      - [cryptocurrency Data](#cryptocurrency-data)
    - [Problem Set One](#problem-set-one)
      - [Portfolio Analysis](#portfolio-analysis)
      - [Predictive Model Evaluation for
        Cryptocurrency](#predictive-model-evaluation-for-cryptocurrency)
      - [Mutual Fund Database](#mutual-fund-database)

# Understanding the R Environment

## Global vs. Local Environments

R allows us to work within different scopes, known as environments. The
global environment is where your work resides over the long term,
whereas the local environment is temporary and only exists during your
current session.

### Local Environment

Think of the local environment as your workspace for the current
session. Anything you do here is temporary unless you explicitly save
your work. Let’s see a basic operation:

``` r
print(6 + 2) # This calculation is performed in the global environment
```

    ## [1] 8

However, when creating variables or “objects,” you typically work
locally. It’s important to note that local variables can overwrite
global ones, which might cause unexpected behavior.

``` r
Six = 6
two = 2
print(Six + two) # A simple addition using local objects
```

    ## [1] 8

Global variables in R, such as TRUE and FALSE, are universally
recognized:

``` r
print(T); print(TRUE) # These are globally recognized logical values
```

    ## [1] TRUE

    ## [1] TRUE

``` r
print(F); print(FALSE)
```

    ## [1] FALSE

    ## [1] FALSE

Functions, whether local or global, behave similarly. Thanks to R’s
global environment, you can use a function without explicitly defining
it in your script.

``` r
sqrt(4) # Calculates the square root, a global function
```

    ## [1] 2

``` r
log(exp(1)) # Logarithm and exponential functions
```

    ## [1] 1

## Libraries: Extending R’s Capabilities

Libraries, collections of functions and data sets, significantly extend
R’s functionality. By default, R includes a set of preinstalled
libraries, but you can install more as needed.

``` r
# install.packages("DescTools") # Uncomment to install DescTools
library("DescTools") # Loads functions for statistical analysis
library(lubridate) # Provides functions to work with dates
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

Using these libraries, you can perform complex tasks efficiently:

``` r
x = as.Date("2024-04-15") # Converts a string to a Date object
year(x); quarter(x); week(x); day(x) # Extracts components of the date
```

    ## [1] 2024

    ## [1] 2

    ## [1] 16

    ## [1] 15

## Organise working flows

It’s essential to keep your working directory organized:

``` r
setwd("C:/Introduction to R")
getwd()
```

By managing your environment and organizing your files, you ensure your
work is reproducible and clear.

## Data Structures in R

### Vectors

Vectors are the simplest data structure in R. They are one-dimensional
arrays that can hold numeric, character, or logical data.

``` r
d10 = c(10, 20, 30, 40, 50) # Numeric vector
dten = c("ten", "twenty", "thirty", "forty", "fifty") # Character vector
print(class(d10))
```

    ## [1] "numeric"

``` r
print(class(dten))
```

    ## [1] "character"

Remember, vectors in R are homogeneous; they can only contain data of
the same type.

``` r
dmix = c(10, "ten") # Mixed vector
print(dmix) # Notice how everything is coerced to the same type
```

    ## [1] "10"  "ten"

``` r
class(dmix)
```

    ## [1] "character"

You can also create sequences and select specific elements from vectors,
which is fundamental to working with data in R.

``` r
dseq = seq(2, 100, by = 2) # Creates a sequence of even numbers
print(dseq)
```

    ##  [1]   2   4   6   8  10  12  14  16  18  20  22  24  26  28  30  32  34  36  38
    ## [20]  40  42  44  46  48  50  52  54  56  58  60  62  64  66  68  70  72  74  76
    ## [39]  78  80  82  84  86  88  90  92  94  96  98 100

#### Selecting elements:

``` r
dseq[1:2];  head(dseq,2); 
```

    ## [1] 2 4

    ## [1] 2 4

``` r
dseq[49:50]; tail(dseq,2)
```

    ## [1]  98 100

    ## [1]  98 100

``` r
dseq[25]; dseq[length(dseq)*0.5]
```

    ## [1] 50

    ## [1] 50

### TASK: Analyzing Investment Returns

This task introduces you to R’s capabilities for handling financial data
through a straightforward analysis of investment returns. By the end of
this exercise, you’ll be able to calculate and analyze the returns of a
hypothetical investment portfolio.

Imagine you are a financial analyst assessing the performance of a
portfolio. The portfolio consists of investments in four different
assets. Over the past 10 months, each asset has generated monthly
returns as follows:

- Asset A: 8%
- Asset B: 5%
- Asset C: 12%
- Asset D: 7%

1.  **Create a Returns Vector**: Create a vector named
    **`monthlyReturns`** containing the monthly returns for each asset.
    Use decimal points for percentages (e.g., 8% becomes 0.08).

2.  **Calculate Portfolio Return**: Assume each asset has equal weight
    in the portfolio. Calculate the average monthly return of the
    portfolio and print the result. This will give you an idea of the
    overall performance.

    - Use Mean() from DescTools or mean() from base syntax.

3.  **Assessing Risk with a Simple Metric**: Calculate the standard
    deviation of the portfolio’s returns, which will serve as a
    rudimentary risk metric. In finance, the standard deviation is often
    considered a measure of the investment’s volatility.

    - Use SD() from DescTools or sd() from base syntax.

4.  **Forecast Future Value**: Create a sequence named
    **`futureMonths`** from 1 to 10, representing the next ten months.
    Assuming the portfolio continues to earn the average monthly return
    you calculated in Step 2, calculate the projected portfolio value
    for the next 10 months if the initial investment is \$1,000. Use the
    formula for compound interest for this calculation
    ![exp(r\times \tau)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;exp%28r%5Ctimes%20%5Ctau%29 "exp(r\times \tau)").

### Matrix

Matrices are two-dimensional data structures, which you can manipulate
in various ways. Let’s see how to create and work with matrices:

``` r
m=matrix(0, nrow=1, ncol=4);print(m) 
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    0    0    0    0

``` r
m1=matrix(NA, nrow=1, ncol=4);m2=matrix(NA,nrow=0, ncol=4);print(m1);print(m2)
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]   NA   NA   NA   NA

    ##      [,1] [,2] [,3] [,4]

``` r
m3=matrix(c("Hello","Beautiful","World"), ncol=3);print(m3) 
```

    ##      [,1]    [,2]        [,3]   
    ## [1,] "Hello" "Beautiful" "World"

You index by \[row, column\]

``` r
m[1, 2] # Accesses the element at first row, second column}
```

    ## [1] 0

``` r
m4 = matrix(seq(1, 16), nrow = 4) # Creates a 4x4 matrix (byrow=F Default setting) 

print(m4 + 4) # Adds 4 to every element in the matrix 
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    5    9   13   17
    ## [2,]    6   10   14   18
    ## [3,]    7   11   15   19
    ## [4,]    8   12   16   20

``` r
print(m4/4) # Divide each element by 4}
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,] 0.25 1.25 2.25 3.25
    ## [2,] 0.50 1.50 2.50 3.50
    ## [3,] 0.75 1.75 2.75 3.75
    ## [4,] 1.00 2.00 3.00 4.00

Vectors and matrices can interact, showcasing R’s powerful data
manipulation capabilities:

Other operations are possible

``` r
v1 = c(4, 6, 2, 8); print(v1 * m4) # Multiplies each column of m4 by the vector v1}
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    4   20   36   52
    ## [2,]   12   36   60   84
    ## [3,]    6   14   22   30
    ## [4,]   32   64   96  128

### Data Frames

Data frames in R are powerful tools for data analysis, allowing you to
work with data sets that have different types of data (numeric,
character, etc.) across columns. They are similar to matrices but
provide more flexibility, making them ideal for statistical modeling and
data manipulation.

``` r
# Creating vectors of different types
x1 <- c(2, 4, 6)  # Numeric vector
x2 <- c(1, 3, 5)  # Another numeric vector
x3 <- c("Indiv1", "Indiv2", "Indiv3")  # Character vector


# Using data frames to preserve data types
df1 <- data.frame(x1, x2, x3)
print(df1)
```

    ##   x1 x2     x3
    ## 1  2  1 Indiv1
    ## 2  4  3 Indiv2
    ## 3  6  5 Indiv3

Data frames allow you to keep the distinct characteristics of each data
type intact. Unlike matrices, where attempting to mix types leads to
type coercion, data frames maintain each column’s type, allowing for
operations on numeric columns while preserving character data.

``` r
print(rbind(x1, x2, x3)) # binding rows
```

    ##    [,1]     [,2]     [,3]    
    ## x1 "2"      "4"      "6"     
    ## x2 "1"      "3"      "5"     
    ## x3 "Indiv1" "Indiv2" "Indiv3"

``` r
print(cbind(x1, x2, x3)) # binding columns 
```

    ##      x1  x2  x3      
    ## [1,] "2" "1" "Indiv1"
    ## [2,] "4" "3" "Indiv2"
    ## [3,] "6" "5" "Indiv3"

Data frames provide flexible indexing options, making it easier to
manipulate and access data.

``` r
# Multiplying two columns in a data frame
print(df1[,"x1"] * df1[,"x2"])  # Using matrix-like indexing
```

    ## [1]  2 12 30

``` r
print(df1$x1 * df1$x2)  # Using $ for direct column access
```

    ## [1]  2 12 30

This flexibility in indexing, especially the `$`operator for direct
column access, is invaluable for data analysis, allowing you to easily
manipulate and analyze specific columns within a data frame.

### Lists

Lists are versatile data structures in R that allow you to store
elements of different types and sizes. They are essential for organizing
complex data sets and can contain vectors, matrices, data frames, and
even other lists.

``` r
# Creating a list containing diverse elements
L <- list(parN = dseq, matrix = m4, dataframe = df1)
print(L)
```

    ## $parN
    ##  [1]   2   4   6   8  10  12  14  16  18  20  22  24  26  28  30  32  34  36  38
    ## [20]  40  42  44  46  48  50  52  54  56  58  60  62  64  66  68  70  72  74  76
    ## [39]  78  80  82  84  86  88  90  92  94  96  98 100
    ## 
    ## $matrix
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16
    ## 
    ## $dataframe
    ##   x1 x2     x3
    ## 1  2  1 Indiv1
    ## 2  4  3 Indiv2
    ## 3  6  5 Indiv3

``` r
# A list can also contain other lists
L2 <- list(char = m3, Number = 8, list1 = L)
print(L2)
```

    ## $char
    ##      [,1]    [,2]        [,3]   
    ## [1,] "Hello" "Beautiful" "World"
    ## 
    ## $Number
    ## [1] 8
    ## 
    ## $list1
    ## $list1$parN
    ##  [1]   2   4   6   8  10  12  14  16  18  20  22  24  26  28  30  32  34  36  38
    ## [20]  40  42  44  46  48  50  52  54  56  58  60  62  64  66  68  70  72  74  76
    ## [39]  78  80  82  84  86  88  90  92  94  96  98 100
    ## 
    ## $list1$matrix
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16
    ## 
    ## $list1$dataframe
    ##   x1 x2     x3
    ## 1  2  1 Indiv1
    ## 2  4  3 Indiv2
    ## 3  6  5 Indiv3

List is a special environment that allows you to store and group all
type of data, with different structure and class. You can also have list
containg lists.

``` r
L2=list(char=m3,Number=8, list1=L)
print(L2)
```

    ## $char
    ##      [,1]    [,2]        [,3]   
    ## [1,] "Hello" "Beautiful" "World"
    ## 
    ## $Number
    ## [1] 8
    ## 
    ## $list1
    ## $list1$parN
    ##  [1]   2   4   6   8  10  12  14  16  18  20  22  24  26  28  30  32  34  36  38
    ## [20]  40  42  44  46  48  50  52  54  56  58  60  62  64  66  68  70  72  74  76
    ## [39]  78  80  82  84  86  88  90  92  94  96  98 100
    ## 
    ## $list1$matrix
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16
    ## 
    ## $list1$dataframe
    ##   x1 x2     x3
    ## 1  2  1 Indiv1
    ## 2  4  3 Indiv2
    ## 3  6  5 Indiv3

Lists can store a variety of data types, making them incredibly useful
for grouping related data together, especially when the data types
differ or when you need to organize data hierarchically.

#### Indexing list

Accessing elements within lists can be done in multiple ways.

``` r
# Accessing elements in a list
print(L2[[1]])  # Accesses the first element of L2
```

    ##      [,1]    [,2]        [,3]   
    ## [1,] "Hello" "Beautiful" "World"

``` r
# Accessing a specific element within a nested list
print(L2$list1$matrix)  # Accesses the 'matrix' element within the nested 'list1' of L2
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16

``` r
# Another example of accessing nested list elements
print(L2[[3]][[2]])  # Accesses the 'matrix' within 'list1', nested in L2
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16

## Importing Data

Data frames are the preferred structure for data analysis in R due to
their versatility in handling structured data. Here, we’ll cover how to
import data from common formats like CSV, Excel, and Stata. We’ll also
touch on importing data from Python and SQL, showcasing R’s flexibility.

``` r
# Importing data from CSV files
library(readr) # The readr package provides fast and friendly ways to read tabular data
dataset_csv <- read_csv("directory/file.csv") # Replace 'directory/file.csv' with the actual path to your CSV file

# Importing data from Excel files
library(readxl) # The readxl package simplifies reading Excel files
dataset_excel <- read_excel("directory/file.xlsx") # Replace 'directory/file.xlsx' with the path to your Excel file

# Importing data from Stata
library(haven) # The haven package allows you to import/export data between R and Stata, SPSS, and SAS
dataset_stata <- read_stata("directory/file.dta") # Replace 'directory/file.dta' with the path to your Stata file

# Interfacing with Python
library(reticulate) # The reticulate package provides a comprehensive set of tools for interoperability between Python and R

# Interacting with SQL databases
library(DBI) # The DBI package defines a common interface between the R and database management systems (DBMS)
library(RMySQL) # RMySQL provides a DBI-compliant interface to MySQL
# Example: conn <- dbConnect(RMySQL::MySQL(), dbname = "your_database_name", host = "host_name")
```

### APIs

For real-time or historical financial data analysis, APIs are
invaluable. Below are examples using the `quantmod` and `alphavantager`
packages to fetch stock market data and cryptocurrency information,
respectively.

``` r
# install.packages("quantmod")
library(quantmod)
```

    ## Warning: package 'quantmod' was built under R version 4.2.3

    ## Loading required package: xts

    ## Warning: package 'xts' was built under R version 4.2.3

    ## Loading required package: zoo

    ## Warning: package 'zoo' was built under R version 4.2.3

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## Loading required package: TTR

    ## Warning: package 'TTR' was built under R version 4.2.3

    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo

``` r
stock_symbol <- "AAPL" # Ticker for Apple
start_date <- as.Date("2020-01-01")  # Start date of the data
end_date <- as.Date("2020-12-31")    # End date of the data
getSymbols(stock_symbol, src = "yahoo", from = start_date, to = end_date)
```

    ## [1] "AAPL"

``` r
print(rbind(head(AAPL, 2), tail(AAPL,2))) 
```

    ##            AAPL.Open AAPL.High AAPL.Low AAPL.Close AAPL.Volume AAPL.Adjusted
    ## 2020-01-02   74.0600    75.150  73.7975    75.0875   135480400      73.05942
    ## 2020-01-03   74.2875    75.145  74.1250    74.3575   146322800      72.34913
    ## 2020-12-29  138.0500   138.790 134.3400   134.8700   121047300     132.36049
    ## 2020-12-30  135.5800   135.990 133.4000   133.7200    96452100     131.23192

#### cryptocurrency Data

``` r
# Fetching cryptocurrency data using Alpha Vantage
# Note: You'll need to install the alphavantager package and obtain an API key 
library(alphavantager) # The alphavantager package is an interface to the  API for financial data
av_api_key("YOUR_API_KEY") # obtain it from (https://www.alphavantage.co/)
data_btc <- av_get(symbol = "BTC", av_fun = "TIME_SERIES_DAILY") # time series data for Bitcoin (BTC)
print(tail(data_btc, 1)) # Display data point
```

### Problem Set One

#### Portfolio Analysis

Imagine you are a financial analyst at an investment firm. You’re tasked
with evaluating the performance of two investment portfolios to decide
which one is more promising based on their returns over the last six
months. The firm considers diversification across different asset
classes important, so each portfolio consists of stocks, bonds, and
cryptocurrencies.

1.  **Create Data Frames for Each Portfolio**:

    - Portfolio A: Create a data frame with the columns **`AssetClass`**
      (Stocks, Bonds, Crypto), **`AverageReturn`** (5%, 3%, 12%), and
      **`Volatility`** (10%, 5%, 20%).

    - Portfolio B: Create another data frame with the same columns but
      with the data **`AverageReturn`** (4%, 4%, 15%) and
      **`Volatility`** (9%, 4%, 25%).

2.  Create and print a list named **`PortfolioAnalysis`** containing
    both data frames. Name the elements **`PortfolioA`** and
    **`PortfolioB`**.

#### Predictive Model Evaluation for Cryptocurrency

A financial technology startup is developing predictive models for
cryptocurrency prices to offer insights to investors. They’ve created
two models and tested their predictions against the actual closing
prices of a popular cryptocurrency from Monday to Friday.

1.  Create a data frame with the following information:

    - Predictions Model 1: **`c(10, 8, 12, 13, 15)`**

    - Actual Prices: **`c(8, 10, 11, 10, 13)`**

    - Predictions Model 2: **`c(9, 7.5, 10.5, 9.5, 12.5)`**

2.  Calculate the Root Mean Squared Error (RMSE) for each prediction
    model: Use the RMSE function from `DescTools`\`

#### Mutual Fund Database

1.  Create and print a data frame named **`mutualFundsDB`** that
    includes the following columns: **`FundName`**, **`TotalAssets`**,
    **`AnnualReturns`**, **`ExpensesRate`**, **`FamilySize`**
    (Small/Big), **`Subadvised`** (Yes/No), and **`TeamManaged`**
    (Yes/No).

    - `Fund A`: Total Assets : 2000 million, Annual Returns: 5%,
      Expenses Rate: 0.02, Family Size: Big, Subadvised: No, Team
      Managed: Yes

    - `Fund B` : Total Assets: 150 million, Annual Returns: 6%, Expenses
      Rate: 0.025, Family Size: Small, Subadvised: Yes, Team Managed: No

    - `Fund C`: Total Assets: 500 million, Annual Returns: 4.5%,
      Expenses Rate: 0.015, Family Size: Big, Subadvised: No, Team
      Managed: Yes

    - `Fund D` : Total Assets: 1200 million, Annual Returns: 7%,
      Expenses Rate: 0.02, Family Size: Small, Subadvised: Yes, Team
      Managed: No
