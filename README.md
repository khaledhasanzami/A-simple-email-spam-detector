# A simple email spam detector

**Dataset:** We used a clean dataset of *kernlab package* named *spam* which is available [here](https://cran.r-project.org/web/packages/kernlab/index.html) 

**Files:**
There are 4 types of files in the repository:
- codebook.md epitomizes the whole code in simle text
- spam.R is the code used to build the email spam detector
- readme.md explains the repository
- Other .png files are used plots the codebook.md

**Code:**
Summary of the analysis is given below:
 - Lead with the question
   - Can I use quantitative characteristics of the emails to classify them as SPAM/HAM?
 - Describe the approach
   - Collected data from UCI -> created training/test sets
   - Explained relationships
   - Choose logistic model on training set by cross validation
   - Applied to test, 78% test set accuracy
   - Anything with more than 6.6% dollar signs is classified as Spam
   - Our test set error rate was 22.4%
- Interpret results
   - Number of dollar signs seems reasonable, e.g. "Make money with Viagra \$ \$ \$ \$!"
- Challenge results
   - 78% isn't that great
   - I could use more variables
  
