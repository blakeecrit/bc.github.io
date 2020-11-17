---
title: Statistical Methods
description: STAT 280 - taught by Dr. Melody Denhere
---
[Back to folder](/UMW/index.md)

Project name: "A Biological Basis for Homosexuality"

Link to dataset: [INAH3](/INAH3.csv) \
Link to R file: [BBH.r](/Final.r)

**Introduction:**\
For our project, we decided to find if there is physiological basis for sexual preference, that Simon LeVay did for his research.  Simon LeVay measured the volumes of four cell groups in the interstitial nuclei of the anterior hypothalamus in postmortem tissue from 41 subjects.  He collected the data from seven different metropolitan hospitals in New York and California.  The subjects in this experiment were based on three characteristics: Gender (Male, Female), Orientation (Heterosexual, Homosexual), & Death (AIDS-related, Non-AIDS related).  Based on these characteristics, there are 8 possible groups.  In the sample collected, there were only 5 groups, which are:
1.	Male, Heterosexual, AIDS
2.	Male, Heterosexual, Non-AIDS
3.	Male, Homosexual, AIDS
4.	Female, Heterosexual, AIDS
5.	Female, Heterosexual, Non-AIDS

**Hypothesis:**
The questions that we needed to find statistical evidence for is, “if heterosexual females tend to differ from homosexual males in the volume of INAH3”, “if heterosexual males tend to differ from homosexual males”, and “if heterosexual males tend to differ from heterosexual females.”  The hypotheses needed to answer these questions are:
1.	H<sub>o</sub>: There is no difference in the volume of INAH3 between heterosexual females (Group 5) and homosexual males (Group 3).\
H<sub>a</sub>: There is a difference in the volume of INAH3 between heterosexual females and homosexual males.
2.	H<sub>o</sub>: There is no difference in the volume of INAH3 between heterosexual males (Group 1 & Group 2) and homosexual males (Group 3).\
H<sub>a</sub>: There is a difference in the volume of INAH3 between heterosexual males and homosexual males. 
3.	H<sub>o</sub>: There is no difference in the volume of INAH3 between heterosexual males (Group 1 & Group 2) and heterosexual females (Group 5).\
H<sub>a</sub>: There is a difference in the volume of INAH3 between heterosexual males and heterosexual females.

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
