---
layout: post
published: true
title: Writing const and non-const Getters
date: '2020-07-19'
---
One use of `const_cast` is to avoid duplicating code when implementing getters in a class. If you want users of your class to have write-access to a member object, then in order to provide write-access and read-only access to the member you might do something like:

```
class Texture
{
    // lots of stuff
}

class Player
{
    Texture _texture;

public:
    
    const Texture& texture() const { return _texture_; }
    Texture& texture() { return _texture; }
};
```

But we would like to avoid the code duplication. 

We have two options. We can implement the const version and do a cast in the non-const version like so:


```
const Texture& texture() const { return _texture; }

Texture& texture() 
{
    return const_cast<Texture&>(static_cast<const Player&>(*this));
}
```

Or we can do the opposite, we can implement the non-const version and cast in the const version. Something like:

```
const Texture& texture() const
{ 
    return (const_cast<Person&>(*this)).texture();
}

Texture& texture() { return _texture; }
```

When I first saw this, my gut reaction was to go with the second option mostly because casting **away** the const felt wrong. However, what I didn't realize is that the second version is calling a non-const function within a const context, which is even worse than casting away const'ness.

Because of that, the first option is preferred.

**Note:** This topic is discussed in Item 3 in *Effective C++, Third Edition* by Scott Meyers