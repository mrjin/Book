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

## Special String-Handling Functions
1.
```Haskell
safeHead :: [a] -> Maybe a
safeHead [] = Nothing
safeHead (x:xs) = Just x

safeTail :: [a] -> Maybe [a]
safeTail [] = Nothing
safeTail (x:xs) = Just xs

safeLast :: [a] -> Maybe a
safeLast [] = Nothing
safeLast (x:[]) = Just x
safeLast (x:xs) = safeLast xs

safeInit :: [a] -> Maybe [a]
safeInit [] = Nothing
safeInit (x:xs) = Just (init (x:xs))
```
2.
```haskell
splitWith :: (a -> Bool) -> [a] -> [[a]]
splitWith _ [] = []
splitWith pred (x:xs) | not (pred x) = splitWith pred xs
splitWith pred xs = (takeWhile pred xs):(splitWith pred next)
                    where rest = dropWhile pred xs
                          rev pred x = not (pred x)
                          next = dropWhile (rev pred) rest
```

4.
```haskell
transpose' :: String -> String
transpose' [] = []
transpose' s = concat (map  f  (zip line1 line2))
               where [line1,line2] = lines s
                     f (a,b) = a:b:'\n':[]
```

## Chapter 4
1.
```haskell
asInt_fold :: String -> Int
asInt_fold (x:xs) = if x == '-'
                    then (-1) * (foldl step 0 xs)
                    else foldl step 0 (x:xs)
                    where step zero char = 10 * zero + digitToInt char
asInt_fold [] = 0
```
2.
it can actually cope with these test cases.
3.
it did have the same result as that on the book. But I am not sure whether it
is requiring to add some error sentence in the function.
4.
```haskell
type ErrorMessage = String
data Ei = Left String
        | Right Int
          deriving (Show)

asInt_either' :: String -> Ei
asInt_either' [] = Main.Left "Contains No Digits"
asInt_either' ('-':xs) = case (asInt_either' xs) of
                            Main.Left error -> Main.Left error
                            Main.Right value -> Main.Right ((-1) * value)
asInt_either' xs = foldl step (Main.Right 0) xs
                  where step (Main.Right zero) char | (char >= '0') && (char <= '9') = Main.Right (10 * zero + digitToInt char)
                                               | otherwise = Main.Left ("non-digit '" ++ [char] ++ "'")
                        step (Main.Left error) char = Main.Left error
```
5.
can use `:info concat`
6.
```haskell
concat' :: [[a]] -> [a]
concat' [] = []
concat' xs = foldr (++) [] xs
```
I use foldr to cope with infinite list.
7.
```haskell
takeWhile' :: (a -> Bool) -> [a] -> [a]
takeWhile' cond (x:xs) | cond x = x : takeWhile' cond xs
                       | otherwise = []
takeWhile' _ [] = []

takeWhile'' :: (a -> Bool) -> [a] -> [a]
takeWhile'' cond xs = foldr step [] xs
                      where step a b | cond a = a:b
                                     | otherwise = []
```
`takeWhile'` is the explicit recursive and `takeWhile''` is the fold kind.
8.
if the character and the following character are the same, then they belong to one
group.
9.
```haskell
groupBy' :: (a -> a -> Bool) -> [a] -> [[a]]
groupBy' _ [] = []
groupBy' cond xs = foldl step [] xs
                  where step [] c = [[c]]
                        step x c | cond (head (last x)) c = (init x) ++ [c : (last x)]
                        step x c | otherwise = x ++ [[c]]
```
10.
```haskell
any' :: (a -> Bool) -> [a] -> Bool
any' cond xs = foldr (||) False xs
```
Here is a version of `any` in the form of right fold.Given that the short circuit
exists in Haskell, the right fold seems superior.
I think that all of them can be realized by folding. But the question is whether
they need to. For example, cycle, the infinite loop, which can be easily realized
with explicit recursive.
