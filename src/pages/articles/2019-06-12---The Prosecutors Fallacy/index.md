---
title: The Prosecutor's Fallacy
date: "2019-06-12T23:24:37.121Z"
layout: post
draft: false
path: "/posts/the-prosecutors-fallacy/"
category: "Data Science"
tags:
  - "probability"
  - "statistics"
  - "pandas"
  - "law"
  - "data science"
description: "in which Thomas Bayes and Wonder Woman teach you about the law of total probability and the law in general"
---

# The Prosecutor's Fallacy
## Conditional Probability in the Courtroom

![ww-cover.jpg](ww-cover.jpg)

*The lasso of truth*

Imagine you have been arrested for murder. You know that you are innocent, but physical evidence at the scene of the crime matches your description. The prosecutor argues that you are guilty because the odds of finding this evidence given that you are innocent are so small that the jury should discard the probability that you did not actually commit the crime. 

But those numbers don't add up. The prosecutor has misapplied *conditional probability* and neglected the prior odds of you, the defendant, being guilty before they introduced the evidence. 

## Conditional Probability

The [prosecutor's fallacy](https://www.cebm.net/2018/07/the-prosecutors-fallacy/) is a courtroom misapplication of [Bayes' Theorem](https://www.youtube.com/watch?v=LIQrs3dviIs). Rather than ask the probability that the defendant is *innocent* given all the *evidence*, the prosecution, judge, and jury make the mistake of asking what the probability is that the *evidence* would occur if the defendant were *innocent* (a much smaller number):

+ **What we want to know in the name of justice:**

P(defendant is guilty|all the evidence)

+ **What the prosecutor is usually actually demonstrating to the court:**

P(all the evidence|defendant is innocent)

### Bayes Theorem

To illustrate why this difference can spell life or death, imagine yourself the defendant again. You want to prove to the court that you're really telling the truth, so you agree to a polygraph test.

Coincidentally, the same man who invented the lie detector later created _Wonder Woman_ and her lasso of truth.

![lasso.jpg](lasso.jpg)

*What are the odds?*

William Moulton Marston debuted his invention in the case of [James Alphonso Frye](https://www.yalelawjournal.org/essay/on-evidence-proving-frye-as-a-matter-of-law-science-and-history), who was accused of murder in 1922.

![frye.gif](frye.gif)
*Frye being polygraphed by Marston*
 
For our simulation, we'll take the mean of a more modern polygraph from [this paper](https://www.tandfonline.com/doi/full/10.1080/23744006.2015.1060080) ("Accuracy estimates of the CQT range from 74% to 89% for guilty examinees, with 1% to 13% false-negatives, and 59% to 83% for innocent examinees, with a false-positive ratio varying from 10% to 23%...")

|          | *Lying (0.15)* | *Not Lying (0.85)* |
|----------|--------------|------------------|
| Test (+) | 0.81         | 0.17             |
| Test (-) | 0.08         | 0.71             |

Examine these percentages a moment. Given that [this study](http://wwwp.oakland.edu/Assets/upload/docs/News/2014/Serota-Levine-Prolific-Liars-2014.pdf) found that a vast majority of people are honest most of the time, and that "big lies" are things like "not telling your partner who you have really been with", let's generously assume that 15% of people would lie about murder under a polygraph test, and 85% would tell the truth.

If we tested 10,000 people with this lie detector under these assumptions... 


    1500 people out of 10000 are_lying 
    
    1215 people out of 10000 are_true_positives 
    
    120 people out of 10000 are_false_negatives 
    
    8500 people out of 10000 are_not_lying 
    
    1445 people out of 10000 are_false_positives 
    
    6035 people out of 10000 are_true_negatives 
    

![png](output_2_1.png)


The important distinctions to know before we apply Bayes' Theorem are these:

+ The _true positives_ are the people who lied and failed the polygraph (they were screened correctly)
+ The _false negatives_ are the people who lied and *beat* the polygraph (they were screened incorrectly)
+ The _false positives_ are the people who told the truth but failed the polygraph anyway
+ The _true negatives_ are the people who told the truth and passed the polygraph

Got it? Good.

#### Now: If you, defendant, got a *positive* lie detector test, what is the chance you were _actually_ lying?

What the polygraph examiner really wants to know is not P(+|L), which is the accuracy of the test; but rather P(L|+), or the probability you were lying given that the test was positive. We know how P(+|L) relates to P(L|+).

P(L|+) = P(+|L)P(L) / P(+) 

To figure out what P(+) is independent of our prior knowledge of whether or not someone was lying, we need to compute the total sample space of the event of testing positive using the [Law of Total Probability](https://www.statisticshowto.datasciencecentral.com/total-probability-rule/):

P(L|+) = P(+|L)P(L) / P(+|L)P(L) + P(+|L^c)P(L^c)

That is to say, we need to know not only the probability of testing positive given that you _are_ lying, but also the probability of testing positive given that you're _not_ lying (our false positive rate). The sum of those two terms gives us the total probability of testing positive. That allows us to finally determine the conditional probability that you are lying: 


```python
 def P_L_pos(pct_liars, pct_non_liars):
    P_pos_L = 0.81
    P_L = pct_liars
    P_pos_not_L = 0.17
    P_not_L = pct_non_liars
    
    P_pos = (P_pos_L*P_L)+(P_pos_not_L*P_not_L)
    
    P_L_pos = (P_pos_L*P_L)/(P_pos)
    
    print(f'The probability that you are actually lying, \
      given that you tested positive on the polygraph, is {round(P_L_pos*100, 2)}%.')
```


```python
P_L_pos(0.15, 0.85)
```

    The probability that you are actually lying, given 
    that you tested positive on the polygraph, is 45.68%.



```python
def false_pos(pct_liars, pct_non_liars):
    P_pos_L = 0.81
    P_L = pct_liars
    P_pos_not_L = 0.17
    P_not_L = pct_non_liars
    
    P_pos = (P_pos_L*P_L)+(P_pos_not_L*P_not_L)
    
    P_not_L_pos = (P_pos_not_L*P_not_L)/P_pos
    
    print(f'The probability of a false positive is {round(P_not_L_pos*100, 2)}%.')
```


```python
false_pos(0.15, 0.85)
```

    The probability of a false positive is 54.32%.


The probability that you're actually lying, given a positive test result, is only **45.68%**. That's _worse than chance_. Note how it differs from the test's _accuracy_ levels (81% true positives and 71% true negatives). Meanwhile, your risk of being falsely accused of lying, even if you're telling the truth, is also close to–indeed, slightly higher than–chance, at **54.32%**. Not reassuring.

![news.gif](news.gif)

*Marston was, in fact, a notorious fraud.*

The _Frye_ court ruled that the polygraph test could not be trusted as evidence. To this day, lie detector tests are inadmissible in court because of their unreliability. But that does not stop the prosecutor's fallacy from creeping in to court in other, more insidious ways.

# "Suspicious" Deaths

This statistical reasoning error runs rampant in the criminal justice system and corrupts criminal cases that rely on everything from fingerprints to DNA evidence to cell tower data. What's worse, courts often reject the expert testimony of statisticians because "it's not rocket science"–it's "common sense":

+ In the Netherlands, a nurse named [Lucia de Berk](https://arxiv.org/pdf/math/0607340.pdf) went to prison for life because she had been proximate to "suspicious" deaths that a statistical expert calculated had less than a 1 in 342 million chance of being random. The calculation, tainted by the prosecutor's fallacy, was incorrect. The true figure was more like 1 in 50 (or even 1 in 5). What's more, many of the "incidents" were only marked suspicious _after_ investigators knew that she had been close by.

+ A British nurse, [Ben Geen](https://www.thetimes.co.uk/article/nurse-was-victim-of-shipman-hysteria-3q9c6nrmxjp), was accused of inducing respiratory arrest for the "thrill" of reviving his patients, on the claim that respiratory arrest was too rare a phenomenon to occur by chance given that Green was near. 

+ Mothers in the U.K. have been prosecuted for murdering their children, when really they died of SIDS, after experts erroneously quoted the odds of two children in the same family dying of SIDS as 1 in [73 million](https://www.theguardian.com/science/2006/oct/28/uknews1)

The data in Ben Geen's case are available thanks to Freedom of Information requests -- so I have briefly analyzed them. 


```python
# Hospital data file from the expert in Ben Geen's exoneration case
# Data acquired through FOI requests
# Admissions: no. patients admitted to ED by month
# CardioED: no. patients admitted to CC from ED by month with cardio-respiratory arrest
# RespED: no. patients admitted to CC from ED by month with respiratory arrest 

import pandas as pd
hdf = pd.read_csv('Hdf.csv')
hdf.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Month</th>
      <th>CardioTot</th>
      <th>CardioED</th>
      <th>HypoTot</th>
      <th>HypoED</th>
      <th>RespTot</th>
      <th>RespED</th>
      <th>Admissions</th>
      <th>AllCases</th>
      <th>MonthNr</th>
      <th>Hospital</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1999</td>
      <td>Nov</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1189.0</td>
      <td>4.0</td>
      <td>-1</td>
      <td>Oxford Radcliffe</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1999</td>
      <td>Dec</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1378.0</td>
      <td>8.0</td>
      <td>0</td>
      <td>Oxford Radcliffe</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000</td>
      <td>Jan</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1185.0</td>
      <td>8.0</td>
      <td>1</td>
      <td>Oxford Radcliffe</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000</td>
      <td>Feb</td>
      <td>5.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1139.0</td>
      <td>6.0</td>
      <td>2</td>
      <td>Oxford Radcliffe</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000</td>
      <td>Mar</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1215.0</td>
      <td>7.0</td>
      <td>3</td>
      <td>Oxford Radcliffe</td>
    </tr>
  </tbody>
</table>
</div>



The most comparable hospitals to the one in which Geen worked are large hospitals that saw at least one case of respiratory arrest (although "0" in the data most likely means "missing data" and not that zero incidents occurred).


![png](output_12_0.png)



```python
ax = sns.boxplot(x='Year', y='CardioED', data=df)
```


![png](output_13_0.png)



```python
ax = sns.pairplot(df, x_vars=['Year'], y_vars=['CardioED', 'RespED', 'Admissions'])
```


![png](output_14_0.png)



The four hospitals that are comparable to the one where Geen worked are Hexham, Solihull, Wansbeck, and Wycombe. The data for Solihull (for both CardioED and RespED) are anomalous:


```python
import re
regex = re.compile(r'(Hexham|Solihull|Wansbeck|Wycombe)')
compdf = hdf[hdf['Hospital'].str.match(regex) == True]
```


```python
compdf.pivot_table(index='Hospital').reset_index()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Hospital</th>
      <th>Admissions</th>
      <th>AllCases</th>
      <th>CardioED</th>
      <th>CardioTot</th>
      <th>HypoED</th>
      <th>HypoTot</th>
      <th>MonthNr</th>
      <th>RespED</th>
      <th>RespTot</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Hexham</td>
      <td>382.520408</td>
      <td>2.724490</td>
      <td>0.887755</td>
      <td>0.887755</td>
      <td>1.673469</td>
      <td>1.673469</td>
      <td>71.5</td>
      <td>0.163265</td>
      <td>0.163265</td>
      <td>2005.410959</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Solihull</td>
      <td>557.306818</td>
      <td>0.301587</td>
      <td>0.000000</td>
      <td>0.261905</td>
      <td>0.000000</td>
      <td>0.039683</td>
      <td>71.5</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2005.410959</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wansbeck</td>
      <td>1706.285714</td>
      <td>16.969388</td>
      <td>5.010204</td>
      <td>5.010204</td>
      <td>10.918367</td>
      <td>10.918367</td>
      <td>71.5</td>
      <td>1.040816</td>
      <td>1.040816</td>
      <td>2005.410959</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Wycombe</td>
      <td>NaN</td>
      <td>0.980392</td>
      <td>0.568627</td>
      <td>0.627451</td>
      <td>0.039216</td>
      <td>0.058824</td>
      <td>71.5</td>
      <td>0.235294</td>
      <td>0.294118</td>
      <td>2005.410959</td>
    </tr>
  </tbody>
</table>
</div>



```python
import statsmodels.api as sm
from matplotlib import pyplot as plt

for hospital in compdf['Hospital'].unique():
    data = compdf[(compdf['Hospital'] == hospital)].CardioED.values.flatten()
    
    fig = sm.qqplot(data, line='s')
    h = plt.title(f'qqplot - CardioED {hospital}')
    plt.show()
```


![png](output_22_0.png)



![png](output_22_1.png)



![png](output_22_2.png)



![png](output_22_3.png)


After accounting for the discrepancies in the data, we can calculate that respiratory events without accompanying cardiac events happen, on average, roughly a little under 5 times as often as cardiac events (4.669 CardioED admissions on average per RespED admission).






<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>CardioTot</th>
      <th>CardioED</th>
      <th>HypoTot</th>
      <th>HypoED</th>
      <th>RespTot</th>
      <th>RespED</th>
      <th>Admissions</th>
      <th>AllCases</th>
      <th>MonthNr</th>
    </tr>
    <tr>
      <th>Year</th>
      <th>Month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="9" valign="top">2003</th>
      <th>Apr</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>739.0</td>
      <td>5.5</td>
      <td>40</td>
    </tr>
    <tr>
      <th>Aug</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>792.5</td>
      <td>4.0</td>
      <td>44</td>
    </tr>
    <tr>
      <th>Dec</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>1.5</td>
      <td>1.5</td>
      <td>865.5</td>
      <td>9.5</td>
      <td>48</td>
    </tr>
    <tr>
      <th>Jul</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>4.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>843.5</td>
      <td>6.0</td>
      <td>43</td>
    </tr>
    <tr>
      <th>Jun</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>5.5</td>
      <td>5.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>819.0</td>
      <td>8.0</td>
      <td>42</td>
    </tr>
    <tr>
      <th>May</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>2.5</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>793.0</td>
      <td>4.0</td>
      <td>41</td>
    </tr>
    <tr>
      <th>Nov</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>771.5</td>
      <td>5.0</td>
      <td>47</td>
    </tr>
    <tr>
      <th>Oct</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.5</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>813.5</td>
      <td>3.5</td>
      <td>46</td>
    </tr>
    <tr>
      <th>Sep</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>782.0</td>
      <td>2.0</td>
      <td>45</td>
    </tr>
    <tr>
      <th rowspan="12" valign="top">2004</th>
      <th>Apr</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.5</td>
      <td>1.5</td>
      <td>828.5</td>
      <td>9.5</td>
      <td>52</td>
    </tr>
    <tr>
      <th>Aug</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>2.5</td>
      <td>2.5</td>
      <td>837.5</td>
      <td>9.0</td>
      <td>56</td>
    </tr>
    <tr>
      <th>Dec</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>972.0</td>
      <td>6.5</td>
      <td>60</td>
    </tr>
    <tr>
      <th>Feb</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>757.0</td>
      <td>5.0</td>
      <td>50</td>
    </tr>
    <tr>
      <th>Jan</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>846.0</td>
      <td>4.5</td>
      <td>49</td>
    </tr>
    <tr>
      <th>Jul</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>864.0</td>
      <td>7.0</td>
      <td>55</td>
    </tr>
    <tr>
      <th>Jun</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>885.5</td>
      <td>9.0</td>
      <td>54</td>
    </tr>
    <tr>
      <th>Mar</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>855.5</td>
      <td>7.5</td>
      <td>51</td>
    </tr>
    <tr>
      <th>May</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>863.0</td>
      <td>7.5</td>
      <td>53</td>
    </tr>
    <tr>
      <th>Nov</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>2.5</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>823.5</td>
      <td>6.0</td>
      <td>59</td>
    </tr>
    <tr>
      <th>Oct</th>
      <td>5.0</td>
      <td>5.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>871.0</td>
      <td>6.0</td>
      <td>58</td>
    </tr>
    <tr>
      <th>Sep</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>853.5</td>
      <td>6.0</td>
      <td>57</td>
    </tr>
    <tr>
      <th rowspan="9" valign="top">2005</th>
      <th>Apr</th>
      <td>5.5</td>
      <td>5.5</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1015.5</td>
      <td>14.5</td>
      <td>64</td>
    </tr>
    <tr>
      <th>Aug</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>5.5</td>
      <td>5.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1109.0</td>
      <td>10.0</td>
      <td>68</td>
    </tr>
    <tr>
      <th>Dec</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1080.5</td>
      <td>14.0</td>
      <td>72</td>
    </tr>
    <tr>
      <th>Feb</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>893.0</td>
      <td>4.5</td>
      <td>62</td>
    </tr>
    <tr>
      <th>Jan</th>
      <td>2.5</td>
      <td>2.5</td>
      <td>3.5</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>972.5</td>
      <td>6.0</td>
      <td>61</td>
    </tr>
    <tr>
      <th>Jul</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1115.5</td>
      <td>8.5</td>
      <td>67</td>
    </tr>
    <tr>
      <th>Jun</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1133.0</td>
      <td>12.5</td>
      <td>66</td>
    </tr>
    <tr>
      <th>Mar</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>990.0</td>
      <td>5.5</td>
      <td>63</td>
    </tr>
    <tr>
      <th>May</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1039.5</td>
      <td>9.0</td>
      <td>65</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2008</th>
      <th>Sep</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>2.5</td>
      <td>2.5</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1107.0</td>
      <td>7.0</td>
      <td>105</td>
    </tr>
    <tr>
      <th rowspan="12" valign="top">2009</th>
      <th>Apr</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>7.5</td>
      <td>7.5</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1143.0</td>
      <td>10.0</td>
      <td>112</td>
    </tr>
    <tr>
      <th>Aug</th>
      <td>0.5</td>
      <td>0.5</td>
      <td>10.5</td>
      <td>10.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1083.5</td>
      <td>11.0</td>
      <td>116</td>
    </tr>
    <tr>
      <th>Dec</th>
      <td>5.0</td>
      <td>5.0</td>
      <td>6.5</td>
      <td>6.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1150.0</td>
      <td>12.0</td>
      <td>120</td>
    </tr>
    <tr>
      <th>Feb</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>8.5</td>
      <td>8.5</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1087.0</td>
      <td>11.5</td>
      <td>110</td>
    </tr>
    <tr>
      <th>Jan</th>
      <td>4.5</td>
      <td>4.5</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1103.0</td>
      <td>13.5</td>
      <td>109</td>
    </tr>
    <tr>
      <th>Jul</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>10.5</td>
      <td>10.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1153.0</td>
      <td>12.5</td>
      <td>115</td>
    </tr>
    <tr>
      <th>Jun</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1108.5</td>
      <td>12.0</td>
      <td>114</td>
    </tr>
    <tr>
      <th>Mar</th>
      <td>1.5</td>
      <td>1.5</td>
      <td>13.0</td>
      <td>13.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1175.5</td>
      <td>15.0</td>
      <td>111</td>
    </tr>
    <tr>
      <th>May</th>
      <td>2.5</td>
      <td>2.5</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1120.0</td>
      <td>10.5</td>
      <td>113</td>
    </tr>
    <tr>
      <th>Nov</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1067.5</td>
      <td>9.5</td>
      <td>119</td>
    </tr>
    <tr>
      <th>Oct</th>
      <td>6.0</td>
      <td>6.0</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>1.5</td>
      <td>1.5</td>
      <td>1189.5</td>
      <td>15.5</td>
      <td>118</td>
    </tr>
    <tr>
      <th>Sep</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>8.5</td>
      <td>8.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1177.5</td>
      <td>12.0</td>
      <td>117</td>
    </tr>
    <tr>
      <th rowspan="12" valign="top">2010</th>
      <th>Apr</th>
      <td>5.5</td>
      <td>5.5</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1158.5</td>
      <td>15.5</td>
      <td>124</td>
    </tr>
    <tr>
      <th>Aug</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>9.5</td>
      <td>9.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1212.0</td>
      <td>11.5</td>
      <td>128</td>
    </tr>
    <tr>
      <th>Dec</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>6.5</td>
      <td>6.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1251.5</td>
      <td>10.5</td>
      <td>132</td>
    </tr>
    <tr>
      <th>Feb</th>
      <td>2.5</td>
      <td>2.5</td>
      <td>8.5</td>
      <td>8.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1073.0</td>
      <td>11.5</td>
      <td>122</td>
    </tr>
    <tr>
      <th>Jan</th>
      <td>7.0</td>
      <td>7.0</td>
      <td>10.5</td>
      <td>10.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1074.0</td>
      <td>17.5</td>
      <td>121</td>
    </tr>
    <tr>
      <th>Jul</th>
      <td>5.0</td>
      <td>5.0</td>
      <td>9.5</td>
      <td>9.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1190.0</td>
      <td>14.5</td>
      <td>127</td>
    </tr>
    <tr>
      <th>Jun</th>
      <td>4.5</td>
      <td>4.5</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1213.0</td>
      <td>15.5</td>
      <td>126</td>
    </tr>
    <tr>
      <th>Mar</th>
      <td>2.5</td>
      <td>2.5</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1229.5</td>
      <td>11.5</td>
      <td>123</td>
    </tr>
    <tr>
      <th>May</th>
      <td>5.0</td>
      <td>5.0</td>
      <td>7.5</td>
      <td>7.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1203.5</td>
      <td>12.5</td>
      <td>125</td>
    </tr>
    <tr>
      <th>Nov</th>
      <td>6.5</td>
      <td>6.5</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1186.0</td>
      <td>14.0</td>
      <td>131</td>
    </tr>
    <tr>
      <th>Oct</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1267.5</td>
      <td>14.0</td>
      <td>130</td>
    </tr>
    <tr>
      <th>Sep</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>5.5</td>
      <td>5.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1226.5</td>
      <td>7.0</td>
      <td>129</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">2011</th>
      <th>Apr</th>
      <td>3.5</td>
      <td>3.5</td>
      <td>14.5</td>
      <td>14.5</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1151.0</td>
      <td>19.0</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Feb</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1136.0</td>
      <td>12.0</td>
      <td>134</td>
    </tr>
    <tr>
      <th>Jan</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>9.5</td>
      <td>9.5</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1224.0</td>
      <td>14.5</td>
      <td>133</td>
    </tr>
    <tr>
      <th>Mar</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>14.0</td>
      <td>14.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1246.5</td>
      <td>18.0</td>
      <td>135</td>
    </tr>
    <tr>
      <th>May</th>
      <td>7.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>12.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1189.5</td>
      <td>19.5</td>
      <td>137</td>
    </tr>
  </tbody>
</table>
</div>

![png](output_29_1.png)

The average number of respiratory arrests per month unaccompanied by cardiac failure is approximately 1-2, with large fluctuations. That's not particularly rare, and certainly not rare enough to send a nurse to prison for life. (You can read more about the case and this data [here](https://arxiv.org/abs/1407.2731) and see my jupyter notebook [here](https://nbviewer.jupyter.org/github/lorarjohns/prosecutors_fallacy/blob/master/Prosecutors_Fallacy.ipynb).)

Common sense, it would seem, is hardly common--a problem which the judicial system should take much more seriously than it does.
