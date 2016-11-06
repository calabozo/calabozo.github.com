---
layout: default
title: Linear Regression in R
---

We define the basic input data with noise


```R
x <- seq(-10,10,length.out=100)
a <- 2
b <- 5
Y <- a+b*x
Xn <- x+cbind(rnorm(length(x),sd=2),rep(1,times=length(x)))

plot(Xn[,1],Y)
abline(c(a,b),col="blue")
```


![png](output_1_0.png)


We implement the Gradient descent method to calculate the parameters a and b


```R
Theta <- matrix(c(1,1),nrow=2)

#Cost function
J <- function(X,Theta, Y){
  j <- 1/length(X)*( sum(X %*% Theta - Y)^2)
}

#Derivate of the cost function
dJ <- function(X,Theta, Y){
  dj <- 1/length(X)*t(X)%*%( X %*% Theta - Y)
}

maxIter <- 1000
alpha <- 0.01
err<-rep(0,maxIter)
for (i in 1:maxIter){
    Theta <- Theta - alpha*dJ(Xn,Theta,Y)
    err[i] <- J(Xn,Theta,Y)
}

an <- Theta[1]
bn <- Theta[2]

print(paste("Estimated value of a=",an,", b=",bn))
```

    [1] "Estimated value of a= 0.614237488865548 , b= 4.32518417215818"



```R
plot(err, main="Mean Square Error")
```


![png](output_4_0.png)


The approximation to our curve in red with the original curve in red:


```R
plot(Xn[,1], Y)
abline(Theta,col='red')
abline(c(a,b),col='blue')
grid()
```


![png](output_6_0.png)



```R
Another way to do this is using the Moore–Penrose pseudoinverse matrix
```


```R
Theta<-solve(t(Xn) %*% Xn)%*%t(Xn)%*%Y
print(paste("Estimated value of a=",Theta[1],", b=",Theta[2]))
```

    [1] "Estimated value of a= 0.614233014830346 , b= 4.32518880315233"


This pseudoinverse is included in the MASS R library as ginv(A)


```R
library(MASS)
Theta<-ginv(Xn)%*%Y
print(paste("Estimated value of a=",Theta[1],", b=",Theta[2]))
```

    [1] "Estimated value of a= 0.614233014830342 , b= 4.32518880315233"


But R also incorporates a function to calculate linear regression. This is the one which produces the better output


```R
fit <- lm(Y ~ Xn[,1])
print(paste("Estimated value of a=",fit$coefficients[1],", b=",fit$coefficients[2]))

plot(Xn[,1], Y)
abline(fit,col='red')
abline(c(a,b),col='blue')
grid()
```

    [1] "Estimated value of a= 2.40283864349388 , b= 4.51088771771247"



![png](output_12_1.png)

