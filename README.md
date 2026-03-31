# Hospital-Readmission-Study-for-Diabetes-Patients

Everyone in our group had Diabetes stricken families. That was our driving force to do something related to this topic.




**Objectives**




1. To suggest optimal models to predict Diabetes Patients Readmission
2. To uncover the underlying reasons behind those readmissions



After sorting our objectives, we explored the dataset. The dataset had the records of over 100,000 patients for 50 features/variables. We had 13 integer variables and 37 variables of type ‘object’
There was some missing data that are represented with ‘?’ which was dealt with in the feature engineering section.
We removed the rows which were related to death/ hospice.
Then we created a new variable which showed the prevalence of readmission of diabetic patients within 30 days which was found to be 11%.
We realized this was an imbalanced classification problem, which was dealt with.
Most of the independent variables were categorical.
There was a mix of both categorical and numerical data.
We removed the identifiers which weren’t useful- (encounter id and patient nbr)
Age and Weight were categorical which was dealt with later in the extra features section.
Although Admission type discharge disposition and admission source are numerical,  they represent IDs and hence are considered categorical.
Drugs- Examide and citoglipton have only 1 value and will not be useful in the modelling so we removed them.
Diag1,2 and 3 are basically number of diagnosis and have a lot of values which are basically ICD codes and need a lot of work to address it in the correct way, hence we removed these but instead used the number of diagnoses to capture some of its information.
Medical speciality had a lot of unknown/missing values which we addressed in feature engineering. 




**Feature Engineering**




Here, we worked on the features. We will break down this section into numerical features, categorical features, and extra features.

NUMERICAL FEATURES:
Here, we created a dataframe of numerical variables which we were to use in our predictive models.
We checked if there were any missing values and there weren’t any.

CATEGORICAL FEATURES:
Here we dealt with the non-numeric features.
We made a dataframe with the categorical variables which we were to use in our models.
We checked the missing data here and found out race, medical speciality and payer code had a few missing values.
Medical speciality had a lot of unknown/missing values which we grouped as a new category and named it ‘UNK’ for unknown.
We did the same for race and Payer code.
Then we took the top 10 medical speciality as a new variable as there were a lot of specialities with very few values like 1, so we clubbed them under ‘others’. So we considered top 10 including the UNK, i.e. unknown since it had a very high frequency.
To convert our categorical features to numbers, we used one-hot encoding.
We created a new variable which included the IDs mapping which were admission type, discharge disposition and admission source.
We converted these in categorical and then we merged all the new variables that we made in the categorical section as a new dataframe.

EXTRA FEATURES
Age and weight were categorical variables, so we converted them in numerical. We created age as an ordinal feature while for weight since there were many missing values, we simply created a variable to say if weight was filled or not. We added these two columns in a dataframe as well.

Total number of features: 143
Numerical features: 8
Categorical Features: 133
Extra features: 2
MISSING VALUES: checked for missing values in all of them again.

We made a dataframe with all the new variables we created (for numerical, categorical and extra features) in the predictive modelling.
Now let’s make it clear that the new variable which we created that was readmitted within 30 days is the dependent variable and others are the factors we are going to understand the relationship with our dependent variable.




**Data Visualisation**




Inferences:
We saw more than 10k people had been readmitted giving 9:1 ratio, i.e. for every 9 diabetic patients getting admitted, 1 gets readmitted.
We inferred that Diabetic Patients in the age group 70-80 are more prone to get readmitted.
Diabetic Patients in the ethnic group Caucasian are more prone to get readmitted, but it can also be a case where in the data for Caucasian race is higher than other races. It might also be a case wherein the hospital from which the data is provided could be located in a Caucasian majority area.
Usually the patients that get readmitted have shown to be prescribed with more medications.
Readmission rate for men and women is almost similar although if we compare just the readmission count between Men and Women, women are more prone to get readmitted.
Since the readmission rate for both type of patients (patients with change in medications and no change) is similar, we can say that change of medication doesn’t affect the readmission rate.
Patients who were prescribed proper diabetes medication ironically were more prone to get readmitted.
We can see that more the number of inpatients, outpatients and emergency in preceding year
Patients that have not been tested are more likely to get readmitted.
High correlation can be seen in the Number of Lab Procedure of the patient and their readmission status.
More the number of lab procedures make the patients more prone to get readmitted.




**Segregating the dataset into Training, Validation and test sets**




