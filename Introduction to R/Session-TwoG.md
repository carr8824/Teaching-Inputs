R Basics - Session 2: Conditionals, Functions, and Loops
================
Cristhian Rodriguez Revilla

- [Conditionals](#conditionals)
  - [Logical Outcomes](#logical-outcomes)
    - [Dealing with `NA` or `NaN`](#dealing-with-na-or-nan)
    - [Dealing with `Inf`](#dealing-with-inf)
    - [Dealing with `NULL`](#dealing-with-null)
  - [Creating Conditional Outcomes](#creating-conditional-outcomes)
    - [The Basics of If-Statements](#the-basics-of-if-statements)
    - [Expanding Horizons with Else](#expanding-horizons-with-else)
    - [Nesting and Combining
      Conditions](#nesting-and-combining-conditions)
    - [Crafting a Symphony of
      Conditions](#crafting-a-symphony-of-conditions)
  - [Task: Economic Indicator
    Analysis](#task-economic-indicator-analysis)
- [Functions f(x)](#functions-fx)
  - [Task: Calculating the Net Present Value (NPV) of a
    Project.](#task-calculating-the-net-present-value-npv-of-a-project)
- [Loops](#loops)
  - [Counters](#counters)
  - [Efficiency in Loops: The Apply
    Family](#efficiency-in-loops-the-apply-family)
    - [Using **sapply** for Vectorized
      Operations](#using-sapply-for-vectorized-operations)
    - [Analyzing Financial Projects with
      lapply](#analyzing-financial-projects-with-lapply)
  - [Task: Portfolio Investment
    Analysis](#task-portfolio-investment-analysis)

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

<img src="logo.png" alt="Watermark" class="img-watermark">

# Conditionals

Imagine you’re at a crossroads, and the path you choose depends on the
weather. If it’s sunny, you’ll take the path through the park; if it’s
raining, you’ll choose the path with shelter. This decision-making
process is akin to using conditionals in programming. In R, conditionals
allow our programs to respond differently based on certain conditions
being true or false.

Let’s explore some basic operators that help us define these conditions:

## Logical Outcomes

- `==` checks if two values are equal.
- `!=` determines if two values are different.
- `>` and `<` compare values to see if one is greater than or less than
  the other.
- `!` reverses the logical outcome, turning TRUE into FALSE, and vice
  versa.

``` r
3 > 2 # Logical (TRUE)
```

    ## [1] TRUE

``` r
res= -2 < 4 # Logical (TRUE)
class(res)
```

    ## [1] "logical"

``` r
res= -2 != 4 # Logical (TRUE)
print(res)
```

    ## [1] TRUE

``` r
res= !(-2 != 4) # Logical (TRUE) Reversed to FALSE 
print(res)
```

    ## [1] FALSE

In R, logical operators are used to evaluate conditions on a one-to-one
basis. This means each comparison or logical operation is conducted
element-wise across vectors, producing a corresponding logical value
(`TRUE` or `FALSE`) for each comparison.

### Dealing with `NA` or `NaN`

A special case to consider is when the data includes NA values. `NA`
represents a missing value in R—a placeholder for data that is not
available. Unlike an empty string or zero, which signify the presence of
an explicit value, NA indicates the absence of information. This
distinction is crucial in data analysis, as it affects how we interpret
and manage our datasets.

``` r
x <- c(NA, 1, 2)
is.na(x)  # Returns a logical vector indicating the presence of NA values
```

    ## [1]  TRUE FALSE FALSE

``` r
# Avoid x==NA (this is not possible)
```

### Dealing with `Inf`

Infinity in R, denoted as `Inf`, represents values that are beyond the
largest numbers the R environment can handle. It’s a unique value that
can emerge from certain mathematical operations, such as division by
zero or the logarithm of zero. Handling `Inf` values is essential
because they can significantly impact statistical calculations, such as
means and standard deviations, leading to potentially misleading
results.

``` r
y <- c(Inf, 3)
is.infinite(y)  # Checks for the presence of infinite values
```

    ## [1]  TRUE FALSE

``` r
# Avoid y==Inf
```

### Dealing with `NULL`

Another special value in R is NULL. It represents the absence of a
vector or a completely undefined value. While NA is used within vectors
to denote missing elements, NULL is used to signify that a variable
contains no data at all.

``` r
y <- NULL
is.null(y)  # Checks for the presence of infinite values
```

    ## [1] TRUE

``` r
# Avoid y==NULL
```

## Creating Conditional Outcomes

### The Basics of If-Statements

At the heart of conditional logic in R are the if statements. They act
as the gatekeepers, directing the flow of execution based on whether a
condition is met. Imagine standing at a fork in the road: if the path to
the right is clear, you take it; otherwise, you wait for further
instructions.

``` r
if(5 > 10) {  # Since 5 is not greater than 10, this condition evaluates to FALSE.
  print("Larger")  # This line will not execute because the condition is FALSE.
}
```

Here, we compare two numbers, 5 and 10. Since 5 is not greater than 10,
our condition (`5 > 10`) evaluates to FALSE, and the code inside the if
block does not execute.

### Expanding Horizons with Else

While an `if` statement can make a decision, coupling it with `else`
offers a pathway when the `if` condition fails. It’s akin to having a
backup plan: if Plan A doesn’t work, there’s always Plan B.

``` r
x <- seq(1, 100, 1)  # Creates a sequence of numbers from 1 to 100.
if(x[1] %% 2 == 0) {  # Checks if the first number in x is even.
  y <- "Even"
} else {
  y <- "Odd"
}
print(y)  # Prints "Odd" because 1 is not divisible by 2.
```

    ## [1] "Odd"

Notice how we directly address the issue of the `if` statement
considering only the first element of a vector. It’s crucial when
dealing with vectors to specify exactly which element we’re evaluating
because conditions only consider one logical value (default the first).

### Nesting and Combining Conditions

Real-world decisions are seldom black and white; often, they involve
multiple criteria. Similarly, in R, we can nest and combine conditions
to reflect complex decision trees. For instance, And (`&`) and Or (`|`)
allow us to combine conditions:

- `&` requires both conditions to be `TRUE`
- `|` requires at least one condition to be `TRUE`.

``` r
x <- 20
if(x > 0 & x %% 2 == 0) {
  y <- 1
} else {
  y <- 0
}
print(y)  # Prints 1 because x is positive and even.
```

    ## [1] 1

### Crafting a Symphony of Conditions

As we delve deeper, we realize that conditionals can be layered,
creating a rich tapestry of logic that precisely captures our
decision-making process.

``` r
x <- 4
# Experiment with different values of x to see how the outcome changes.
if(x > 0 & !is.na(x)) {
  y <- 1
} else if(x < 0 & x %% 4 == 0) {
  y <- -1
} else if(x %% 4 != 0) {
  y <- 0
} else {
  y <- NA
}
print(y)  # The outcome depends on the value and conditions applied to x.
```

    ## [1] 1

Here, we traverse a cascade of conditions, checking one after another,
until we find a match. It’s like navigating through a maze, where each
turn (condition) brings us closer to the exit (outcome). Remember to
define well you condition, excess of conditions are a waste of code:
conditionals are aligned in sequential order, breaking until it finds
the first TRUE, otherwise it will go to the else FALSE outcome.

## Task: Economic Indicator Analysis

In the ever-evolving landscape of global finance, the health of an
economy can be gauged by various indicators, such as GDP growth rate,
unemployment rate, and inflation rate. As future leaders in finance,
understanding and analyzing these indicators are crucial for making
informed decisions.

Your task is to utilize conditional logic in R to analyze the economic
health of a country based on its unemployment, inflation, and GDP rates.

1.  **Dataset Creation**:

    - Create a vector **`unemployment_rates`** that contains
      unemployment rates for a fictional country over the last quarter
      (3 months): Use 7 %, 5%, 10%.

    - Create a vector **`inflation_rates.`** using the observed rates
      over the last quarter (3 months): Use 8 %, 10%, 12%.

    - Create a vector **`gdp_rates.`** using the observed rates over the
      last quarter (3 months): Use 5 %, 3.5%, 1%.

2.  **Economic Health Analysis**: Create a condition to determine the
    state from the economy.

    - If the average unemployment rate is below 10%, the GDP is above
      5%, and the inflation is below 5%, the economy is considered
      **strong**.

    - If the average unemployment rate is either below 10%, the GDP
      above 5%, or the inflation below 5%, the economy is considered
      **stable**.

    - Any other events lead to **weak**

3.  Using the data from question 1, test the condition created in
    question 2.

# Functions f(x)

In the R programming language, functions act as fundamental building
blocks that allow for efficient data manipulation, calculation, and
analytical processing. Whether you’re dealing with pre-defined
mathematical functions or leveraging the extensive libraries available
in R, functions are designed to streamline your workflow and enhance
productivity.

R comes equipped with a wide array of predefined functions such as
`cos`, `sin`, `sqrt`, `exp`, and `log`. These functions enable you to
map input values (`x`) into desired outcomes (`f(x)`), facilitating
straightforward computations that are vital in financial analysis and
modeling.

While R’s predefined functions cover a broad spectrum of analytical
needs, there are scenarios in financial analysis where custom functions
are necessary. These scenarios might include tasks that require
automation or specific outcomes not readily available through existing
functions.

``` r
# Defining a custom polynomial function
Mypoly <- function(x) {
  fx = 4*x^3 - 3*x^2 + 2*x - 1  # Polynomial expression
  return(fx)  # Outputs the result of the polynomial calculation
}

# Using the custom function
Mypoly(4)  # Calculates the polynomial for x = 4
```

    ## [1] 215

Custom functions can also accept multiple inputs, allowing for more
complex analyses. In the following example, `fdesc` calculates the
discount factor using the rate (`r`) and time (`tau`) inputs.

``` r
# Defining a function with multiple inputs
fdesc <- function(r, tau) {
  fx = exp(-r * tau)  # Calculates the discount factor
  return(fx)  # Outputs the discount factor
}

# Using the function with multiple inputs
fdesc(r = 0.01, tau = seq(1, 10, 1))  # Calculates discount factors for tau = 1 to 10
```

    ##  [1] 0.9900498 0.9801987 0.9704455 0.9607894 0.9512294 0.9417645 0.9323938
    ##  [8] 0.9231163 0.9139312 0.9048374

In financial modeling, it’s often useful to return multiple outcomes
from a single function. The compound function demonstrates how to handle
conditional logic and return a list containing multiple elements,
showcasing the versatility of custom functions in handling various
financial calculations.

``` r
# Defining a function that returns multiple outcomes
compound <- function(r, tau, Rate) {
  if (Rate == "Discrete") {
    Simple = (1 + r * tau)
    Comp = (1 + r)^tau
  } else {
    Simple = "Warning: Continuous Scenarios NA"
    Comp = exp(r * tau)
  }
  
  Outcome = list("Simple" = Simple, "Compound" = Comp)
  return(Outcome)  # Outputs a list containing both simple and compound outcomes
}

# Using the function to calculate compound outcomes
compound(r = 0.01, tau = 10, Rate = "Discrete")
```

    ## $Simple
    ## [1] 1.1
    ## 
    ## $Compound
    ## [1] 1.104622

## Task: Calculating the Net Present Value (NPV) of a Project.

Net Present Value (**NPV**) is a core concept in finance, representing
the difference between the present value of cash inflows and outflows
over a period of time. It’s a critical measure for assessing the
profitability of a project or investment.

1.  Write a function in R that calculates the NPV of a series of cash
    flows at a given discount rate. Your function should be flexible
    enough to handle any number of cash flows (which may be uneven) and
    should return the NPV as its output.

- **`cashFlows`**: A numeric vector containing the series of cash flows
  (including the initial investment as the first negative cash flow).

- **`discountRate`**: The discount rate (as a decimal) to be applied to
  the cash flows.

![NPV=\sum\_{t} \frac{CashFlows\_{t}}{(1+r)^{t}}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;NPV%3D%5Csum_%7Bt%7D%20%5Cfrac%7BCashFlows_%7Bt%7D%7D%7B%281%2Br%29%5E%7Bt%7D%7D "NPV=\sum_{t} \frac{CashFlows_{t}}{(1+r)^{t}}")

# Loops

Loops are fundamental programming constructs that facilitate repetitive
tasks, making them invaluable in financial data analysis for tasks
ranging from simple data transformations to complex scenario
simulations. Understanding loops in R will empower you with the ability
to automate repetitive tasks efficiently.

## Counters

Counters allow you to execute a block of code multiple times, making
them especially useful for iterating over sequences or vectors.

Consider a scenario where you need to perform a task repeatedly, like
generating a report for five consecutive years. A for loop automates
this process:

``` r
year=seq(2020, 2024, 1)

for(i in 1:5){
  
# A simple for loop to print iteration numbers
print(paste("Report", year[i]))

  
}
```

    ## [1] "Report 2020"
    ## [1] "Report 2021"
    ## [1] "Report 2022"
    ## [1] "Report 2023"
    ## [1] "Report 2024"

In finance, you often work with sets of numbers, like interest rates or
investment returns. You can use a for loop to iterate over these sets:

``` r
rates=c(0.02, 0.01, 0.025, 0.03)

for(r  in rates){
  print(paste("Rate: ", r))
}
```

    ## [1] "Rate:  0.02"
    ## [1] "Rate:  0.01"
    ## [1] "Rate:  0.025"
    ## [1] "Rate:  0.03"

For more complex operations, such as examining the interactions between
two sets of financial variables, nested loops come into play:

``` r
for(i in year){
  for(j in rates){
    print(paste("Year:",i, "Rate:", j))
  }
}
```

    ## [1] "Year: 2020 Rate: 0.02"
    ## [1] "Year: 2020 Rate: 0.01"
    ## [1] "Year: 2020 Rate: 0.025"
    ## [1] "Year: 2020 Rate: 0.03"
    ## [1] "Year: 2021 Rate: 0.02"
    ## [1] "Year: 2021 Rate: 0.01"
    ## [1] "Year: 2021 Rate: 0.025"
    ## [1] "Year: 2021 Rate: 0.03"
    ## [1] "Year: 2022 Rate: 0.02"
    ## [1] "Year: 2022 Rate: 0.01"
    ## [1] "Year: 2022 Rate: 0.025"
    ## [1] "Year: 2022 Rate: 0.03"
    ## [1] "Year: 2023 Rate: 0.02"
    ## [1] "Year: 2023 Rate: 0.01"
    ## [1] "Year: 2023 Rate: 0.025"
    ## [1] "Year: 2023 Rate: 0.03"
    ## [1] "Year: 2024 Rate: 0.02"
    ## [1] "Year: 2024 Rate: 0.01"
    ## [1] "Year: 2024 Rate: 0.025"
    ## [1] "Year: 2024 Rate: 0.03"

## Efficiency in Loops: The Apply Family

While for loops are versatile, they’re not always the most efficient way
to handle data in R, especially with large datasets common in finance.
The apply family of functions offers a more efficient approach.

### Using **sapply** for Vectorized Operations

The **sapply** function applies a function over a vector or list and
simplifies the output. It’s particularly useful for applying financial
calculations across datasets:

``` r
# Using sapply to apply a function over a vector
sapply(c(0.02, 0.04, 0.065), fdesc, tau = 10)
```

    ## [1] 0.8187308 0.6703200 0.5220458

### Analyzing Financial Projects with lapply

The **lapply** function is ideal for applying a function over a list,
returning a list of results. It’s useful for evaluating multiple
financial projects:

``` r
# Defining a list of projects with projected cash flows
projects = list(
  "Project1" = c(-400, 40, 120, 220, 150),
  "Project2" = c(-600, 300, 250, 125, 60),
  "Project3" = c(-1000, 450, 350, 300, 100)
)

# Calculating NPV for each project
NPVs = lapply(projects, calculateNPV, discountRate = 0.05)
print(NPVs)
```

    ## $Project1
    ## [1] 60.38842
    ## 
    ## $Project2
    ## [1] 69.8135
    ## 
    ## $Project3
    ## [1] 87.45327

``` r
# Identifying the project with the highest NPV
bestProjectIndex = which.max(sapply(NPVs, unlist))
print(paste("The best project is Number", bestProjectIndex))
```

    ## [1] "The best project is Number 3"

## Task: Portfolio Investment Analysis

Imagine you are a financial analyst at a boutique investment firm. Your
portfolio manager has provided you with a list of annual returns for
five stocks over the past year. Your task is to identify the stock with
the highest sharpe ratio. This is a fundamental step in assessing the
performance of investments in the portfolio.

- **StockA** : 5%, 2% , -3% , 7%, 4%

- **StockB** : 3%, 6%, -2%, 4%, 5%

- **StockC** : -1%, -2%, 0%, 3%, 2%

- **StockD** : 7%, 9%, 5%, -2%, 1%

1.  Create a function to calculate the Sharpe ratio, having the return
    of a stock as an input.

2.  Initialize loop to iterate through each stock for an
    **`annualReturns`** list.

3.  Find the Stock with superior Sharpe.
