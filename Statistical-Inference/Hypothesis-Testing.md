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

ggplot(data , aes(sample = x))+
+stat_qq(distribution=qnorm)+
+stat_qq_line(col="blue")+
+ggtitle("QQ-plot for revenue")+
+xlab("Theoritical Quantiles")+ylab("Sample Quantiles")

#το '+' χρησιμοποιειται για να προσθεσουμε διαδοχικα στοιχεια σε ενα γραφημα

#Kolmogorov-Smirnov test

#πρεπει να ειναι πληρως καθορισμενη η θεωρητικη συναρτηση κατανομης

Meanx= mean(x)
Sdx= sd(x)
ks.test(data$x,pnorm,meanx,sdx)

data%>%summarise(statistic = ks.test(data,"pnorm",meanx,sdx)$statistic, p.value=ks.test(data,"pnorm",meanx,sdx)$p.value)



#lilliefors test

#στην πραξη πολλες φορες δεν γνωριζουμε τις παραμετρους της κανονικης κατανομης , οποτε υπολογιζουμε τις εκτιμησεις τους απο τα δεδομενα και τις βαζουμε ως ορισματα στον ελεγχο

lillie.test(data$x)


#Shapiro-Wilk test(μονο για κανονικη κατανομη χρησιμοποιειται)

# W ανηκει (0,1) , οσο πιο κοντα στο 1 τοσο μεγαλύτερη η συμφωνια με την κανονικοτητα

shapiro.test(data$x)

#Οταν εχω δυο δειγματα και θελω να εξαιτασω την κανονικοτητα , μπορω να παρω την διαφορα των παρατηρησεων των δυο δειγματων και κανω shapiro.test στην νεα μεταβλητη difference.Αυτο ισχυει μονο οταν τα δειγματα ειναι paired.
Οταν τα δειγματα δεν ειναι paired εξαιταζεις την κανονικοτητα αναγκαστικα στο καθε δειγμα ξεχωριστα!

Difference = A-B
shapiro.test(difference)

#group_by

#Τα ταξινομει ως προς ενα κρητηριο και #μετα κανει summarise

data%>%group_by(type)%>%summarise(statistic= shapiro.test(data$x)$statistic,p.value=shapiro.test(data$x)$p.value)

#aggregate

aggregate(data$x ,
          by=list(data$type
          FUN= function(z) {y<-shapiro.test(data$x); c(y$statistic,y$p.value)})

