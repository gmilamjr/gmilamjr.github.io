---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/primes_image_2.png"
tags: [Python, Primes]
---

In this post we will look into creating a function in Python that will find all Prime numbers below a given value. For example, passing a value of 1000 to the function would return a list of all prime numbers below 1000.

If unsure what is a Prime number this is a natural number that is not a product of two smaller natural numbers and greater than 1. Therefore 7 is a prime number as no other numbers can be multiplied together to get 7 except itself (7) and 1. However, 8 is not a prime number as 2 * 4 would result on 8. Click for more information on [Prime numbers](https://en.wikipedia.org/wiki/Prime_number).

---

To start will need to use a variable that will be used to define the upper limit of numbers that we want to search through. Will use 30, the expected result would be all prime numbers that are equal or less than 30.


```ruby
n = 30
```


Since the smallest Prime number is 2, will want to create a list of numbers between 2 to 30 (upper bound) that will be checking against for prime numbers. As the range logic is not inclusive of the upper range that is set, instead of using *n* will use *n+1*.

Additionally, will be using a set instead of list due that there are certain functions that will allow us to remove non-primes from the set during our search.


```ruby
number_range = set(range(2, n+1))
```

We will use a list to keep track of the prime numbers that are discovered.

```ruby
primes_list = []
```

To iterate through the list and check for primes will need to use a loop; however, before doing so will test the logic for detecting primes manually and then will add the loop once we confirmed all is working as expected.

The set of numbers (number_range) contains all the integers between 2 and 30. We will extract the first number from this set and check if it is a prime. If we determine it is a prime we will add to the primes_list; if not, we do not keep it.

We can use the *pop* to remove an element from a list or set and provide that value to us.

```ruby
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30}
```

If we use pop, and assign this to the object called **prime** it will *pop* the first element from the set out of **number_range**, and into **prime**

```ruby
prime = number_range.pop()
print(prime)
>>> 2
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30}
```

Since this is the very first value of our range we know its going to be a prime as there is nothing smaller than that and therefore no integer that can divide evenly into it. Since its a prime, will add it to the primes list.

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

To determine if any of the remaining numbers on the number_range is a non-prime will use a nifty trick. For the prime number we just found (which so far is 2) will calculated all multiples of that up to the upper bound (in this case being 30). 

Similarly to the number_range will use a set instead of a list as will allows us to leverage special functionality that will be useful for our approach.

```ruby
multiples = set(range(prime*2, n+1, prime))
```

When creating a range the syntax is range(start, stop, step). As the prime number is already stored we will not use this as the starting point but instead will start our range with its first multiple (2 * prime).

For the stopping point of our range - we specify that we want our range to go up to the upper bound (30), so we use n+1 to specify that we want 30 to be included.

Now, the **step** is key here.  We want multiples of our number, so we want to increment in steps *of our* number so we can put in **prime** here

Lets have a look at our list of multiples...

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30}
```

We will use the special set functionality **difference_update** which removes any values from our number range that are multiples of the number we just checked. The reason we're doing this is because if a number is a multiple of anything other than 1 or itself then it is **not a prime number** and can be removed from the list to be checked (number_range)

Before we apply the **difference_update**, let's look at our two sets.

```ruby
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30}
```

**difference_update** works in a way that will update one set to only include the values that are *different* from those in a second set

To use this, we put our initial set and then apply the difference update with our multiples

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29}
```

When we look at our number range now, all values that were also present in the multiples set have been removed as we *know* they were not primes. This greatly reduces the number of numbers that need to be checked. As well, the smallest number on the updated list *is a prime number* as we know nothing smaller than it divides into it and therefore now we can re-run all the logic from the beginning (e.g. loop)

Will use a while loop, as will be iterating over the number_range until is empty and performing the same set of instructions.

Below is the code with the while loop added.

Let's run it for any primes below 1000...

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n+1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = number_range.pop()
    primes_list.append(prime)
    multiples = set(range(prime*2, n+1, prime))
    number_range.difference_update(multiples)
