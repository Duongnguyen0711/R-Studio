# R-Studio
Apply clustering method for detecting anomalies in Vietnamese National High School exam scores of Ha Giang province

In 2018, the Vietnamese Ministry of Education and Training published the National High School exam results and the data for analysis in this assignment was downloaded via the following link:    
https://www.mediafire.com/file/v04b47bi8dad47n/THPT_2018_Quoc_gia.csv/file.
The data file contains the exam results of 744,396 candidates in 9 subjects: Math, Physics, Chemistry, Vietnamese, History, Geography, Biology, English and GDCD (Civic Education). 
For every R-Studio project, we must set Working Directory for RStudio file by following the steps: Session -> Set Working Directory -> Choose Directory, then we will create a new R file by the following path: File -> New File -> R Script. To load the data file “THPT 2018 Quoc gia.csv” into RStudio, we need to make sure it is in the same Directory as our RStudio file.
The “THPT 2018 Quoc gia.csv” is a file with csv format, thus, to use “read_csv” function we need to install library (tidyverse). Tidyverse is known as a collection of R packages designed specifically for data science, which share a common data structure and grammar foundation (Wickham & Grolemund, 2017). The packages in tidyverse library can be mentioned as ggplots2, dplyr, tidyr, readr, purrr, tibble, stringr, forcats (tidyverse.org, n.d). These packages will cooperate closely for creating a completed data analysis workflow. 
First, we need to load the file “THPT 2018 Quoc gia.csv” into R using package read_csv
 
The candidate's identification number must include 8 digits, and the first two digits of the ID number will represent for code of the student’s province, for example, candidates who have the 2 first digits of ID number stating “01” come from Hanoi, candidates who have the first 2 digits of ID number starting with “05” come from Ha Giang Province.
However, when entering these ID numbers into an excel file, the candidates who have an identification number starting with "0" were deleted the first digit and only 7 digits remain. Hence, we need to add the number “0” before the identification number of those whose 7 digits. To do that, we will use the mutate () function.

 
After creating 1 more column to represent the province code of candidates, we can use subset() command to filter out candidates from Ha Giang only and we set the name for that data frame as “hg_scores”.

	K-means Clustering algorithm 
In this report, the data of 4 subjects: Math, Physics, Chemistry and Biology will be used for clustering due to 2 main reasons: Firstly, the four subjects are belonging to STEM field, thus learners of these subjects tend to have similarities regarding intellectual capacity, test-taking skills, and methods of studying. For example, students who do well in Math will find learning Physics very light because having logical thinking and accurate calculation ability will help them to perform the calculation of formulas and laws in physics easily. Second, the combination of these subjects plays an essential role in determining the ability of a student to enter a University in Vietnam. If students have a goal to pass a university, they must put their effort equally at least 3 per 4 above subjects. Given the above 2 arguments, the scores of these 3 out of 4 subjects must be closely correlated, otherwise we will have to check carefully for any anomalies.
We can extract data of these 4 exam subjects by using select() command, then eliminate missing values by using na.omit() command. After executing these 2 commands, we will get 1 data frame with 560 observations and 4 variables, which means that there were 560 students taking all the 4 subjects.
After having the data frame for analysis, we will find the optimal number of Clusters by using Elbow method. To execute the command of Elbow method, we need to call 2 libraries: factoextra and NbClust. 

 

 
The above graph represents the optimal number of clusters based on the within group sum of square. The K-means algorithm was run 10 times and the error dropped drastically from the first cluster to the second cluster was introduced. From the fifth cluster, the within sum of square is very stable, thus I will decide the number of clusters for K-means method will be 5.
The next step will be Visualization of clusters.
 

 

From the cluster plot above, we can see that there are outliers in each cluster. To have a better understanding of these clusters and how outliers were distributed, we will look at the following boxplot.
To draw the boxplot in R-Studio, we need to call the library (lvplot).
 

 

From the above boxplot, we can see that the first cluster shows the exam results of those students who have weak to average academic ability with grades for 4 subjects ranging from 2 to 6. In the second cluster, the results for two subjects Math and Physics are very high, almost higher than 8, but Chemistry and Biology are very low (lower than 3). The third cluster represents the results of Math, Physics and Chemistry are extremely high, but Biology is low. The fourth cluster is students with average to good academic performance who have test scores in the 4 subjects distributed from 4 to 8. The fifth cluster is those who have the very weak academic ability with almost 4 subjects lower than 5. As argued above, the scores of three out of four subjects must be similar, however, there is a clear difference between the scores in Math & Physics compared with Chemistry & Biology in cluster 2. Hence, we can indicate that outliers will be gathered in the second cluster. 
In the next step, we will find the outliers by looking at the distance from the center of clusters to the data points. The 10 data points with the furthest distance to the center will be outliers.
 

After doing above commands, the 10 outliers will be:
 

As expected prediction above, the 10 outliers are all in the second cluster with very high scores in Math & Physics but very low in Chemistry & Biology.

 
V.	Conclusion
Through out the assignment, different Machine learning algorithms have been discussed. Then the K- Means Clustering has been applied to detect anomalies in National High School exam results in Ha Giang Province of Vietnam.  Ten outliers have been listed out, which are students: 536, 427, 423, 294, 535, 413, 511, 269, 187 and 362.
![image](https://github.com/Duongnguyen0711/R-Studio/assets/141879633/783399cd-8876-4b48-b3ad-17933312df03)
