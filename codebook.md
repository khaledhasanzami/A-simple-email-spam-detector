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
# Looking at the dataset #
- If we look at the column names, we will find out that they are just words.
```
> names(trainSpam)
 [1] "make"              "address"           "all"               "num3d"             "our"              
 [6] "over"              "remove"            "internet"          "order"             "mail"             
[11] "receive"           "will"              "people"            "report"            "addresses"        
[16] "free"              "business"          "email"             "you"               "credit"           
[21] "your"              "font"              "num000"            "money"             "hp"               
[26] "hpl"               "george"            "num650"            "lab"               "labs"             
[31] "telnet"            "num857"            "data"              "num415"            "num85"            
[36] "technology"        "num1999"           "parts"             "pm"                "direct"           
[41] "cs"                "meeting"           "original"          "project"           "re"               
[46] "edu"               "table"             "conference"        "charSemicolon"     "charRoundbracket" 
[51] "charSquarebracket" "charExclamation"   "charDollar"        "charHash"          "capitalAve"       
[56] "capitalLong"       "capitalTotal"      "type"  
```
- If we look at the first 5 rows, we will see that, they are just the frequency an word is in an email.
```
> head(trainSpam)
   make address  all num3d  our over remove internet order mail receive will people report addresses free
1  0.00    0.64 0.64     0 0.32 0.00   0.00        0  0.00 0.00    0.00 0.64   0.00      0         0 0.32
7  0.00    0.00 0.00     0 1.92 0.00   0.00        0  0.00 0.64    0.96 1.28   0.00      0         0 0.96
9  0.15    0.00 0.46     0 0.61 0.00   0.30        0  0.92 0.76    0.76 0.92   0.00      0         0 0.00
12 0.00    0.00 0.25     0 0.38 0.25   0.25        0  0.00 0.00    0.12 0.12   0.12      0         0 0.00
14 0.00    0.00 0.00     0 0.90 0.00   0.90        0  0.00 0.90    0.90 0.00   0.90      0         0 0.00
16 0.00    0.42 0.42     0 1.27 0.00   0.42        0  0.00 1.27    0.00 0.00   0.00      0         0 1.27
   business email  you credit your font num000 money hp hpl george num650 lab labs telnet num857 data
1         0  1.29 1.93   0.00 0.96    0      0  0.00  0   0      0      0   0    0      0      0 0.00
7         0  0.32 3.85   0.00 0.64    0      0  0.00  0   0      0      0   0    0      0      0 0.00
9         0  0.15 1.23   3.53 2.00    0      0  0.15  0   0      0      0   0    0      0      0 0.15
12        0  0.00 1.16   0.00 0.77    0      0  0.00  0   0      0      0   0    0      0      0 0.00
14        0  0.00 2.72   0.00 0.90    0      0  0.00  0   0      0      0   0    0      0      0 0.00
16        0  0.00 1.70   0.42 1.27    0      0  0.42  0   0      0      0   0    0      0      0 0.00
   num415 num85 technology num1999 parts pm direct cs meeting original project re edu table conference
1       0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
7       0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
9       0     0          0    0.00     0  0   0.00  0       0      0.3       0  0   0     0          0
12      0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
14      0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
16      0     0          0    1.27     0  0   0.42  0       0      0.0       0  0   0     0          0
   charSemicolon charRoundbracket charSquarebracket charExclamation charDollar charHash capitalAve
1          0.000            0.000                 0           0.778      0.000    0.000      3.756
7          0.000            0.054                 0           0.164      0.054    0.000      1.671
9          0.000            0.271                 0           0.181      0.203    0.022      9.744
12         0.022            0.044                 0           0.663      0.000    0.000      1.243
14         0.000            0.000                 0           0.000      0.000    0.000      2.083
16         0.000            0.063                 0           0.572      0.063    0.000      5.659
   capitalLong capitalTotal type
1           61          278 spam
7            4          112 spam
9          445         1257 spam
12          11          184 spam
14           7           25 spam
16          55          249 spam
```
- We can clearly see that the word make is absent from 1st two emails.
- Each cell has the numerical value representing the occurrence of a word (given by column) for a given e-mail (row).
  - each row :arrow_right: an email
  - each column :arrow_right: a word
  - each cell :arrow_right: occurance of a word in an email
-If we look at what type of emails we have in *trainSpam*, we can see 906 emails are Spam and 1381 are nonspam.
```
> table(trainSpam$type)

nonspam    spam 
   1381     906 
   ```
- Data is binomial in nature. It means we have only two types of data which are spam or nonspam.
- This is made numerical with:
```
> trainSpam$numType = as.numeric(trainSpam$type) - 1
> table(trainSpam$numType)

   0    1 
1381  906 
```
- If we convert binomial data into numeric, it will be 1 and 2. So, we subtract 1 to get it in binary : 0 and 1.
> 0 :arrow_right: nonSpam

> 1 :arrow_right: Spam 

