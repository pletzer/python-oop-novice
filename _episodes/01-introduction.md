---
title: "Objects in Python"
teaching: 30
exercises: 20
questions:
- "What is object oriented programming?"
- "When should I use object oriented programming?"
- "What are objects in Python?"
- "What is a class or type?"
- "Can objects belong to more than one class?"
- "How can objects be created from a class?"
objectives:
- "Be able to distinguish between class and object"
- "Be able to construct objects via a class's constructor"
- "Be able to distinguish between equality and identity of objects"
keypoints:
- "Anything that we can store in a variable in Python is an object"
- "Every object in Python has a class (or type)"
- "`list` and `numpy.ndarray` are commonly-used classes; lists and arrays are corresponding objects"
- "Calling the class as a function constructs new objects of that class"
- "Classes can inherit from other classes; objects of the subclass are automatically also of the parent class"
---

## What is object oriented programming?

Since the 1990s, Python has grown in popularity. As a result, the complexity of some Python packages has reached the point where their 
application programming interface no longer reduces to simple function calls. Such packages might have an 
_object oriented_ design.

As a Python user, you will likely need to grasp the essence of object oriented programming in order to use these packages. Such packages are built around _objects_, i.e. data structures with _attributes_ and _methods_. Attributes are properties, we can think of them as adjectives (e.g. "red", "heavy", etc.). Methods are functions that operate on the object or other objects, we can think of them as verbs (e.g "walk", "eat", ...). The concepts of objects, attributes and methods will be further clarified in the course of this workshop.

Object oriented programming exists in other languages (C++, Java and Fortran). Many concepts presented here apply equally to these languages. However, Python is the ideal language to learn object oriented programming due to its interactivity.

## When should I use object oriented programming?

Object oriented programming allows you to perform complex tasks sometimes in more efficient way.

Consider for example a plotting package. Plotting data typically involves many fine grained operations (creating the plot, adding axis labels, title, legend etc.). On could write a function that performs all these operations with a single call. However, this function would take a lot of arguments to control all aspects of plotting. What if you wanted to change the title? You would have to call the function again with the same arguments except for one small change. This is error prone but also inefficient since most of the plot was fine and only minor tweaking was needed.

You may face the dilemma of using a function or a class when implementing code. Using a _procedural_ approach may be the right choice if the code

 * Performs a simple operation
 * The function takes only a few arguments
 * The function is pure (does not have a state) 
 * And you don't expect the need to add functionality with time

## An example showing the difference between procedural and object-oriented programming

In the procedural programming approach, the mean of a Numpy array can be computed by using
`numpy.mean`,

~~~
import numpy
numbers = numpy.arange(10)
print(numpy.mean(numbers))
~~~
{: .language-python}


However, we can also calculate the mean of a Numpy array as:

~~~
print(numbers.mean())
~~~
{: .language-python}


> ## The dot notation
>
> Note the `.` (dot) separating the object (`numbers`) from the `mean` method. The dot notation is a common feature of many object oriented programming languages. It means `mean` is a *function that belongs to*. In the first case, `mean` belongs to module `numpy` and in the second case `mean` belongs to object `numbers`. 
{: .callout}

Let's see if we can do this with a normal list:
~~~
more_numbers = [1, 2, 3, 4]
print(more_numbers.mean())
~~~
{: .language-python}

In this case Python will complain with an error. How does Python know it can do this for `numbers` but not `more_numbers`? This will be explained later.

## An example of object oriented design from the scikit-learn package

The example below shows how object oriented programming lies at the heart of many Python packages. Here, a linear regression model `mymodel` is created, the model is fitted with data and predictive values `ypred` are inferred from the model after it is fitted, 
~~~
from sklearn.linear_model import LinearRegression
mymodel = LinearRegression(fit_intercept=False)
mymodel.fit(X=[[1,],[2,]], y=[1, 2])
ypred = mymodel.predict(X=[[1.2,],[1.8,], [2.2,]])
~~~
{: .language-python}
While it would have been possible to write a function that performs the `fit` and `predict` operations together, equivalent to
~~~
ypred = LinearRegression(fit_intercept=False).fit(X=[[1,],[2,]], y=[1, 2]).predict(X=[[1.2,],[1.8,], [2.2,]])
~~~
{: .language-python}
the object oriented design provides additional advantages. Specifically, the `predict` operation can be called as many times as desired once the model is fitted. In many machine learning algorithms, predicting values is cheap compared to fitting. Therefore, by separating the `fit` and the `predict` calls we can be more efficient.

