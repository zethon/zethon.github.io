---
layout: post
published: true
title: Passing by Value vs Passing by Reference
date: '2019-05-22'
---
I'm going to define a simple function that takes two variables, adds `1` to the first, adds `2` to the second, stores the sum of them into a third variable and then prints that out. 

Then somewhere else in the code I'm going to use that function. You're going to ask something like "is this code below written in a module, or a function, or what?" The answer is "yes". What you should remember is that **all code is executed within a function which lives within a module**.

All of this code is pseudo-code but is close enough to real languages that it will give you the right idea.

## Passing by Copy

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

What to note here is that `x` and `y` inside this function are **copies** of whatever variables we will pass in. This functions accepts arguments that are *passed by value*. 

```
someVariable1 = 3;
someVariable2 = 4;

AddNumbers(someVariable1, someVariable2);

print ("someVariable1 = " + someVariable1);
print ("someVariable2 = " + someVariable2);
```

I'm going to break this up. First we declare two variables:

```
someVariable1 = 3;
someVariable2 = 4;
```

Then we call our function: 

```
AddNumbers(someVariable1, someVariable2);
```

This calls the function `AddNumbers` above. It will print: `Total is: 10`.

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

## Passing by Value

Now we will change the function above slightly so that we can *pass by reference* and show how that is different.

This is still pseudo-code but I've put the `&` there to say "the arguments you pass into this function will be passed by reference. The `&` syntax is pretty common but some languages do it differently.

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

Notice, I changed the function's name to `AddNumbers2`. Also, this time the `x` and `y` REFERENCES to the original variables we will use as arguments. That means if we modify them, we also modify the original. Think of it like "particle coupling" in quantum mechanics.

Now let's exam the parts again:

```
someVariable1 = 3;
someVariable2 = 4;
```

This is the same. Nothing more to see here:

```
AddNumbers2(someVariable1, someVariable2);
```

This calls the function "AddNumbers2" we just defined.. It will print: `Total is: 10` just like the other version

This looks mostly the same as the first function's output above. What's different though is the output of this:

```
print ("someVariable1=" + someVariable1);
print ("someVariable2=" + someVariable2);
```

Which is:

```
someVariable1 = 4;
someVariable2 = 6;
```

Notice that the variables we passed into the second function DID get changed because `AddNumbers2` was using REFERENCES.

Also notice that the functions' bodies are identical. All I did was modify the way that they accept arguments. 

Keep in mind that this was all pseudo-code and some languages accept references differently or with different notation, but the idea is generally the same.

Another thing to note here is that I used "pass by value" and "pass by reference" with functions. Functions are just small segments of work. Modules are collections of functions. 

<hr/>
<small>Note: This is written for my friend David who is taking a programming class.</small>