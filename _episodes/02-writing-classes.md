---
title: "Writing classes"
teaching: 30
exercises: 20
questions:
- "How are classes written in Python?"
- "What do methods look like?"
- "How can a class customise how its instances are constructed?"
objectives:
- "Write classes from scratch"
- "Write methods for classes"
- "Write custom `__init__` methods"
keypoints:
- "Classes in Python are blocks started with the `class` keyword"
- "Method definitions look like functions, but must take a `self` argument"
- "The `__init__` method is called when instances are constructed"
---

In the previous section, we've seen how objects can have different behaviour, provided by methods, which in turn are provided by the class of an object.

But what if we want to make our own classes and objects?

Let's assume that we want to write our own linear regression class. Our linear regression object should bahve similarly to `sklearn.linear_model.LinearRegression` -- it should have `fit` and `predict` methods. Here is a possible implementation:

~~~
import numpy

class MyLinearRegression:

    def __init__(self):
        """Constructor"""
        self.coef_ = []

    def fit(self, X, y):
        """Fit the linear model"""
        Xa = numpy.array(X)
        Xt = Xa.transpose()
        self.coef_ = numpy.linalg.inv(Xt @ Xa) @ Xt @ y

    def predict(self, X):
        """Predict using the linear model"""
        return X @ self.coef_
~~~
{: .language-python}

First we define the class with the `class` keyword. The name of the class is `MyLinearRegression`. The class has one attribute, `self.coef_`, which represents the least square coefficients. We don't know how many coefficients there will be so we initialise `self.coef_` to an empty list.

Next we implement two methods: `fit` and `predict`. The `fit` method does not return anything, it computes the linear square coefficients. The 
`predict` method takes data values and computes the "best" estimates for those values.

In addition there is the `__init__` method. This is a special method used to construct the object. In this case `__init__` does not take any arguments. 

All the methods are contained within the `class` definition and take `self` as first argument. `self` refers to the 
object itself _within the class_. Thus, `self.coef_` is the `coef_` attribute attached to this particular instance. 

> ## Pronouncing `__init__`
>
> The method name `__init__` is most often pronounced "dunder init",
> where the "dunder" is short for "double underscore", since the name
> starts and ends with two underscores.
>
> We'll encounter more methods with "dunder" in the name in a later episode.
{: .callout}


> ## Other names than `self`
>
> While it is possible to use any variable name for the first argument of a
> method, and Python will not complain, other programmers will. Since one aim
> when programming is to be as clear as possible to others who may read the
> program later, we strongly recommend following the convention of calling
> the first argument to methods `self`.
{: .callout}

> ## Naming classes
>
> Another convention in Python is that class names start with a capital letter,
> and instead of underscores, initial letters of subsequent words are also
> capitalised. This makes it easier to distinguish classes from objects and
> other variables at a glance.
{: .callout}

This is how you could use the class:
~~~
mymodel2 = MyLinearRegression()
mymodel2.fit(X=[[1,],[2,]], y=[1, 2])
ypred = mymodel2.predict(X=[[1.2,],[1.8,], [2.2,]])
~~~
{: .language-python}

Note that our new class behaves the same way as `sklearn.linear_model.LinearRegression`, which we used in the previous episode. We could use `MyLinearRegression` in place of `sklearn.linear_model.LinearRegression` in our scripts. The changes would be minimal because both `MyLinearRegression` and `sklearn.linear_model.LinearRegression` mostly conform to the same application interface. 

> ## Problem
>
> Add an attribute `intercept_` to  class `MyLinearRegression` and initialise this attribute to zero.
>
>> ## Solution
>>~~~
>>import numpy
>>
>>class MyLinearRegression:
>>
>>    def __init__(self):
>>        """Constructor"""
>>        self.coef_ = []
>>        self.intercept_ = 0.0
>>
>>    def fit(self, X, y):
>>        """Fit the linear model"""
>>        Xa = numpy.array(X)
>>        Xt = Xa.transpose()
>>        self.coef_ = numpy.linalg.inv(Xt @ Xa) @ Xt @ y
>>
>>    def predict(self, X):
>>        """Predict using the linear model"""
>>        return X @ self.coef_
>>~~~
>>{: .language-python}
> {: .solution}
{: .challenge}

## Using inheritance to simplify `MyLinearRegression`

Perhaps we like class `sklearn.linear_model.LinearRegression` but would like to change just one method, `fit`, because we think ours is best. Do we really need to rewrite the entire class from scratch? 

How about we derive our class from `sklearn.linear_model.LinearRegression`? 
~~~
from sklearn.linear_model import LinearRegression

class MyLinearRegression2(LinearRegression):

    def fit(self, X, y):
        """Fit the linear model"""
        Xa = numpy.array(X)
        Xt = Xa.transpose()
        self.coef_ = numpy.linalg.inv(Xt @ Xa) @ Xt @ y
        self.intercept_ = 0.

~~~
{: .language-python}

> ## Problem
>
> Class `sklearn.linear_model.LinearRegression` has additional members and methods. One such method is `score`. Check that instances of `MyLinearRegression` can call `score`.
>
>> ## Solution
>>~~~
>>m = MyLinearRegression2()
>>m.fit(X=[[1,],[2,]], y=[1, 2])
>>m.score(X=[[1.2,],[1.8,], [2.2,]], y=[1.2, 1.8, 2.2])
>>~~~
>>{: .language-python}
> {: .solution}
{: .challenge}