> ## Discuss the pros/cons of objected oriented programming over procedural programming
>
>> ## Solution
>> This is not an exhaustive list but here are some differences:
>>  * The procedural approach is _generic_, it works both on lists and numpy arrays
>>  * The object oriented approach requires a specific type, which makes it safer
>>  * The object oriented way may be more concise
>>  * In the object oriented way we're telling people what they can do with an object
> {: .solution}
{: .challenge}

## What type is it?

We mentioned that object oriented programming attaches functions to types. Let's investigate this further by using `type` to identify what the data type of `numbers` is:

~~~
type(numbers)
~~~
{: .language-python}

~~~
<class 'numpy.ndarray'>
~~~
{: .output}

What about the type of the variable `more_numbers`?

~~~
type(more_numbers)
~~~
{: .language-python}

~~~
<class 'list'>
~~~
{: .output}
We can see here that `numbers` is an object of the type `numpy.ndarray`. In Python, anything which can be stored in a variable or passed to a function is called an _object_. Objects are classified by their `type`, or their `class`.

> ## Class or Type?
>
> Note that in literature, you'll find a subtle
> distinction between _class_ and _type_. However, since in Python 3
> we can't have one without the other, we will use both terms interchangably.
{: .callout}

> ## Let's find some types
>
> Can you find anything that you can store in a variable which
> does not have a class? What is the type of the number `1`,
> or the string `"hello"`?
>
> Does the class change if they are passed directly to `type`,
> or if they are stored in a variable?
>
>> ## Solution
>>
>> ~~~
>> type(1)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'int'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> type("hello")
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'string'>
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

> ## Does everything have a class?
>
> Try to find words that Python recognises that do not have classes. What about `numpy.mean` or `numpy`?
> What about `if` or `for`? Can you think of others?
>
>> ## Solution
>>
>> ~~~
>> type(numpy.mean)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'function'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> type(numpy)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'module'>
>> ~~~
>> {: .output}
>> The objects `numpy.mean` and `numpy` are things that we typically wouldn't store in variables or passed around. However, they could in principle be stored in variables, and therefore are objects with a class.
>> ~~~
>> type(if)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>>  File "<stdin>", line 1
>>    type(if)
>>          ^
>>SyntaxError: invalid syntax
>> ~~~
>> {: .output}
>> ~~~
>> type(for)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>>  File "<stdin>", line 1
>>    type(for)
>>          ^
>>SyntaxError: invalid syntax
>> ~~~
>> {: .output}
>> The words `if` and `for` are part of the Python language itself, they can't be stored in variables. Only things which can be stored in variables can have a class.
> {: .solution}
{: .challenge}


## Instances and Methods

We say that an object of a particular class is an _instance_ of that
class. To use a real world example, we could have the type or class
`Chair` which describes all the chairs in the world. The chair that
you are sitting on right now is a specific _instance_ of the chair
class.

We can check if an object is an _instance of_ a particular class with the `isinstance` function.

~~~
isinstance(numbers, numpy.ndarray)
~~~
{: .language-python}

~~~
True
~~~
{: .output}

Every object is created with a single class, and which can't be changed. The class of an object can also provide behaviour that the object might have, by providing functions to objects in its class. As seen before, these functions can be called by using a dot after the variable name, for example:
~~~
numbers.mean()
~~~
{: .language-python}

The functions which are associated with an object are provided by the class of the object. When a class provides a function to an object we call that function a _method_ of the class.

We say that the `numpy.ndarray` class provides the `mean` _method_. Since `numbers` belongs to the class `numpy.ndarray`, we can use the `mean` method on the object referred to by `numbers`, by calling `numbers.mean()`. This allows objects of a `numpy.ndarray` to provide functionality specific to objects of class `numpy.ndarray`.

It's worth noting that both mutable and immutable objects can have methods. Methods of immutable objects, however, can't change
the underlying object. If needed, they will return a brand new object, and set the expected value in the new object. To keep this change,
you will need to store it in a variable, for example:
~~~
hello = "hello, world"
capital_hello = hello.capitalize()
print(capital_hello)
~~~
{: .language-python}

Methods of mutable objects can, and often do, change the object.
~~~
grades = [84, 78, 91]
grades.append(66)
print(grades)
~~~
{: .language-python}

~~~
[84, 78, 91, 66]
~~~
{: .output}

In this case, we don't need the extra `=` to assign the value to a new object.

