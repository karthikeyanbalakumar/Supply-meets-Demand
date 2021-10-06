# Supply-meets-Demand
Plotting the supply and demand curves in class using live data collected from students (using R)

---
title: "Equilibrium price"
author: "Karthikeyan Balakumar"
date: "02/10/2021"
output: html_document
---

Loading base packages
```{r}
library(dplyr)
library(googlesheets4)
library(tidyverse)
```
Getting data from the demand side
```{r}
demand_side <- read_sheet("https://docs.google.com/spreadsheets/d/1CmebdGatjC1s2_HeaCUrT3nX3gNJmelLJQLTUepCQkU/edit?resourcekey#gid=1064656721")
```

Creating the demand curve data frame
```{r}
demand_side_summary <- demand_side %>%
  summarise(
    demand_at_100 = mean(`How many bottles of water would you buy if a bottle costs Rs. 100/-`),
    demand_at_90 = mean(`How many bottles of water would you buy if a bottle costs Rs. 90/-`),
    demand_at_80 = mean(`How many bottles of water would you buy if a bottle costs Rs. 80/-`),
    demand_at_70 = mean(`How many bottles of water would you buy if a bottle costs Rs. 70/-`),
    demand_at_60 = mean(`How many bottles of water would you buy if a bottle costs Rs. 60/-`),
    demand_at_50 = mean(`How many bottles of water would you buy if a bottle costs Rs. 50/-`),
    demand_at_40 = mean(`How many bottles of water would you buy if a bottle costs Rs. 40/-`),
    demand_at_30 = mean(`How many bottles of water would you buy if a bottle costs Rs. 30/-`),
    demand_at_10 = mean(`How many bottles of water would you buy if a bottle costs Rs. 20/-`),
  )

a<- as.data.frame(colnames(demand_side_summary))

demand_side_summary<- t(demand_side_summary)

demand_side_summary<- data.frame(demand_side_summary, a)

library(readr)

demand_side_summary$price=parse_number(demand_side_summary$colnames.demand_side_summary.)
demand_side_summary$quantity_demanded = demand_side_summary$demand_side_summary

demand_side_summary<- subset(demand_side_summary, select = c(quantity_demanded, price))

```

Plotting the demand curve
```{r}
library(ggplot2)
ggplot(data= demand_side_summary, aes(x=quantity_demanded, y=price))+ geom_line(size = 1.0 ,color="steelblue") +    labs(title="The Demand Curve", x="Quantity Demanded", y="Price per bottle")

```

Getting data from the supply side
```{r}
supply_side <- read_sheet("https://docs.google.com/spreadsheets/d/1vwSd7X_nEVb-ZFcLICsAmfMCJFo5QG1ga87Urnsejbk/edit?resourcekey#gid=1004839069")
```

Creating the supply side data frame
```{r}
supply_side_summary<- supply_side %>%
  summarise(
    supply_at_100 = mean(`How many bottles of water are you willing to supply at Rs. 100/- per bottle...10`),
    supply_at_90 = mean(`How many bottles of water are you willing to supply at Rs. 90/- per bottle...9`),
    supply_at_80 = mean(`How many bottles of water are you willing to supply at Rs. 80/- per bottle...8`),
    supply_at_70 = mean(`How many bottles of water are you willing to supply at Rs. 70/- per bottle...7`),
    supply_at_60 = mean(`How many bottles of water are you willing to supply at Rs. 60/- per bottle...6`),
    supply_at_50 = mean(`How many bottles of water are you willing to supply at Rs. 50/- per bottle...5`),
    supply_at_40 = mean(`How many bottles of water are you willing to supply at Rs. 40/- per bottle...4`),
    supply_at_30 = mean(`How many bottles of water are you willing to supply at Rs. 30/- per bottle...3`),
    supply_at_20 = mean(`How many bottles of water are you willing to supply at Rs. 20/- per bottle`)
  )

b<- as.data.frame(colnames(supply_side_summary))

supply_side_summary<- t(supply_side_summary)

supply_side_summary<- data.frame(supply_side_summary, b)

library(readr)

supply_side_summary$price=parse_number(supply_side_summary$colnames.supply_side_summary.)
supply_side_summary$quantity_supplied = supply_side_summary$supply_side_summary

supply_side_summary<- subset(supply_side_summary, select = c(quantity_supplied, price))


```

Plotting the supply curve
```{r}
library(ggplot2)
ggplot(data= supply_side_summary, aes(x=quantity_supplied, y=price))+ geom_line(size = 1.0, color="darkred") +    labs(title="The Supply Curve", x="Quantity supplied", y="Price per bottle")

```

merging the two data sets
```{r}
equilibrium<- inner_join(supply_side_summary, demand_side_summary)
```
plotting the equilibrium price
```{r}
ggplot(equilibrium, aes(y=price)) + 
  geom_line(aes(x = quantity_supplied), size = 1.0, color = "darkred") + 
  geom_line(aes(x = quantity_demanded), size = 1.0 ,color="steelblue") +
  labs(title="When Supply Meets Demand", x="Quantity", y="Price")
```
