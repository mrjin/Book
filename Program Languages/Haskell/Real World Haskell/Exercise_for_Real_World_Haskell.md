# Exercise for Real World Haskell

##　Chapter 1 Getting Started
1. Types
+ 5 + 8, 3 * 5 + 8, 2 + 4, (+) 2 4, succ 6, succ 7
    Num
+ sqrt 16, sin(pi/2)
    Floating
+ truncate pi, round 3.5, round 3.4, floor 3.7, ceiling 3.3
    Integral

2. :show bingings
```
it :: Integral b => b = _
x :: Num a => a = _
```

3.
```Haskell
main = interact wordCount
    where wordCount input = show (length (words input)) ++ "\n"
```

4.
```Haskell
main = interact charCount
    where charCount input = show (length input) ++ "\n"
```
