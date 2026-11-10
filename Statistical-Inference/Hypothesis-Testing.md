#Normality tests

#QQ plot
install.packages("ggplot2") #Δημιουργια στατιστικων γραφηματων και οπτικοποιησεων δεδομένων(Data Visualization)
install.packages("nortest") #Χρησιμοποιειται για ελεγχους κανονικοτητα(Normality tests)
library(ggplot2)
library(nortest)

#Πρωτος τροπος
qqnorm(data$x,main="qqplot for x",pch=1,frame=FALSE) # pch -> οριζει το συμβολο των σημειων(κυκλος) , frame=False -> δεν εμφανιζει το πλαισιο του γραφηματος
qqline(data$x,col="steelblue",lwd=2) # lwd-> Παχος της γραμμης

#Δευτερος τροπος

