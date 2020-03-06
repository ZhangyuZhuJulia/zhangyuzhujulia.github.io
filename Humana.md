## Humana-Mays 2019 Healthcare Analytics Case Competition 

### Case Description:
Analyze 7 million medical records of 20,000 patients provided by Humana to create a model aim at predict if members will continue opioid therapy six months after initial prescribing. Provide insights and suggestions to help identify members at risk for continued long term use of opioid therapies allowing for early intervention

### Background Information:
Throughout the early 2000s, LTOT for non cancer pain conditions (NCPC) increased considerably without a corresponding increase in incidence of NCPC.<br>
As many as 1 in 4 patients receiving long term opioid therapy in a primary care setting will struggle with opioid disorder <br>
Evidence suggests an elevated risk for overdose, abuse, misuse, and negative health outcomes related to increased dosing or longer duration therapy, including fractures, Myocardial Infarction, and sexual dysfunction.

### Definition:
1. Opioid Naïve <br>
Defined as not having an opioid ‘on hand’ in the preceding 90 day period, based on service date and pay day supply count
2. Long Term Opioid Therapy (LTOT) <br>
Defined as continuous use of an opioid medication with 90% of days covered over a 6 month period

### Procedures:
1. Data prepartion<br>
* **Getting insights from large and un-organized dataset**<br>
Because this case is deeply involved with large amount of medical and healthcare knowledges and backgraounds, what we did first is understand the data deeply and find relationship between variables.<br>
Based on the goal to predict if members will continue opioid therapy six months after initial prescribing, we think backward about possible causation leads to this result.<br>
After understand the logic and variables provided in the dataset, we combined variables to generate new information that will help build the prediction model and clean the data to prepare for building the model <br>
<br> Create subset for each ID
```
for(i in unique(full_data$id)){
  thisID = subset(full_data,id==i)
```
* **some of the new variables we created**
* new day 0 date
<br> reason to create this variable: each patient may have multiple day 0, which represent the begining of a new 6 months period. If a patient did not take any pill for past 90 days, and the next day that he/she starts taking pill is the new day 0. We need identify all qualified day 0 for each patient in order to count have many days this patient has been taking pills within a 6 months period.
<br> code:
```
if(length(thisID$row_num)>=2){
  for (j in 2:max(thisID$row_num)){
  thisID$new_day_0_date[1] <- 0
  ifelse(thisID$df[thisID$row_num == j] >= 90,
         thisID$new_day_0_date[thisID$row_num == j] <- thisID$new_day[thisID$row_num == j],
         thisID$new_day_0_date[thisID$row_num == j] <- NA)
     }
else{
    thisID$new_day_0_date[1] <- 0
    }
full_data$new_day_0_date[full_data$id==i] = thisID$new_day_0_date 

```


