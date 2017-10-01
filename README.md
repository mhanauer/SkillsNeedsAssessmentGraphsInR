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
needsOverall = as.data.frame(cbind(item, selOverall, ccrOverall, asOverall))
head(needsOverall)
```
Here create histograms for each of them
```{r, echo=FALSE, message=FALSE, warning=FALSE}
theme_set(theme_grey(base_size = 13))
p = ggplot(needsOverall, aes(x = item, y = selOverall));p
p = p+geom_bar(stat = "identity"); p
p = p + geom_text(aes(label=selOverall), vjust=1.6, color="white", size=3.5)+
  theme_minimal(); p
p = p+ggtitle("SEL Average Scores Across All Items");p
p = p + theme(plot.title = element_text(hjust = 0.5)); p
p = p+scale_y_continuous(name="SEL Average Score"); p
p = p+scale_x_continuous(name="Item Number"); p
p = p + scale_x_continuous(breaks=seq(1,6, 1)); p
p = p+ coord_cartesian(ylim = c(1, 7)); p
p = p + scale_y_continuous(breaks=seq(1,7, 1)); p
p = p+ labs(x = "Item Number"); p
p = p+ labs(y = "SEL Average Score"); p

############CCR###########################
theme_set(theme_grey(base_size = 13))
p = ggplot(needsOverall, aes(x = item, y = ccrOverall));p
p = p+geom_bar(stat = "identity"); p
p = p + geom_text(aes(label=ccrOverall), vjust=1.6, color="white", size=3.5)+
  theme_minimal(); p
p = p+ggtitle("College and Career Readiness Average Scores Across All Items");p
p = p + theme(plot.title = element_text(hjust = 0.5)); p
p = p+scale_y_continuous(name="CCR Average Score"); p
p = p+scale_x_continuous(name="Item Number"); p
p = p + scale_x_continuous(breaks=seq(1,6, 1)); p
p = p+ coord_cartesian(ylim = c(1, 7)); p
p = p + scale_y_continuous(breaks=seq(1,7, 1)); p
p = p+ labs(x = "Item Number"); p
p = p+ labs(y = "College and Career Readiness Average Score"); p

############AS###########################
theme_set(theme_grey(base_size = 13))
p = ggplot(needsOverall, aes(x = item, y = asOverall));p
p = p+geom_bar(stat = "identity"); p
p = p + geom_text(aes(label=asOverall), vjust=1.6, color="white", size=3.5)+
  theme_minimal(); p
p = p+ggtitle("Academic Success Average Scores Across All Items");p
p = p + theme(plot.title = element_text(hjust = 0.5)); p
p = p+scale_y_continuous(name="Academic Success Average Score"); p
p = p+scale_x_continuous(name="Item Number"); p
p = p + scale_x_continuous(breaks=seq(1,6, 1)); p
p = p+ coord_cartesian(ylim = c(1, 7)); p
p = p + scale_y_continuous(breaks=seq(1,7, 1)); p
p = p+ labs(x = "Item Number"); p
p = p+ labs(y = "Academic Success Average Score"); p

```
Create fancy tables for of them.
```{r, message=FALSE, warning=FALSE, echo=FALSE}
library(formattable)
formattable(needsOverall)
formattable(needsOverall, list(selOverall = formatter("span",
   style = function(x) ifelse(x > median(x), "color:red", NA))))


```


