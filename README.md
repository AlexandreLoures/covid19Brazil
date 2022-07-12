
<!-- README.md is generated from README.Rmd. Please edit that file -->

# covid19brazil

<!-- badges: start -->

[![build](https://github.com/AlexandreLoures/covid19brazil/actions/workflows/main.yml/badge.svg)](https://github.com/AlexandreLoures/covid19brazil/actions)
[![CRAN_Status_Badge](https://www.r-pkg.org/badges/version/covid19brazil)](https://cran.r-project.org/package=covid19brazil)
[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html)
[![License:
Mit](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Github
commit](https://img.shields.io/github/last-commit/AlexandreLoures/covid19brazil)](https://github.com/AlexandreLoures/covid19Brazil/commit/main)
[![](https://cranlogs.r-pkg.org/badges/grand-total/covid19brazil?color=blue)](https://cran.r-project.org/package=covid19brazil)
<!-- badges: end -->

The `covid19brazil` R package has daily information on the number of
accumulated cases and accumulated deaths for the COVID-19 pandemic in
Brazil. The information available in the package is organized as
follows:

-   `brazil_total` - Dataset with information about the new Coronavirus
    (COVID-19) for Brazil as a whole
-   `brazil_region` - Dataset with information on the new Coronavirus
    (COVID-19) for the five regions of Brazil
-   `brazil_state` - Information on the new Coronavirus (COVID-19) for
    the twenty-seven Federative Units of Brazil
-   `brazil_municipality` - Information on the new Coronavirus
    (COVID-19) for the 5,570 municipalities in Brazil

The graph below shows the number of cases accumulated in each Federative
Unit of Brazil per 100,000 inhabitants (data for the day 2022-06-24).

Data source: [Ministerio da Saude - Sistema Unico de Saude
(SUS)](https://www.gov.br/saude/pt-br)

<img src="figures/mapStates.png" width="100%" />

# Installation

To install the `package`, one of the two standard methods for installing
`packages`in R can be adopted. Directly through the
[cran](https://cran.r-project.org/package=covid19brazil) (choosing the
closest repository):

``` r
install.packages ("covid19brazil")
```

Or even using the command below. In the latter case, the latest version
of the `package` will be installed.

``` r
# install.packages ("devtools")
devtools::install_github ("AlexandreLoures/covid19brazil")
```

# The update_data command

To obtain the most up-to-date version of the data on the new coronavirus
(COVID-19) pandemic in Brazil, use the `update_data` command. Data is
updated daily on [Github (Dev)
version](https://github.com/AlexandreLoures/covid19brazil), however, the
[CRAN version](https://cran.r-project.org/package=covid19brazil) of the
`covid19brazil` package is updated every month or two. **Don’t forget to
restart the R session to load the new data**.

``` r
update_data ()
```

# Using the package

``` r
data ("brazil_total")

head (brazil_total)
#   region       date epidWeek population accumCases newCases accumDeaths
# 1 Brasil 2020-02-25        9  210147125          0        0           0
# 2 Brasil 2020-02-26        9  210147125          1        1           0
# 3 Brasil 2020-02-27        9  210147125          1        0           0
# 4 Brasil 2020-02-28        9  210147125          1        0           0
# 5 Brasil 2020-02-29        9  210147125          2        1           0
# 6 Brasil 2020-03-01       10  210147125          2        0           0
#   newDeaths newRecov followUp
# 1         0        0        0
# 2         0        1        0
# 3         0        1        0
# 4         0        0        1
# 5         0        1        1
# 6         0        1        1
```

# Download, read and analyze the microdata

``` r
library (ploty)

plot_ly(data = brazil_total,
        x = ~ date,
        y = ~home_confinement, 
        name = 'Home Confinement', 
        fillcolor = '#FDBBBC',
        type = 'scatter',
        mode = 'none', 
        stackgroup = 'one') %>%
  add_trace( y = ~ hospitalized_with_symptoms, 
             name = "Hospitalized with Symptoms",
             fillcolor = '#E41317') %>%
  add_trace(y = ~intensive_care, 
                name = 'Intensive Care', 
                fillcolor = '#9E0003') %>%
  layout(title = "Number of cases accumulated by region",
         legend = list(x = 0.8, y = 0.9),
         yaxis = list(title = "Number of Cases"),
         xaxis = list(title = "Source: Ministerio da Saude (Sistema Unico de Saude"))
```

<img src="figures/accumCases.png" width="100%" />

``` r
p <- plot_ly(data = brazil_total,
        x = ~ date,
        y = ~home_confinement, 
        name = 'Home Confinement', 
        fillcolor = '#FDBBBC',
        type = 'scatter',
        mode = 'none', 
        stackgroup = 'one') %>%
  add_trace( y = ~ hospitalized_with_symptoms, 
             name = "Hospitalized with Symptoms",
             fillcolor = '#E41317') %>%
  add_trace(y = ~intensive_care, 
                name = 'Intensive Care', 
                fillcolor = '#9E0003') %>%
  layout(title = "Number of deaths accumulated by region",
         legend = list(x = 0.8, y = 0.9),
         yaxis = list(title = "Number of Deaths"),
         xaxis = list(title = "Source: Ministerio da Saude (Sistema Unico de Saude"))
```

<img src="figures/accumDeaths.png" width="100%" />

# Moving averages and forecast

``` r
library (ploty)

plot_ly(data = brazil_total,
        x = ~ date,
        y = ~home_confinement, 
        name = 'Home Confinement', 
        fillcolor = '#FDBBBC',
        type = 'scatter',
        mode = 'none', 
        stackgroup = 'one') %>%
  add_trace( y = ~ hospitalized_with_symptoms, 
             name = "Hospitalized with Symptoms",
             fillcolor = '#E41317') %>%
  add_trace(y = ~intensive_care, 
                name = 'Intensive Care', 
                fillcolor = '#9E0003') %>%
  layout(title = "Number of cases accumulated by region",
         legend = list(x = 0.8, y = 0.9),
         yaxis = list(title = "Number of Cases"),
         xaxis = list(title = "Source: Ministerio da Saude (Sistema Unico de Saude"))
```

<img src="figures/rollmeanCases.png" width="100%" />

``` r
p <- plot_ly(data = brazil_total,
        x = ~ date,
        y = ~home_confinement, 
        name = 'Home Confinement', 
        fillcolor = '#FDBBBC',
        type = 'scatter',
        mode = 'none', 
        stackgroup = 'one') %>%
  add_trace( y = ~ hospitalized_with_symptoms, 
             name = "Hospitalized with Symptoms",
             fillcolor = '#E41317') %>%
  add_trace(y = ~intensive_care, 
                name = 'Intensive Care', 
                fillcolor = '#9E0003') %>%
  layout(title = "Number of deaths accumulated by region",
         legend = list(x = 0.8, y = 0.9),
         yaxis = list(title = "Number of Deaths"),
         xaxis = list(title = "Source: Ministerio da Saude (Sistema Unico de Saude"))
```

<img src="figures/rollmeanDeaths.png" width="100%" />
