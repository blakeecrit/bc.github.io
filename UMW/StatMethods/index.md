---
title: Statistical Methods
description: STAT 280 - taught by Dr. Melody Denhere
---

Project name: "A Biological Basis for Homosexuality"

Link to dataset: [INAH3](/INAH3.csv)

```r
data = read.csv('INAH3.csv')
attach(data)

# descriptive statistics
tapply(Volume, Sex, summary)
tapply(Volume, Sex, mean)

summary(Volume[Sex=="Male"])
summary(Volume[Sex=="Female"])
summary(Volume[Orientation=="Heterosexual"])
summary(Volume[Orientation=="Homosexual"])
summary(Volume[Death=="AIDS"])
summary(Volume[Death=="Non-AIDS"])
summary(Volume)

# difference in groups?
require(car)
Boxplot(Volume~Group, col='beige')
Grp = aov(Volume~Group)
summary(Grp)
pairwise.t.test(Volume, Group, p.adjust='bonf')

data2 = data[-36,] # removes the only point in group 4, so pairtests would work!
attach(data2);detach(data);remove(data)

# difference in groups, WITHOUT group 4
Grp = aov(Volume~Group)
summary(Grp)

#pairwise tests
pairwise.t.test(Volume, Group, p.adjust='bonf')

#graphical representation
resG = Grp$res
qqnorm(resG);qqline(resG, col='red')
hist(resG, col='beige')
Boxplot(resG, col='beige')

#two-way ANOVA to answer questions
fit = aov(Volume~Sex+Orientation+Sex:Orientation)
summary(fit)
Boxplot(Volume~Sex*Orientation, col='beige') # the removal of the outlier causes more outliers to form...
pairwise.t.test(Volume, Sex:Orientation, p.adjust='bonf') # use these p-values to answer the questions!

#graphical representation
res = fit$res
qqnorm(res);qqline(res, col='red')
hist(fit, col='beige')
Boxplot(fit, col='beige')
```
