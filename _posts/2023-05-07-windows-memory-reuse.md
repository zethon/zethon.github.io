---
title: Windows can and does reuse memory right away
date: '2023-05-07'
categories:
  - blog
tags:
  - c++
  - windows
published: true
---

# Unit Testing

I have an object that will sometimes delete and immediately reallocate some memory. For example:
    
```cpp
class Object
{
public:
    char* GetNewBuffer()
    {
        delete[] _buffer;
        _buffer = new char[1024];
        return _buffer;
    }

private:
    char* _buffer;
};
```

A reasonable unit test might be:
```cpp
TEST(Object, GetNewBuffer)
{
    Object object;
    char* buffer = object.GetNewBuffer();
    EXPECT_NE(nullptr, buffer);

    char* buffer2 = object.GetNewBuffer();
    EXPECT_NE(nullptr, buffer2);
    EXPECT_NE(buffer, buffer2);
}
```

As a matter of fact, this seems to be so reasonable that CoPilot suggested the final test before I had a chance to type it. However, this test can fail, and in my recent experience, it *can fail often* on Windows!

# Windows Memory Reuse

The class I was writing is more complicated than the example above, so when my unit tests failed I assumed it was a problem with my code. Of course this was an issue where it was only failing on the build machines and locally everything worked just fine. After adding various debug logs to by unit tests, the only explanation I could come up with is that Windows will sometimes immediately reuse memory that was just freed. Hence, the unit test was bad..