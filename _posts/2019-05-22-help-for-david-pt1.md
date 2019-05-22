---
layout: post
published: true
title: Help for David pt1
date: '2019-05-22'
---
## Passing by Value vs Passing by Reference

I'm going to define a simple function that takes two variables, adds `1` to the first, adds `2` to the second, stores the sum of them into a third variable and then prints that out:

```
function AddNumbers(x, y)
{
    x = x + 1;
    y = y + 2;

    z = x + y;
    print ("Total is: " + z);
}
```

The `x` and `y` inside the body of that function are COPIES of the variables we passed in. This is called "passing by value" (which is very similar to "passing by copy").

Now, somewhere else in the code I'm going to use that function. You're going to ask something like "is this code below written in a module, or a function, or what?" The answer is "yes". What you should remember is that all code is executed within a function which lives within a module.

```
// I'm declaring two variables with two different values
someVariable1 = 3;
someVariable2 = 4;

// This calls the function "AddNumbers" above.
// It will print: "Total is: 10"
AddNumbers(someVariable1, someVariable2);
```

But the value of the variables we passed into the function remain unchanged because we passed them "by value":

```
// This will print: "someVariable1=3"
print ("someVariable1=" + someVariable1);

// This will print: "someVariable2=4"
print ("someVariable2=" + someVariable2);
```

<hr/>
Now we will change the function above slightly so that we can pass by REFERENCE. This is still pseudo-code but I've put the "&" there to say "the arguments you pass into this function will be passed by REFERENCE.

Notice, I changed the function's name to `AddNumbers2`.

```
function AddNumbers2(&x, &y)
{
    x = x + 1;
    y = y + 2;
    
    z = x + y;
    print ("Total is: " + z);
}
```

This time the 'x' and 'y' REFERENCES to the original variables we will use as arguments. That means if we modify them, we also modify the original. Think of it like "particle coupling" in quantum mechanics.

Now let's call everything like before:

```
someVariable1 = 3;
someVariable2 = 4;

// This calls the function "AddNumbers2" above.
// It will print: "Total is: 10" just like the
// other version
AddNumbers2(someVariable1, someVariable2);
```

This looks mostly the same as further above. It will even print the same things. What's different though is:

```
// This will print: "someVariable1=4"
print ("someVariable1=" + someVariable1);

// This will print: "someVariable2=6"
print ("someVariable2=" + someVariable2);
```

Notice that the variables we passed into the second function DID get changed because `AddNumbers2` was using REFERENCES.

Also notice that the functions' bodies are identical. All I did was modify the way that they accept arguments. 

Keep in mind that this was all pseudo-code and some languages accept references differently or with different notation, but the idea is generally the same.