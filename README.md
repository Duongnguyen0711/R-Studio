# R-Studio
Apply clustering method for detecting anomalies in Vietnamese National High School exam scores of Ha Giang province

library(tidyverse)
#Load data into R
national_scores <- read_csv("THPT 2018 Quoc gia.csv")

#Add the "0" to the ID number of those whose ID number contains only 7 digits

national_scores %>% mutate(SoBD = as.character(SoBD)) %>%  mutate(SoBD = case_when(str_count(SoBD) == 7 ~ paste0("0", SoBD), TRUE ~ SoBD)) %>% 
 
#Create 1  more column representing the province code of candidates

mutate(code = str_sub(SoBD, start = 1, end = 2)) -> national_scores

# Using subset() command to create a data frame of those students who come from Ha Giang province only.

hg_scores <- subset(national_scores, code =="05")

#extract data of 4 subjects Math, Physics, Chemistry and Biology
df_hg_scores = hg_scores %>% select(Math, Physics, Chemistry, Biology) %>% na.omit()

library(factoextra)
library(NbClust)
# Plot the optimal number of clusters
fviz_nbclust(df_hg_scores, kmeans, method = "wss") + labs(subtitle = "Elbow method") 

#Visualization of clusters
km <- kmeans(df_hg_scores, 5, nstart = 25)
fviz_cluster(km,df_hg_scores, ellipse.type = "norm")

library(lvplot)

#Boxplot visualization
df_hg_scores%>%mutate(cluster=factor(km$cluster))%>% gather(Math:Biology,key="Exams",value="Scores")%>%
  ggplot()+geom_lv(aes(x=Exams,y=Scores,fill=..LV..),col="blue",show.legend = F,)+
  facet_wrap(~cluster,ncol=2)+ coord_flip()+ scale_fill_brewer(palette="Reds",direction = -1)+theme_bw()

#Data frame of the  5 centers
centers <- km$centers[km$cluster]
#Distance between the objects and cluster centers
distances <- sqrt(rowSums(df_hg_scores- centers)^2)
#10 outliers with largest distance from the cluster centers 
outliers <- order(distances, decreasing = T) [1:10]

print(outliers)

print(df_hg_scores[outliers, ])
