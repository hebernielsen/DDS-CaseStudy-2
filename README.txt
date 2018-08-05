ReadMe.md for Project 2 - Predicting Attrition Using HR Data
Last Revised August 5, 2018

This Codebook is organized as follows:

1. Authors and Contact Information

2. Data
	2.A. Github Location Library
	2.B. Submission Files
	2.C. Core Data File Codebook

3. Processing Steps
	3.A. Answers to Case Study 2 Tasks
	3.B. Building the Model to Predict Attrition
	3.C. Validating the Model to Predict Attrition 



---------------------------------------------
1. Authors and Contact Information

This codebook is for the case study group of SMU MSDS 6306 404 members:

Quincy Roundtree | e: qroundtree@mail.smu.edu p: 702.305.9592
Christopher Graves | e: ccgraves@mail.smu.edu p: 214.542.1971
Allen Crane | e: acrane@mail.smu.edu p: 210.913.5072
Nick Cellini | e: ncellini@smu.edu p: 717.490.4880
Heber Nielsen | e: hcnielsen@mail.smu.edu p:


2. Data
	2.A. Github Location Library
		
		https://github.com/ccgraves/DDS-CaseStudy-2 	

	2.B. Submission Files

		Project 2.rmd (Case Study 2 Markdown file)
		Project 2.html (Case Study 2 HTML file)		
	
	2.C. Core Data File Codebook
		
		CaseStudy2-Data.xlsx (Core data file containing HR data and Attition information)


Variable		Variable Type				Allowable Values

Age			numeric:integer, countable value	18-60 (countable)

Attrition		factor					Yes, No

BusinessTravel		factor					'Non-Travel'
								'Travel_Rarely'
								'Travel_Frequently'

DailyRate		numeric:integer, countable value	102-1499

Department		factor					'Human Resources'
								'Research & Development'
								'Sales'
DistanceFromHome	numeric:integer, countable value	1-29 (countable)

Education		numeric:integer, coded value		1 'Below College'
								2 'College'
								3 'Bachelor'
								4 'Master'
								5 'Doctor'

EducationField		factor					'Human Resources'
								'Life Sciences'
								'Marketing'
								'Medical'
								'Other'
								'Technical Degree'

EmployeeCount		numeric:integer, countable value	1 (only value)

EmployeeNumber		numeric:integer, identity value		1 - 2068 (identity value)

EnvironmentSatisfaction	numeric:integer, coded value		1 'Low'
								2 'Medium'
								3 'High'
								4 'Very High'

Gender			factor					Female, Male

HourlyRate		numeric:integer, countable value	30-100 (countable)

JobInvolvement		numeric:integer, coded value		1 'Low'
								2 'Medium'
								3 'High'
								4 'Very High'

JobLevel		numeric:integer, coded value		1-5

JobRole			factor					Sales Executive
								Research Scientist
								Laboratory Technician
								Manufacturing Director
								Healthcare Representative
								Manager
								(Other)

JobSatisfaction		numeric:integer, coded value		1 'Low'
								2 'Medium'
								3 'High'
								4 'Very High'

MaritalStatus		factor					Divorced
								Married
								Single

MonthlyIncome		numeric:integer, countable value	1009-19999 (countable)

MonthlyRate		numeric:integer, countable value	2094-26999 (countable)

NumCompaniesWorked	numeric:integer, countable value	0-9 (countable)

Over18			factor					Yes (only value)

OverTime		factor					No, Yes

PercentSalaryHike	numeric:integer, countable value	11-25 (countable)

PerformanceRating	numeric:integer, coded value		1 'Low'
								2 'Good'
								3 'Excellent'
								4 'Outstanding'

RelationshipSatisfaction numeric:integer, coded value		1 'Low'
								2 'Medium'
								3 'High'
								4 'Very High'

StandardHours		numeric:integer, countable value	80 (one value)

StockOptionLevel	numeric:integer, coded value		0-3

TotalWorkingYears	numeric:integer, countable value	0-40 (countable)

TrainingTimesLastYear	numeric:integer, countable value	0-6 (countable)

WorkLifeBalance		numeric:integer, coded value		1 'Bad'
								2 'Good'
								3 'Better'
								4 'Best'

