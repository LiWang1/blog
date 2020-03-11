#Computational statistics 
### 1, Boolean vectors can be used to obtain a subset of rows and colunms

```
#example
train = (year<2005)
Smarket.2005 = Smarket[!train,]
dim(Smarket.2005)

```
The elements of the vector that corresponds to observations that occurred before 2005 are set to TRUE. The object train is a Boolean vector, since its elements are TRUE and FALSE. 

Similarly, we can remove the specific columns and rows in this way: 

```
#remove the first column
Advertising <- Advertising[,-1] 
```

### 2, glm() function 
The glm() function fits generalized linear models, a class of models that includes logistic regression. The syntax of the glm() function is similar is similar to that of lm(), except that we must pass in the argument family = binomial in order to tell R to run a logistic regression. the predict() function can be used to predict the probability of the given class. The type = 'response' option tells R to output probability. The contrasts() function can show how does R encode the dummy variable 

### 3, Be careful with the intepretation of multiple regression!
```
# We illustrate this in a small simulation
n <- 10000         # use large sample size to get precise estimates
x1 <- rnorm(n,0,1)
x2 <- x1 + rnorm(n,0, 0.1)
y <- 2*x1 - x2 + rnorm(n,0,1)
x3 <- y + rnorm(n,0,1)

# limit number of digits:
options(digits=2)

# Fit different regression models:
coef(lm(y~x1))       # beta1=1
coef(lm(y~x2))       # beta2=1
coef(lm(y~x3))       # beta3=2/3
coef(lm(y~x1+x2))    # beta1=2, beta2=-1
coef(lm(y~x1+x2+x3)) # beta1=1, beta2=-1/2, beta3=1/2
```

Comments:

* Interpretation of each beta_k depends on the other variables in the model! 
* beta_k is the "effect" of x_k on Y when all other variables in themodel are held constant. This "effect" should not be interpreted as a causal effect, but in the sense described above. 

### Parameters to measure the goodness of fit 
* Residual standard error (RSE)

```
n <- nrow(Advertising)
p <- 4    # intercept and three variables for instance
rse = sqrt(sum(residuals(fit.all)^2)/(n-p))
```
* R^2 proportion of variance already explained and the adjusted R^2 when taking into account the number of variables in the model 

```
RSS <- sum( (y-y.hat)^2 )
TSS <- sum( (y-mean(y))^2 )
Rsquared <- 1 - RSS/TSS 

# Re-compute adjusted R^2:
Rsquared.adj <- 1 - (RSS/(n-p))/(TSS/(n-1))
Rsquared.adj
```

### One puzzle (overall F test and individual p values) 
This is a question from the series two of the computational statistics. Specifically, we trained a linear model:
> fit1 = lm(y~x1 + x2) 

From the summary of fit1, we can see p-values of x1 and x2, both of them are above 0.05 and therefore are not significant. 

Then, we use the anova, to compare the model with and without x1 and x2, and we find that the p-value of the F-test is in favor of the more complex model. 

So the question goes to: the overall F-test is significant. However, the p-values for x1 and x2 are not signigicant. How can this be true?

Attached is the answer given by the TAs:(this detailed and refreshing answer is given by Peter, one of the TAs of computational statistics) 

```
the F test compares two models: 
M_12: Y=\beta_0 +\beta_1 X_1+\beta_2 X_2+\varepsilon 
M_0: Y=\beta_0+\varepsilon. 
Here, the F test is significant at the given level, which means that 
there is statsitical evidence to favour the bigger model M_12 over 
the smaller M_0.

The individual T tests for the parameters compare 
M_12 against M_1: Y=\beta_0 +\beta_1 X_1 +\varepsilon and 
M_12 against M_2: Y=\beta_0+\beta_2 X_2+\varepsilon. 
These tests are not significant and the given level which means that
there is not enough statistical evidence to favour M_12 over M_1 or M_2.

The question says "explain how this can be true" and the answer is
that there is no reason why it should not be true. 
We asked the question to make clear that if both individual tests are
non-significant, this does not mean that we can take away both
variables from the model (There are always some students who think that). 
```

SO bascially, we can see that the F-test adn the individual T tests compared different models. Also the last sentence is meaningful: if both individual tests are non-significant, this does not mean that we can take away both variables from the model. 