We split the data after sampling and shuffling it into three parts:
*Training samples*: these samples are used to train the model. We used 70% of the dataset for Training the samples.
*Validation samples*: these samples are held out from the training data and are used to make decisions on how to improve the model. We used 15% of the dataset for validation of the samples.
*Test samples*: these samples are held out from all decisions and are used to measure the generalized performance of the model. We used the rest 15% of the dataset for Testing the samples.

After splitting we checked what percent of the groups were hospitalized within 30 days i.e. The prevalence for each sample. Ideally all three should have similar prevalence. We found that

Test prevalence (n = 14902):0.117
Valid prevalence (n = 14901):0.113
Train all prevalence (n = 69540):0.113

The prevalence is about the same for each group.

IMBALANCED DATA
What we also realized is that the data was imbalanced. There are 3 ways to deal with imbalanced data to give the positives more weight: 
1: sub-sample the more dominant class: use a random subset of the negatives
2: over-sample the imbalanced class: use the same positive samples multiple times
3: create synthetic positive data

Usually, we use the latter two methods but only if there are a handful of positive cases. Since we had a few thousand positive cases, we decided to go with the sub-sample approach. Here, we created a balanced training data set that had 50% positive and 50% negative.

STANDARD SCALING
Since we have features with different scales such as age and weight, we scaled it down for KNN and Logistic Regression. Decision Tree and Random Forests being advanced models don’t require scaled down data, so we used the prior version of the samples for them.




**Model Selection**




In this section, we trained 4 supervised machine learning models. The models we used are:

Logistic Regression, KNN, Decision Tree and Random Forest. 

We selected the best model based on performance on the validation set.

Logistic regression ---
We used Binary Regression since our dependent variable i.e readmitted within 30 days had two outcomes, Yes (depicted by 1) or not readmitted within 30 days(depicted by 0)
ERROR: 0.59

K Nearest-Neighbours(KNN) ---
ERROR: 0.57

Decision Tree ---
ERROR: 0.54

Random Forest ---




**Analyse results for the baseline models**




We made a dataframe with the results and plotted the outcomes using a package called seaborn. We utilized the Area under the ROC curve (AUC) to evaluate the best model.
For logistic regression, the variables with highest positive coefficients are predictive of re-hospitalization.
Most of the important variables for random forest were continuous variables. This makes sense since you can split continuous variables more times than categorical variables.

FEATURE IMPORTANCE: SUMMARY

In both models the most important feature is number_inpatient, which is the number of inpatient visits in the last year. This means that if patients have been to the hospital in the last year they are more likely to be re-hospitalized again. Another important feature is discharge_disposition_id_22 which is used if a patient is discharged to a rehab facility. For a given hospital, we might be able to research rules for being discharged to a rehab facility and add features related to those rules.




**CONCLUSION I**




We used recall to determine the best model so that we capture all the positives. Treating extra patients as ones likely to readmitted is better than missing out on patients likely to get readmitted. According to recall, decision tree turned out to be the best model.
Although in the end, none of the models were up to the mark. The accuracy and recall were low for all the models, and hence going forward with them or even the best performer amongst them will only increase the costs of the hospital. 
The models we used were standard models, both simple and complex. 
Our models did not work but we learnt a lot from this. Our data was insufficient as medical speciality and weight had over 50% of the data absent. 
Furthermore, we sub-sampled our data and left behind almost 80% of the data, which did not give the training model the exposure to all the types of cases, giving it one more reason to not work efficiently 
We had around 70,000 rows of sampled and shuffled data which helped in generalising the decision tree, hence making it the most efficient model.
Random forests are better than decision trees if and only if the trees suffer of instability.
If the predictions of the trees are stable, all sub models in the ensemble return the same prediction and then the prediction of the random forest is just the same as the prediction of each single tree. So then not only will the overall performance be the same, it will be the same cases that are predicted correctly and wrongly, respectively.

why not 100% accurate?
We train on data that is not representative for the test cases: the test cases cover regions of the input space that never appear in the training data. There is no way for a model to know which class (if any - or maybe a 3rd? ...) cases far outside training space should belong to.

Particularly for highly nonlinear partitioning models (such as the decision trees), leaving training space will typically rather sooner than later lead to disaster.
If you plan to train on one class only, you need to look into so-called one-class classifiers which try to establish independent boundaries for each class. One-class classification of your toy data should give you the result that the out-of-training-space cases do not belong to any of the known classes.
Decision trees are a partitioning method, they cannot do one-class classification.




## Conclusion II




We can infer that the readmission rate for men and women is almost similar although if we compare just the readmission count between Men and Women, women are more prone to get readmitted.
We can see that more the number of inpatients, outpatients and emergency in preceding year
Now here we notice that more the number of lab procedures make the patients more prone to get readmitted.
