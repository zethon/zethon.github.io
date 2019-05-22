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

This calls the function "AddNumbers" above. It will print: `Total is: 10`.

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
Now we will change the function above slightly so that we can pass by REFERENCE. This is still pseudo-code but I've put the "&" there to say "the arguments you pass into this function will be passed by REFERENCE.

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

AddNumbers(someVariable1, someVariable2);

print ("someVariable1 = " + someVariable1);
print ("someVariable2 = " + someVariable2);
```

Notice, I changed the function's name to `AddNumbers2`. Also, this time the 'x' and 'y' REFERENCES to the original variables we will use as arguments. That means if we modify them, we also modify the original. Think of it like "particle coupling" in quantum mechanics.

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