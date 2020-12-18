---
title: Passing by Value vs Passing by Reference
date: '2019-05-22'
categories:
  - blog
tags:
  - c++
  - beginner
published: true
---
Note: This was written for a friend of mine who is taking a programming class.
{: .notice--info}

I'm going to define a simple function that takes two variables, adds `1` to the first, adds `2` to the second, stores the sum of them into a third variable and then prints that out. 

Then somewhere else in the code I'm going to use that function. You're going to ask something like "is this code below written in a module, or a function, or what?" The answer is "yes". What you should remember is that **all code is executed within a function which lives within a module**.

All of this code is pseudo-code but is close enough to real languages that it will give you the right idea.

## Passing by Value

Here is the full code which we'll discuss in segments:

```
function AddNumbers(x, y)
{
    x = x + 1;
    y = y + 2;
    
    z = x + y;
    print ("Total is: " + z);
}

someVariable1 = 3;
someVariable2 = 4;

AddNumbers(someVariable1, someVariable2);

print ("someVariable1 = " + someVariable1);
print ("someVariable2 = " + someVariable2);
```

First let's look at where we declare the function:

```
function AddNumbers(x, y)
{
    x = x + 1;
    y = y + 2;

    z = x + y;
    print ("Total is: " + z);
}
```

Let's look at this carefully:

`function AddNumbers(x, y)` - We declare a function and say it accepts two arguments/parameters which we will call `x` and `y`.

`{` - This simply marks the beginning of the function.

`x = x + 1;` - Here we take whatever value is in `x`, add `1` to it and store the new value back into `x`

`y = y + 2;` - Here we take whatever value is in `y`, add `2` to it and store the new value back into `y`

`z = x + y;` - This creates a **new variable**, named `z`, which stores the sum of `x` and `y`

`print ("Total is: " + z);` - This will print whatever value we have in `z`.

`}` - This simply marks the end of the function.

What to note here is that `x` and `y` inside this function are **copies** of whatever variables we will pass in. This function accepts arguments that are *passed by value*. 

Here is the code where we actually use the function:


```
someVariable1 = 3;
someVariable2 = 4;

AddNumbers(someVariable1, someVariable2);

print ("someVariable1 = " + someVariable1);
print ("someVariable2 = " + someVariable2);
```

First we declare two variables:

```
someVariable1 = 3;
someVariable2 = 4;
```

Then we call our function: 

```
AddNumbers(someVariable1, someVariable2);
```

This calls the function `AddNumbers` above. It will print: `Total is: 10`. Make sure you understand why before moving on.

The value of the variables we passed into the function remain unchanged because we passed them "by value". We can test this by printing them out:

```
print ("someVariable1 = " + someVariable1);
print ("someVariable2 = " + someVariable2);
```

The output will be:
```
someVariable1 = 3
someVariable2 = 4
```


<hr/>

## Passing by Reference

Now we will change the function above slightly so that we can *pass by reference* and show how that is different.

Here is the full code:

```
function AddNumbers2(&x, &y)
{
    x = x + 1;
    y = y + 2;
    
    z = x + y;
    print ("Total is: " + z);
}

someVariable1 = 3;
someVariable2 = 4;

AddNumbers2(someVariable1, someVariable2);

print ("someVariable1 = " + someVariable1);
print ("someVariable2 = " + someVariable2);
```

Notice, I changed the function's name to `AddNumbers2`. 

This is still pseudo-code but I've put a `&` before each parameter to say "this parameter will be passed by reference. The `&` syntax is pretty common but some languages do it differently.

Since the arguments are *passed by reference*, that means if we modify them inside the function then we also modify the original. Think of it like "particle coupling" in quantum mechanics.

Now let's go through the rest of the program again. Much of it will be the same.

```
someVariable1 = 3;
someVariable2 = 4;

AddNumbers2(someVariable1, someVariable2);
```

This calls the function `AddNumbers2` we just defined. It will print: `Total is: 10` just like the other version. However, remember that we modified `x` and `y` inside the function `AddNumbers2` and since we passed in `someVariable1` and `someVariable2` by reference, that means their values were also changed!

```
print ("someVariable1=" + someVariable1);
print ("someVariable2=" + someVariable2);
```

Which will print:

```
someVariable1 = 4;
someVariable2 = 6;
```

The variables we passed into the second function *did* get changed because `AddNumbers2` was using a *reference* for each parameter. Notice that the functions' bodies are identical. All I did was modify the way that they accept arguments. 

Keep in mind that this was all pseudo-code, some languages accept references differently or with different notation. Also, I took a few liberties here that some languages would complain about. However, the general idea is the same and hopefully explains the difference between *pass by value* and *pass by reference*. 
