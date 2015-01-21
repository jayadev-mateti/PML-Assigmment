# PML-Assigmment
PML-Assignment


Version1
training<-read.csv("D:\\DDrive\\DSP\\Practical Machine Learning\\pml-training.csv",na.strings=c("#DIV/0!","","NULL","NA"))

dim(training)
str(training)
View(training)
summary(training)





for (Var in names(training)){
	 missing<-length(which(is.na(training[,Var])))
	 print(c(Var,missing))}
	OR
miss<-colSums(is.na(training))
	


training1<-training[,colSums(is.na(training))<(.4*length(training))]
training1<-training[,colSums(is.na(training))<19000]
miss<-colSums(is.na(training1))
dim(training1)

remove = c("X","user_name", "raw_timestamp_part_1", "raw_timestamp_part_2", "cvtd_timestamp", "new_window", "num_window")
training2<-training1[,-which(names(training1) %in% remove)]
dim(training2)

#Seeing correlations between predictors#
corrMatrix<-abs(cor(training2))
#setting the correlations with self to zero
diag(training2)=0
which(corrMatrix>0.8,arr.ind=T)

#Dividing the traing set into training and test sets#
inTrain = createDataPartition(y = training2$classe, p = 0.7, list = FALSE)
train = training2[inTrain, ]
test = training2[-inTrain, ]

#Running PCA after centering and scaling#
preProc<-preProcess(train[,-53]+1,method=c("scale","center","pca"),thresh=.99)
train_PC<-predict(preProc,train[,-53]+1)
test_PC<-predict(preProc,test[,-53]+1)


#Running Random forest#
model<-train(train$classe~.,data=train_PC,method="rf",ntree=2,importance=TRUE)
model1<-train(train$classe~.,data=train_PC,method="rf",traincontrol=c(method="cv",number=10),importance=TRUE)


model$finalModel
varImp(model)


#Testing on test set using cross validation#

#Predicting the 20 validation cases#