# Curve Fitting #
- As the data is binomial, we fit a curve that predicts probability for an email being spam depending on the value.
- For example, let's look at the column *charDollar*.
```
png(filename="glm.png")
lmFormula=numType~charDollar
plot(lmFormula,data=trainSpam, ylab="probability")
g=glm(lmFormula,family=binomial, data=trainSpam)
curve(predict(g,data.frame(charDollar=x),type="resp"),add=TRUE)
dev.off()
```
![pic](https://github.com/khaledhasanzami/A-simple-email-spam-detector/blob/master/glm.png)
- Here, we can see that, *charDollar* values > 0.5: there is almost a 100% probability that, it is a spam. This is how binomial regression is used. 
- Now, if we look at the dimensions:
```
> dim(trainSpam)
[1] 2287   59
```
- There are 58 columns. Let's look at what they are: 
```
> head(trainSpam)
   make address  all num3d  our over remove internet order mail receive will people report addresses free
1  0.00    0.64 0.64     0 0.32 0.00   0.00        0  0.00 0.00    0.00 0.64   0.00      0         0 0.32
7  0.00    0.00 0.00     0 1.92 0.00   0.00        0  0.00 0.64    0.96 1.28   0.00      0         0 0.96
9  0.15    0.00 0.46     0 0.61 0.00   0.30        0  0.92 0.76    0.76 0.92   0.00      0         0 0.00
12 0.00    0.00 0.25     0 0.38 0.25   0.25        0  0.00 0.00    0.12 0.12   0.12      0         0 0.00
14 0.00    0.00 0.00     0 0.90 0.00   0.90        0  0.00 0.90    0.90 0.00   0.90      0         0 0.00
16 0.00    0.42 0.42     0 1.27 0.00   0.42        0  0.00 1.27    0.00 0.00   0.00      0         0 1.27
   business email  you credit your font num000 money hp hpl george num650 lab labs telnet num857 data
1         0  1.29 1.93   0.00 0.96    0      0  0.00  0   0      0      0   0    0      0      0 0.00
7         0  0.32 3.85   0.00 0.64    0      0  0.00  0   0      0      0   0    0      0      0 0.00
9         0  0.15 1.23   3.53 2.00    0      0  0.15  0   0      0      0   0    0      0      0 0.15
12        0  0.00 1.16   0.00 0.77    0      0  0.00  0   0      0      0   0    0      0      0 0.00
14        0  0.00 2.72   0.00 0.90    0      0  0.00  0   0      0      0   0    0      0      0 0.00
16        0  0.00 1.70   0.42 1.27    0      0  0.42  0   0      0      0   0    0      0      0 0.00
   num415 num85 technology num1999 parts pm direct cs meeting original project re edu table conference
1       0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
7       0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
9       0     0          0    0.00     0  0   0.00  0       0      0.3       0  0   0     0          0
12      0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
14      0     0          0    0.00     0  0   0.00  0       0      0.0       0  0   0     0          0
16      0     0          0    1.27     0  0   0.42  0       0      0.0       0  0   0     0          0
   charSemicolon charRoundbracket charSquarebracket charExclamation charDollar charHash capitalAve
1          0.000            0.000                 0           0.778      0.000    0.000      3.756
7          0.000            0.054                 0           0.164      0.054    0.000      1.671
9          0.000            0.271                 0           0.181      0.203    0.022      9.744
12         0.022            0.044                 0           0.663      0.000    0.000      1.243
14         0.000            0.000                 0           0.000      0.000    0.000      2.083
16         0.000            0.063                 0           0.572      0.063    0.000      5.659
   capitalLong capitalTotal type numType
1           61          278 spam       1
7            4          112 spam       1
9          445         1257 spam       1
12          11          184 spam       1
14           7           25 spam       1
16          55          249 spam       1
```
- There are 3 columns without a word and 55 columns with words and their occurance.
- So, we now have to look at every column conataining a word, which can be done by a for loop.
# cvError #
- We are creating a null vector of 55 length to be used in the loop.
```
cvError = rep(NA, 55)
```
- For more on *rep()*, look at [rep()](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/rep) and [boot](https://cran.r-project.org/web/packages/boot/boot.pdf) 
# Linear model #
- The line `lmformula = reformulate(...)` creates a linear model formula that changes with each iteration of the  `for()` loop, setting the dependent variable as numType.
- See [reformulate()](https://astrostatistics.psu.edu/su07/R/html/stats/html/delete.response.html) for more.
- The resulting formula will be 
> numtype ~ "column name"
# Error Estimation #
- We want to find out which of these 55 models is predicting the "BEST". For this we use Cross validation...
## cv.glm or CrossValidation ##
- The crossvalidation works as follows: It divides the trainData further into TRAIN and TEST. The TRAIN data is used to compute the glm, and this glm is used to predict the outcome of the TEST data. This is done [in a particular way](https://www.youtube.com/watch?v=fSytzGwwBVw) many times and the results are averaged.
- The CV uses a cost-function to calculate the error.
- The (in this case) counts number of failed predictions. The TEST data is used for this. It takes two parameters which is X
(observed TEST data) and Y (Predicted data based on glm) and checks how many times it failed in this case:
```
costFunction = function(x,y) sum(x!=(y > 0.5))
```
- y>0.5 provides a cutoff to decide if a value is spam or not. So if the predicted value is 0.6 then the prediction is SPAM (or 1). If the predicted value is <=0.5 then it is NOT SPAM (or 0).
- With the for loop we cycle over every single column and in the end pic the column which has the least error of predictions:
```
which.min(cvError)
```

