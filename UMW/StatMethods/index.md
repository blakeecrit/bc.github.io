---
title: Statistical Methods
description: STAT 280 - taught by Dr. Melody Denhere
---

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
1.	H<sup>o</sup>: There is no difference in the volume of INAH3 between heterosexual females (Group 5) and homosexual males (Group 3).
H<sup>a</sup>: There is a difference in the volume of INAH3 between heterosexual females and homosexual males.
2.	H<sup>o</sup>: There is no difference in the volume of INAH3 between heterosexual males (Group 1 & Group 2) and homosexual males (Group 3).
H<sup>a</sup>: There is a difference in the volume of INAH3 between heterosexual males and homosexual males. 
3.	H<sup>o</sup>: There is no difference in the volume of INAH3 between heterosexual males (Group 1 & Group 2) and heterosexual females (Group 5).
H<sup>a</sup>: There is a difference in the volume of INAH3 between heterosexual males and heterosexual females.

**Results:**\
To test these hypotheses, we need to conduct an ANOVA test with a post-hoc analysis of pairwise t-tests on the relationships of the groups.  By looking at these side-by-side boxplots of the groups (see Figure 1), we can tell that there is may be a difference in the volume between the five groups.  Group 2 has the highest median for the Volume (137.5 mm3).  We see that there are many outliers in this data.  When we removed these outliers, it caused other points in the data to become outliers.  Also, you can see that Group 4 has only one data point.
When conducting an ANOVA test for differences in the mean Volume between the groups, the p-value is 0.00794.  Therefore, at a 5% significance level, that there is a significant difference in the mean Volume of at least one group, compared to the others.  Since Group 4 only has one data point we removed it, so the Bonferroni pairwise t-test could be successfully conducted.  With the point removed, we re-ran our ANOVA test, and got an even lower p-value of 0.0056.  When the Bonferroni test was conducted, we saw that the relationship between Group 2 (Male, Heterosexual, Non-AIDS) and Group 3 (Male, Homosexual, AIDS) to have the lowest p-value of 0.0051.\
The conditions for inference for this ANOVA test were almost sufficiently met.  There was a problem in the similar spread condition, since all groups varied in spread.  In the boxplot of the residuals (see Figure 2), we see that there is an outlier with a slight left-skewed distribution.  The histogram and Q-Q plot shows a unimodal and almost symmetric distribution (see Figure 3 & 4).  We still notice that there is an outlier in the boxplot.  With a small sample size of 40 (Group 4 removed), we need the histograms to be unimodal and symmetric.\
To conduct the three hypothesis tests, we will use the same two-way ANOVA test, with Bonferroni pairwise t-test to find the p-value of the comparison of the groups.  The factors are the person’s Sex and Orientation.  From the ANOVA test, we got a p-value of 0.4926 for the Sex factor and a p-value of 0.0007 for the Orientation factor, and a p-value of 0.00253 for both factors combined.  Therefore, at a 5% significance level, we can conclude that a person’s orientation influences the level of INAH3, and there is an interaction in the Sex and Orientation on a person’s INAH3 level.  From the boxplots of the 2 factors (see Figure 5), that there are 3 outliers in the Male, Homosexual group.  With the removal of these points, it causes more points to become outliers, which decreases our sample size and flaws the data (even more than it is).\
For the first hypothesis test, the Bonferroni pairwise t-test helps us to compare the groups to see if there is a difference in the volume of INAH3 between heterosexual females and homosexual males.  The p-value for this comparison is 1.0000.  Which at a 5% significance level, we can conclude that there is no evidence in the volume between heterosexual females and homosexual males.
For the second hypothesis test, the Bonferroni pairwise t-test helps us to determine if there is a difference in the volume of INAH3 between heterosexual males and homosexual males.  The p-value for this data is 0.0021.  At a 5% significance level, we can conclude that there is significant evidence that there is a difference volume of INAH3 between heterosexual males and homosexual males.  That means that we reject the null hypothesis.\
To conduct the last hypothesis test, we will use the two-way ANOVA again test to help us determine if there is a difference in the volume of INAH3 between heterosexual males and heterosexual females.  The p-value for this test is 0.1675, at a 5% significance level, we can conclude that there is no evidence that there is a difference volume of INAH3 between heterosexual males and heterosexual females.  That means we fail to reject the null hypothesis.  The boxplot below shows the volume difference between each group for the data presented.\

**Conclusion:**
For our project we decided to find if there are difference in the volume of INAH3 between different groups of people based on their sex, orientation, and cause of death.  Out of the three-hypothesis test that we tried to find, we rejected only one hypothesis, that there is a difference in the volume of INAH3 between heterosexual males and homosexual males.  We used the data given to us and used R to test the differences between the data.  We agree with Simon LeVay on one of his hypotheses testing on the difference between heterosexual males and homosexual males.  Then we do not agree with Simon LeVay with the other two hypotheses testing.  The first hypothesis is, do heterosexual females tend to differ from homosexual males in the volume of INAH3.  Then the second hypothesis that we do not agree with is, do heterosexual males tend to differ from heterosexual females.

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