> ## Finding out what things are
>
> use `type()` to find the type of `students`, defined as
>
> ~~~
> students = ['Petra', 'Aalia', 'Faizan', 'Shona']
> ~~~
> {: .language-python}
>
> and check this with `isinstance`.
>
>> ## Solution
>>
>> ~~~
>> type(students)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'list'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> isinstance(students, list)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> True
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

> ## Other common classes
>
> What other classes have you encountered previously when using Python?
> What methods did they provide?
{: .challenge}

## Making an object

A class can be called as a function, in which case it constructs new instances
of itself. While this is not the only way to make objects, it is one that all
classes offer. For example, a new list can be created as:

~~~
students = list()
print(students)
print(type(students))
~~~
{: .language-python}

~~~
[]
<class 'list'>
~~~
{: .output}

> ## Making a Numpy array
>
> While all classes can be constructed by calling their name, some classes
> don't recommend this route. For example, `numpy.ndarray` is used internally
> by Numpy to initialise its arrays, but Numpy recommends using one of the
> higher-level functions like `numpy.zeros`, `numpy.ones`, `numpy.empty`, or
> `numpy.asarray` to construct an array (of zeroes, of ones, without
> initialising the data, and initialising from an existing data structure like
> a list, respectively).
{: .callout}

> ## Make a dict
>
> Given the following list of students and their grades, how would you
> construct a `dict` with students as keys, and grades as values?
>
> ~~~
> students = ['Petra', 'Aalia', 'Faizan', 'Shona']
> grades = [84, 78, 91, 66]
> ~~~
> {: .language-python}
>
> You can check the type of the object you've created with `isinstance`
>
> Hint: `zip()` can be used to turn two lists into tuples of corresponding
> pairs of elements.
>
>> ## Solution
>>
>> ~~~
>> student_grades = dict(zip(students, grades))
>> isinstance(student_grades, dict)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> True
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

## Equality and identity

Python has two ways of testing whether two objects are the "same". The first
is _equality_, or whether the associated values or contents of the object are
the same.

The second is _identity_, or whether the objects are in fact the
same instance, with names referring to the same underlying object.

Equality is tested with `==`, which you have probably used before. We can test for identity with the `is` keyword:

~~~
old_students = students
new_students = ['Petra', 'Aalia', 'Faizan', 'Shona']

if old_students == students:
    print("old_students is equal to the students list")
if new_students == students:
    print("new_students is equal to the students list")
if old_students is students:
    print("old_students is identical to the students list")
if new_students is students:
    print("new_students is identical to the students list")
~~~
{: .language-python}

Constructing a new list that has the same elements as an existing list
gives a list that is equal, but not identical, to the existing one. This is
true for any class: constructing a new object that is the same as an existing
one will give a result that is equal, but not identical, to the existing one.


## Inheritance

Object-oriented programming allows relationships to be defined between classes or types.
One class may be considered to be a specialisation or _subclass_ of another.
For a real world example, a car could be considered a specialisation or subclass of the class of all vehicles.

This is very frequently seen in the way Python handles exceptions. For example,
if we check what type a `ValueError` is, we see that it is of
`class 'ValueError'`:

~~~
an_error = ValueError("A value must be provided")
print(type(an_error))
if isinstance(an_error, ValueError):
    print("an_error is a ValueError")
~~~
{: .language-python}

~~~
<class 'ValueError'>
an_error is a ValueError
~~~
{: .output}

However, we can also check if it is an `Exception`:

~~~
if isinstance(an_error, Exception):
    print("an_error is an Exception")
~~~
{: .language-python}

~~~
an_error is an Exception
~~~
{: .output}

This is because `ValueError` is a subclass of `Exception`: value errors are a
specific type of exception that can occur, and so should have all the same
logic that is common to all exceptions.

![Tree of Python exception classes](../fig/exception-hierarchy.svg)

One place this can be used is to structure exception handling; for example:

~~~
numerator = 5
denominator = 0

try:
    print(numerator, "divided by", denominator, "is", numerator / denominator)
except ZeroDivisionError:
    print("You can't divide by zero!")
except Exception:
    print("Something else went wrong.")
~~~
{: .language-python}

`ZeroDivisionError` is another subclass of `Exception`. On encountering an
exception, Python checks each `except` in turn to see whether the exception
matches the class being tested for. The more specific `ZeroDivisionError`
catches the specific case of dividing by zero, but the block is skipped for
all other issues, which are then handled by the more general `Exception`.

{% include links.md %}

