# General Question #
- [x] Can I automatically detect emails that are SPAM that are not?
## To be concrete ##
- [x] Can I use quantative characteristics of the emails to classify them as SPAM/HAM?

This is the question we are gonna answer with our analysis. So, basically we are gonna classify emails those are SPAM.

# Dataset #
- We are gonna use a cleaned dataset *spam* which is available with *kernlab package*.
- Install *kerblab* if not already installed.
  - ``` library(kernlab)```  :arrow_right: load the kernlab package 
  - `data(spam)` :arrow_right: load the spam dataset

# Building test and train sets #
- We are gonna split the dataset into train and test.
- Train will be used to build the model and test to examine how well the model make predictions.
- `set.seed(3435)` :arrow_right: setting this to recreat the rendom devision extacly same next time if needed.
- `trainIndicator = rbinom(4601, size = 1, prob = 0.5)` :arrow_right: dividing the dataset into 2 parts randomly.
```
> table(trainIndicator)
trainIndicator
   0    1 
2314 2287 
```
- So, one half is 2314 and other half is 2287.
- Using *rbinom()* to just like, flip a coin to separate it in two parts. See [rbinom()](https://www.rdocumentation.org/packages/stats/versions/3.3/topics/Binomial)
```
trainSpam = spam[trainIndicator == 1, ]
testSpam = spam[trainIndicator == 0, ]
```
- We allocated two sets of data to two different variables to be used in the process.
