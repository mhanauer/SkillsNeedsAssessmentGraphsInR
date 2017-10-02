---
title: "Example of Report in R"
output:
  word_document: default
  pdf_document: default
  html_document: default
---
Here is an example of a recent needs assessment conducted for two local school districts.  In the graphs below I have bar charts showing the average scores across respondents to the six items displayed below on a 7 point Likert scale for the following categories social and emotional learning (SEL), college and career readiness (CCR), and academic success (AS).  These items were constructed from previous research and made to fit the context of the needs assessments with the support of the local school districts.  Finally, I show a table with the same information with red values indicating the lowest score on each item across the three categories.

```{r, echo=FALSE, message=FALSE, warning=FALSE}
selItems = c("My students have become better able to manage their emotions.", "Other staff in my school would say that my students have become better able to manage their emotions.", "I have noticed that other students at my school have become better able to manage their emotions.", "My school has enough resources to support students’ social and emotional learning.", "My school’s program(s) for social and emotional learning are adequate.", "I am satisfied with the amount of professional development training I have received to address my students’ social and emotional learning.")

ccrItems = c("My students have become better prepared for college and their future careers.", "Other staff in my school would say that my students have become better prepared for college and their future careers.", "I have noticed that other students at my school have become better prepared for college and their future careers.", "My school has enough resources to support students’ college and career readiness programs.", "My school’s program(s) for college and career readiness are adequate.", "am satisfied with the amount of professional development training I have received to address my students’ college and career readiness.")

asItems = c("My students have improved academically.", "Other staff in my school would say that my students have improved academically.", "I have noticed other students at my school improving academically.", "My school has enough resources to support students’ academic success.", "My school’s program(s) for academic success are adequate.", "I am satisfied with the amount of professional development training I have received to address my students’ academic success.")

itemNumbers = c(1:6)

tableItems = as.data.frame(cbind(itemNumbers, selItems, ccrItems, asItems)); formattable(tableItems)

library(Hmisc)
library(dplyr)
library(lme4)
library(nlme)
library(ggplot2)
library(scales)
library(quantmod)
library(psych)

selAverage = c(5, 4.6, 4.4, 3.9, 3.8, 3.8)
ccrOverall = c(5.39, 5.31, 5.28, 4.98, 4.99, 4.73)
asOverall = c(5.71, 5.62, 5.7, 5.05, 5.18, 5.1)
item = c(1:6)
needsOverall = as.data.frame(cbind(item, selOverall, ccrOverall, asOverall))
```
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
Table 1: Table format of SEL, College and Career Readiness, and Academic Success Average Scores Across each Item
```{r, message=FALSE, warning=FALSE, echo=FALSE}
library(formattable)
colnames(needsOverall) = c("Item","SEL_Average", "CCR_Average", "AS_Average")

formattable(needsOverall, list(SEL_Average = formatter(
    "span", style = x ~ style(color = ifelse(x == min(x) , "red", "green"))), CCR_Average = formatter("span", style = x ~ style(color = ifelse(x == min(x), "red", "green"))), AS_Average = formatter("span", style = x ~ style(color = ifelse(x == min(x), "red", "green")))))
```


