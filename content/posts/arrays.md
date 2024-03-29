---
title: 'Arrays'
date: 2023-05-07T22:12:26-03:00
draft: false
author: 'torras'
tags: ['dsa', 'data-structures']
---

## Array

### Definition

Array is a fundamental data structure which consists of a collection of elements and all elements are of the same type. Array elements can be accessed directly by using its position/index keeping in mind that arrays are zero-based which means that its position count begins at 0.

Arrays are usually stored contiguously in memory, which just means that all elements are stored sequentially one after the other in memory. This can be very important when considering arrays as data storage due to something called [cache locality](#cache-locality).

Regarding size, arrays may be classified as static or dynamic:

- _Static_: fixed size. Ex.: C and C++ arrays.
- _Dynamic_: memory is allocated on demand, which usually means that when an array reaches its capacity, a new chunk of memory is allocated with (most of the time) double the original size and the original array elements are copied to this new location. Ex.: C++ vectors, JavaScript and Python arrays.

For the next sections below, arrays are going to be represented using C++, which is a strongly typed language. However, the methodology is very similar for other languages.

### Declaration & Initialization

- Declaring an array

_Syntax_:

```c++
element_data_type array_name [ number_of_elements ];
```

_Example 1_:

```c++
int scores[5]; /* array of integers that contains 5 elements */
```

In the example above `scores` is not initialized, which means that we don't know what are the values stored in each of its positions. It is best practice to always initalize arrays to avoid them holding unpredictable data.

- Initializing an array

_Syntax_:

```c++
array_name[element_index] = value;
```

_Example 2_:

```c++
scores[3] = 100; /* assigns 100 to the fourth array position */
```

It is possible to declare and initialize an array in a single line by using an array list. See below.

_Example 3_:

```c++
int scores[5] {100, 98, 99, 85, 90};
```

### Accessing & Modifying

- Accessing array elements

_Syntax_:

```c++
array_name[element_index]
```

_Example 4_ - considering the initialized array of _Example 3_:

```c++
std::cout << scores[2] << std::endl; /* outputs 99 to the console */
```

- Modifying array elements

_Syntax_:

```c++
array_name[element_index] = new_value
```

_Example 5_ - considering the initialized array of _Example 3_:

```c++
scores[0] = 93; /* changes value at the first position of the array to 93*/
scores[1] = 82; /* changes value at the second position of the array to 88*/

std::cout << scores[0] << std::endl; /* outputs 93, previously 100*/
std::cout << scores[1] << std::endl; /* outputs 82, previously 98 */
```

Can you guess what the output would be for the code snippet below?

```c++
std::cout << scores << std::endl;
```

This would output an hexadecimal value representing the address of the first element of `scores`. This is simply due to the fact that the array name represents the location (address) of the first element of the array in memory. We can, then, imagine the indexes inside the square brackets as being simply offsets from that initial position.

### Time Complexity

| Access | Search | Insertion | Deletion |
| :----: | :----: | :-------: | :------: |
|  O(1)  |  O(n)  |   O(n)    |   O(n)   |

### Space Complexity

| O(n) |
| :--: |

### Extra

#### Cache locality

As arrays are stored contiguously in memory, large chunks of them will be loaded into the cache upon first access. This makes it comparatively quick to access future elements of the array. Suppose that we need to loop through an array. The first access will probably require us to go to memory to retrieve the data (a very slow operation in CPU cycles). However, after the first access the rest of the array would hopefully be in the cache, as the processor will cache the memory directly surrounding this location, and subsequent accesses would be much quicker since that when we try to access the next element in the array, we will not have to go to memory again.
