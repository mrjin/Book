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

## Using Composite Data Types
False :: Bool
(["foo", "bar"], 'a') :: ([[Char]], Char)
[(True, []), (False, [['a']])] :: [(Bool, [[Char]])]

## The Type of a Function of More Than One Argument
1. it can return an element which is the same type of any element in the list,
it can not change that. The function can only word with list rather than other
structures.
2. function `lastButOne`
```
lastButOne :: [a] -> a
lastButOne [] = error "Can't call lastButOne on a empty list"
lastButOne (x:[]) = error "Can't call lastButOne on a list with one element"
lastButOne (x:xs) = if length xs == 1 then x else lastButOne xs
```
3. it will raise an exception.

## Recursive Types
1.
```Haskell
data List a = Cons a (List a)
            | Nil
              deriving (Show)

value (Cons x _) = x
tailList (Cons _ x) = x

fromList :: List a -> [a]
fromList Nil = []
fromList x = value x : fromList (tailList x)
```
2.
```Haskell
data NewTree a = NewNode a (Maybe a) (Maybe a)
                 deriving (Show)
```

## Chapter 3 Defining Types, Streamlining, Functions
1.
```
myLength :: [a] -> Int
myLength [] = 0
myLength (_:xs) = 1 + myLength xs
```
2. there is a type signature in <1>

3.
Actually, here `genericLength` can be used in replace of `length`.
```Haskell
import Data.List

average :: (Real a, Fractional b) => [a] -> b
average xs = realToFrac (sum xs) / genericLength xs
```
4.
```Haskell
palin :: [a] -> [a]
palin [] = []
palin (x:xs) = x : (palin xs) ++ [x]
```
5.
```Haskell
isPalin :: (Eq a) => [a] -> Bool
isPalin xs = reverse xs == xs
```
6.
a solution using `sortBy` by realizing the function
```Haskell
lengthCompare :: [a] -> [a] -> Ordering
lengthCompare a b = if (length a) < (length b)
                    then LT
                    else GT
```
then, you can call it by
```Haskell
sortBy lengthCompare <list to sort>
```
7.
```Haskell
myIntersperse :: a -> [[a]] -> [a]
myIntersperse _ [] = []
myIntersperse _ (x:[]) = x
myIntersperse c (x:xs) = x ++ [c] ++ (myIntersperse c xs)
```
8.
the same code as <7>
9.
```Haskell
-- definition of the tree
data Tree a = Node a (Tree a) (Tree a)
            | Empty
              deriving (Show, Eq)

left (Node _ x _) = x
right (Node _ _ x) = x
node (Node x _ _) = x

treeDepth :: Tree t -> Int
treeDepth Empty = 0
treeDepth x = max (treeDepth (left x)) (treeDepth (right x)) + 1
```
10.
```haskell
data Direction = Left
               | Right
               | Straight
                 deriving (Show, Eq)
```
11.
```haskell
data Point = Point {
                x :: Float,
                y :: Float
            } deriving (Show)

direction :: Point -> Point -> Point -> Direction
direction a b c = if cross > 0
                  then Main.Left
                  else if cross < 0
                       then Main.Right
                       else Straight
                  where cross = a1 * b2 - a2 * b1
                        a1 = x b - x a
                        b1 = y b - y a
                        a2 = x c - x b
                        b2 = y c - y b
```
12.
```haskell
listDirection :: [Point] -> [Direction]
listDirection [] = []
listDirection (x:[]) = []
listDirection (x:y:[]) = []
listDirection (x:y:z:xs) = direction x y z : (listDirection (y:z:xs))
```
13.
