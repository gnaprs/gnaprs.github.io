---
layout: post
title: Advent of Code 2020!
date: 2020-12-01
excerpt: "An old engineer attempts to scrub off the rust on her high school coding days."
tag: 
- coding
- fun
comments: false
---

Advent of Code is an Advent calendar of small programming puzzles for a variety of skill sets and skill levels that can be solved in any programming language you like. People use them as a speed contest, interview prep, company training, university coursework, practice problems, or to challenge each other.
{: .notice}

So, a friend of mine recently introduced me to the Advent of Code challenge. It seemed pretty cool, and I thought - why not? Besides, it's been quite a while since I practiced some problem solving with code! And of course, it also helps that the organizers themselves have a <a href="https://whimsical.com/advent-of-code-2020-get-unstuck-with-whimsical-7hoTGmwqttswvigWHqAgpU">whimsical</a> sense of humour. 

So, here goes a record of my attempts at solving our little daily puzzles!

# Day 1
<a href="https://adventofcode.com/2020/day/1"><b>Report Repair</b></a>

We begin with a little story about Santa taking a break before Christmas (gotta clear those annual leave days with a little downtime!), and it appears that before he can leave the Elves need some accounting help. The problem is simple enough: they need you to <b>find the two entries that sum to `2020`</b> and then multiply those two numbers together.

## Part 1
<center>Tl;dr Find the two entries that sum to 2020; and find their product.</center>
{: .notice}

Simple enough - we can approach this using a quick and dirty brute force method: calculate the sum of every possible pair, and the moment we hit our sum, we spit out our answers.

~~~ python
import pandas as pd

df1 = pd.read_csv("input.txt", header=None)
arr=df1.values
~~~

The main function itself is pretty simple, just two nested for loops:
~~~ python
def getPairs(arr, sum): 
    n = len(arr)

    # Cycle through all pairs and check sum 
    for i in range(0, n): 
        for j in range(i + 1, n): 
            if arr[i] + arr[j] == sum: 
                ans1=arr[i]
                ans2=arr[j]
                print(arr[i],arr[j])

    return ans1,ans2

ans1,ans2=getPairs(arr,2020)
print(ans1*ans2)
~~~

    [1124] [896]
    [1007104]
    
And there's our answer! Ans: `1007104`

## Part 2
*The Elves in accounting are thankful for your help; one of them even offers you a starfish coin they had left over from a past vacation. They offer you a second one if you can find three numbers in your expense report that meet the same criteria.*
<center>Tl;dr <b>Baited by the Elves for more shinies</b>. Now we find a triplet which sum to a certain number, and find their product.</center>
{: .notice}

Here, we find ourselves in a little bit of a rut. (Well, technically not really, since we don't have any constraints on our time complexity or space complexity. But I mean, we're trying to do things *somewhat* optimized here)

It turns out that our little brute force method from before wouldn't be quite as efficient in finding a triplet. Blindly running through every single combination for a triplet with **three** nested for loops is a \\(O(n^3)\\) solution - still alright for our little 200 element sized input, but not great if the Elves suddenly unearthed 5 more piles of accounting tables.

So, a little bit of thinking later (and some googling, not gonna lie), I chanced upon the <a href="https://www.geeksforgeeks.org/two-pointers-technique/">Two Pointer Technique</a>. The concept is simple: Given a sorted array `A` (sorted in ascending order), having `N` integers, find if there exists any pair of elements (`A[i]`, `A[j]`) such that their sum is equal to `X`. This is a lot smarter, because by finding smart pairs (i.e. comparing the sum we're getting to what we want), we can reduce the time complexity of our solution down to \\(O(n^2)+O(n \log n)\approx O(n^2)\\). Yay for optimization!

The algorithm is then pretty simple:
1. First, we hold out one of the triplets, finding our desired sum of the pair.
2. We use the two pointer technique to efficiently determine whether the sum can be reached.
3. If not, skip and move on to hold out the next element of the array.
4. Repeat till our triplet is golden :)

```python
import numpy as np

arr1=np.ndarray.flatten(arr)
arr1.sort()
arr1
```

    array([  24,  465,  539,  812,  896, 1124, 1149, 1207, 1215, 1217, 1222,
           1224, 1225, 1232, 1234, 1237, 1240, 1241, 1252, 1270, 1271, 1279,
           1280, 1284, 1299, 1301, 1304, 1317, 1329, 1330, 1331, 1338, 1343,
           1360, 1365, 1369, 1371, 1372, 1376, 1379, 1381, 1383, 1384, 1386,
           1388, 1389, 1394, 1395, 1397, 1400, 1410, 1417, 1419, 1422, 1427,
           1429, 1434, 1435, 1441, 1442, 1443, 1453, 1457, 1458, 1462, 1469,
           1470, 1473, 1484, 1487, 1490, 1494, 1502, 1505, 1510, 1515, 1524,
           1528, 1536, 1539, 1541, 1544, 1546, 1548, 1551, 1554, 1562, 1566,
           1572, 1582, 1585, 1586, 1589, 1592, 1593, 1597, 1602, 1610, 1615,
           1616, 1619, 1625, 1626, 1629, 1630, 1631, 1632, 1637, 1638, 1647,
           1652, 1658, 1675, 1676, 1688, 1691, 1692, 1697, 1700, 1705, 1707,
           1710, 1711, 1712, 1713, 1720, 1721, 1723, 1736, 1737, 1739, 1741,
           1744, 1745, 1748, 1751, 1752, 1753, 1754, 1758, 1769, 1778, 1780,
           1788, 1792, 1794, 1795, 1800, 1804, 1805, 1815, 1817, 1821, 1823,
           1825, 1829, 1836, 1856, 1857, 1868, 1883, 1884, 1885, 1888, 1892,
           1896, 1902, 1908, 1911, 1915, 1922, 1932, 1933, 1934, 1941, 1948,
           1950, 1955, 1956, 1958, 1963, 1966, 1968, 1974, 1975, 1976, 1977,
           1979, 1980, 1982, 1983, 1988, 1994, 2001, 2003, 2005, 2006, 2008,
           2009, 2010], dtype=int64)


```python
def getTriplet(arr, sum): 
    n = len(arr)
    
    arr.sort() # sort from lowest to highest, fix first element
    
    for i in range(0, n-2): 
        l = i + 1 # left is one after locked value
          
        r = n-1 # right is end of array
        while (l < r): 
            if( arr[i] + arr[l] + arr[r] == sum): 
                print(arr[i], arr[l], arr[r])
                return arr[i], arr[l], arr[r]

            # if less than sum, move to a larger number (left move)
            elif (arr[i] + arr[l] + arr[r] < sum): 
                l += 1
            # if more than sum, move to a smaller number (right move)
            else: # A[i] + A[l] + A[r] > sum 
                r -= 1
  
    # rip no triplet shrugs
    return 0,0,0

ans1,ans2,ans3=getTriplet(arr1,2020)
print(ans1*ans2*ans3)
```

    24 539 1457
    18847752

And there we have it - our golden triplet, `24 539 1457`! Ans: `18847752`

> Thoughts after day 1: given that it's the first day, the problems weren't too difficult - nonetheless, it's a good start to the month, and my problem solving engines have been revved up. Onwards we go!
