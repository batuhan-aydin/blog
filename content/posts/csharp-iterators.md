+++ 
date = "2020-03-16"
title = "Iterators In C#"
slug = "iterators-csharp" 
tags= ["csharp","iterator","ienumerable"]
+++

Iterators are a block of code that uses *yield return* and *yield block*. Iterator methodsa can be used
with these return types, IEnumerable, IEnumarator, IEnumerable<T>, IEnumerator<T>.

``` c#
        static IEnumerable<int> CreateSimpleIterator()
        {
            yield return 10;
            for(var i=0; i<3; i++)
            {
                yield return i;
            }
            yield return 20;
        }
```
In here we create an iterator that return a *IEnumerable* of *int*. If we put it inside a foreach loop, it will return 10, 0, 1, 2, 20.

```c#
foreach(int value in CreateSimpleIterator())
            {
                Console.WriteLine(value);
            }
```

Well, we can just return create a List inside the method, add the number to the List and return it, right? What is the difference? The difference is iterators work lazily. Lazy execution means, execute the code only when you need the value.

```c#
            var enumerable = CreateSimpleIterator();
            using(var enumerator = enumerable.GetEnumerator())
            {
                while(enumerator.MoveNext())
                {
                    Console.WriteLine(enumerator.Current);
                }
            }
```

IEnumerable is a sequence that can be iterated over. IEnumerator is a cursor, it has methods MoveNext and Reset, and a value Current. Their names are pretty explanatory. When we call GetEnumerator we get a IEnumerator so iterate over our sequence. We can think like, IEnumerable is a book, IEnumerator is a bookmark. It makes more sense then. MoveNext just checks and moves the next element and Current returns the current value, i think those parts are also very clean.

MoveNext in here, keep tracks of where you've reached in the code, it is like method stopped right there. When you call the MoveNext again, it keeps executing back then where was it. Thats is what makes it lazy.