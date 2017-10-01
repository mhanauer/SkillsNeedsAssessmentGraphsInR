---
title: "Example of Report"
output:
  word_document: default
  pdf_document: default
  html_document: default
---
Here is an example of an artificial report for four treatment groups with two outcomes (outcome1, outcome2) measured on a scale ranging from 70 to 100.  I also analyzed how important factors to implementation of treatments such as fidelity (1 = 100% fidelity, 0 = less than 100% fidelity) and whether or not a document was turned in on time (onTimeDoc: 1 = turned in documents on time 0 = did not) over 4 time points (fall, winter, spring, and summer).

Additionally, I included a costs analysis to evaluate how income, costs, and revenue (income - costs) changed on a monthly basis for the implementation of the treatments.
```{r, echo=FALSE, message=FALSE, warning=FALSE}
library(Hmisc)
library(dplyr)
library(lme4)
library(nlme)
library(ggplot2)
library(scales)
library(quantmod)
library(psych)

selOverall = c(5, 4.6, 4.4, 3.9, 3.8, 3.8)
ccrOverall = c(5.39, 5.31, 5.28, 4.98, 4.99, 4.73)
asOverall = c(5.71, 5.62, 5.7, 5.05, 5.18, 5.1)
item = c(1:6)
needsOverall = as.data.frame(cbind(selOverall, ccrOverall, asOverall, item))
head(needsOverall)
```
Here create histograms for each of them
```{r, echo=FALSE, message=FALSE, warning=FALSE}
# SEL Overall
theme_set(theme_grey(base_size = 13))
p = ggplot(needsOverall, aes(x = item, y = selOverall));p
p = p+geom_bar(stat = "identity"); p
p = p + geom_text(aes(label=selOverall), vjust=1.6, color="white", size=3.5)+
  theme_minimal(); p
p = p+ggtitle("SEL Average Scores Across All Items");p
p = p + theme(plot.title = element_text(hjust = 0.5)); p
p = p+scale_y_continuous(name="SEL Average Score"); p
p = p+scale_x_continuous(name="Item Number"); p
p = p+ scale_x_continuous(breaks=seq(1,6, 1)); p
```
Create chnage over time for each of the three
```{r, message=FALSE, warning=FALSE, echo=FALSE}
datTimeAgg = round(aggregate(dat, list(dat$time), mean),2)
datTimeAgg$outcome1Change = Delt(datTimeAgg$outcome1)
datTimeAgg$outcome2Change = Delt(datTimeAgg$outcome2)
theme_set(theme_grey(base_size = 13))
p = ggplot(datTimeAgg, aes(x = time))
p = p+geom_line(aes(y = outcome1, color = "Outcome 1"))
p = p+geom_line(aes(y = outcome2, color = "Outcome 2"))
p = p+labs(y = "Outcomes 1 & 2")
p = p +expand_limits(y = c(70, 100))
p = p+ggtitle("Outcomes 1 and 2 Over Time");p
p = p+ scale_x_continuous(name="SEL Overall ")
```
Create fancy tables for of them.
```{r, message=FALSE, warning=FALSE, echo=FALSE}
theme_set(theme_grey(base_size = 13))
p = ggplot(datTimeAgg, aes(x = time))
p = p+geom_line(aes(y = outcome1Change, color = "% Change in Outcome 1"))
p = p+geom_line(aes(y = outcome2Change, color = "% Change in Outcome 2"))
p = p+labs(y = "% Change in Outcomes 1 & 2")
p = p +expand_limits(y = c(-.05, .05))
scale_x_continuous(limits = c(2, 4))
p = p+ scale_x_continuous(breaks=seq(2,4, 1))
p = p + scale_y_continuous(labels = percent)
p = p+ggtitle("% Change in Outcomes 1 and 2 Over Time");p
```

```{r, message=FALSE, warning=FALSE, echo=FALSE}
#Outcome 1 over time 
theme_set(theme_grey(base_size = 13))
p = ggplot(datTimeAgg, aes(x = time))
p = p+geom_line(aes(y = fidelity, group = treatment))
p = p + geom_line(aes(y = fidelity,shape=treatment, color=treatment))
p = p+labs(y = "% Fidelity")
p = p +expand_limits(y = c(.25, 1))
p = p + scale_y_continuous(labels = percent)
p = p+ggtitle("% Fidelity by Treatment over Time");p
```