```

Let's print the primes_list to have a look at what we found!

```ruby
print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

With the primes_list we can gather some additionally information such as the number of primes and what is the largest prime with using the upper bound *n*.

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```


The next thing to do would be to put it into a neat function, which you can see below:

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n+1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = number_range.pop()
        primes_list.append(prime)
        multiples = set(range(prime*2, n+1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
```

Now we can jut pass the function the upper bound of our search and it will do the rest!

Let's go for something large, say a million...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```

---

###### Important Note: Using pop() on a Set in Python

Though, here we were coding something just for fun we need to considered the usage of Set and pop(). Sets are unordered and therefore we cannot 100% guarantee that the pop() method will provide as the lowest value. To overcome this we can take two approaches.

One is to to force the minimum value to be used is to replace the line...

```ruby
prime = number_range.pop()
```

...with the lines...

```ruby
prime = min(sorted(number_range))
number_range.remove(prime)
```

where we sort the set and find the lowest number of it, then will remove it.

Another option is to use SortedSet from the [sortedcontainers](https://grantjenks.com/docs/sortedcontainers/) package

The latter option will be faster than the former but still slower than using a non-sorted set.

---

### Testing

When creating new code it is important to add tests for the new functionality added. This is important as projects get larger as it will help identify if the new code added broke existing functionality. By doing so it will avoid issues that may take a lot of time in debugging.

A good framework to use is [pytest](https://docs.pytest.org/en/7.1.x/).

The following is a test to ensure the function is getting all primes including the upper bound.

```ruby
def test_inclusive_range():
    n = 29
    gt_pl = set([2, 3, 5, 7, 11, 13, 17, 19, 23, 29])
    
    pl = set(primes_finder(n))
    
    assert gt_pl == pl
```

If running pytest and no errors have been detected the output should be similar to the following.

```ruby
================================================= test session starts =================================================
platform win32 -- Python 3.9.1, pytest-7.1.2, pluggy-1.0.0
rootdir: C:\Projects\mini_projects
collected 5 items

test_primes_finder.py .....                                                                                      [100%]

================================================= 5 passed in 37.46s ==================================================
```

Now lets take the case someone mistakenly removes the *+1* from the set (number_range).

```ruby

================================================= test session starts =================================================
platform win32 -- Python 3.9.1, pytest-7.1.2, pluggy-1.0.0
rootdir: C:\Projects\mini_projects
collected 5 items

test_primes_finder.py .F...                                                                                      [100%]

====================================================== FAILURES =======================================================
________________________________________________ test_inclusive_range _________________________________________________

    def test_inclusive_range():
        n = 29
        gt_pl = set([2, 3, 5, 7, 11, 13, 17, 19, 23, 29])

        pl = set(primes_finder(n))

>       assert gt_pl == pl
E       assert {2, 3, 5, 7, 11, 13, ...} == {2, 3, 5, 7, 11, 13, ...}
E         Extra items in the left set:
E         29
E         Use -v to get more diff

test_primes_finder.py:26: AssertionError
------------------------------------------------ Captured stdout call -------------------------------------------------
There are 9 prime numbers between 2 and 29, the largest of which is 23
=============================================== short test summary info ===============================================
FAILED test_primes_finder.py::test_inclusive_range - assert {2, 3, 5, 7, 11, 13, ...} == {2, 3, 5, 7, 11, 13, ...}
============================================ 1 failed, 4 passed in 40.56s =============================================

```

In the above we can see that our inclusive_range test failed while the other 4 were successful. Furthermore, it clearly lets us know what had happen. In this case, the ground truth list contained an extra item 29 which is expected but the calculated primes were missing this. This is very useful cause we caught now instead of afterwards and anytime something breaks it will know about it.

Lastly, when using github you could add this testing as part of your workflow. Meaning that after a push to the repo all existing and newly added tests can be executed to ensure everything is functionally working as expected.