YearsAtCompany		numeric:integer, countable value	0-40 (countable)

YearsInCurrentRole	numeric:integer, countable value	0-18 (countable)

YearsSinceLastPromotion	numeric:integer, countable value	0-15 (countable)

YearsWithCurrManager	numeric:integer, countable value	0-17 (countable)


3. Processing Steps

		Both Project 2.rmd and Project 2.html are organized according to the following sections
	
	3.A. Answers to Case Study 2 Tasks
		Data cleansing
			Read the data into R
			Reformat column names to 12 characters or less
			Validate columns are in proper data type
	     	Preliminary Analysis	
			Remove observations where participant is under 18
			Descriptive Statistics on 10 variables
			Histograms for Age, Monthly Income
			Frequencies for Gender, Education, Occupation
			Counts of management positions
		Deeper Analysis and Visualization
			Bar charts in ggplot or similar
			See Regression Coefficients output from Predictive Model
			Relationship between Age and Income
			Life Satisfaction		

	3.B. Building the Model to Predict Attrition

		i.	The following is our approach to build a predictive model for Attrition. 
			This will form the basis of our recommendation in our presentation.
		ii.	Add row number to master, and insert as a unique column into data set. 
			We will break out the master file into a factor data set and a categorical data set. 
			The categorical data set will be flattened into a one hot encoded file. 
			Preserving the ID column allows us something to join on, when we merge these 
			files back together when we build the model.
		iii.	Split data set into factors and categories, including row IDs
		iv. 	One hot encoding for categorical data
		v.	Merge data files together
		vi.	Remove the Attrition categorical variables, because they are perfectly correlated values
		vii.	The following modeling code was adapted from: 
			https://datascienceplus.com/perform-logistic-regression-in-r/
			A visual take on the missing values might be helpful: the Amelia package has a special 
			plotting function missmap() that will plot your dataset and highlight missing values. 
			In our case, this adds nothing meaningful.
		viii.	We split the data into two chunks: training and testing set. 
			The training set will be used to fit our model which we will be testing over the testing set. 
			Note that the training data set is ~2/3 the size of the total data, because a training 
			set at 1/2 the size of the total data did not converge.
		ix.	Now, let’s fit the model. Be sure to specify the parameter family=binomial 
			in the glm() function. Note that we upped the max iterations to 50, 
			in order to override the default number of iterations. 
			That said, this model converges at 15 iterations.
		x.	By using function summary() we obtain the results of our model
		xi.	Format the model outputs to create a list of the top coefficients, 
			by the absolute Z Score values.
		xii.	Output Coefficients of the model into a data frame. 
			We will format these to graph the top Coefficient values
		xiii.	Create Model Output 2 file of Coefficients and Z Scores
		xiv.	Create Model Output 3 file of Top 10 Coefficients by absolute value of Z Scores
		xv.	Create Model Output 4 file that sorts the Top 10 Coefficients for graphing
		xvi.	Graph the Top 10 Coefficients by Z Score
		xvii.	Conduct what-if analysis, showing the effect of moving each variable 1 unit, 
			and the subsequent impact on Attrition

	3.C. Validating the Model to Predict Attrition 

		i.	Validating the model for fit
			Now we can run the anova() function on the model to analyze the table of deviance 
		ii.	While no exact equivalent to the R2 of linear regression exists, 
			the McFadden R2 index can be used to assess the model fit.
		iii.	Assessing the predictive ability of the model: In the steps above, we briefly 
			evaluated the fitting of the model, now we would like to see how the model is 
			doing when predicting y on a new set of data. By setting the parameter type=‘response’, 
			R will output probabilities in the form of P(y=1|X). Our decision boundary will be 0.5. 
			If P(y=1|X) > 0.5 then y = 1 otherwise y=0. Note that for some applications different 
			decision boundaries could be a better option.
		iv.	As a last step, we are going to plot the ROC curve and calculate the AUC 
			(area under the curve) which are typical performance measurements for a binary classifier. 
			The ROC is a curve generated by plotting the true positive rate (TPR) against the false 
			positive rate (FPR) at various threshold settings while the AUC is the area under the ROC 
			curve. As a rule of thumb, a model with good predictive ability should have an AUC closer 
			to 1 (1 is ideal) than to 0.5.







