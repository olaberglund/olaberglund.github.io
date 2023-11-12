---
author: Ola
title: WPI - Subsequences of size n
---

<!-- https://stackoverflow.com/a/21288092/13131325 -->

This algorithm is super fast. It utilizes dynamic programming, and I'm going to take a closer look at it!

```haskell
subsequencesOfSize :: Int -> [a] -> [[a]]
subsequencesOfSize n xs =
  let l = length xs
   in if n > l then [] else subsequencesBySize xs !! (l - n)
  where
    subsequencesBySize [] = [[[]]]
    subsequencesBySize (x : xs) =
      let next = subsequencesBySize xs
       in zipWith (++) ([] : next) (map (map (x :)) next ++ [[]])
```

The author, Bergi, said this:

> Not that appending the empty lists is the most beautiful solution, but you can see how I have used `zipWith` with the displaced lists so that the results from next are used twice - once directly in the list of subsequences of length n and once in the list of subsequences of length n+1.

## Dynamic programming

<!-- reference skeppsteds book -->

## The Algorithm

So let's now break up the algorithm into it's constituent parts, going deeper for each step. The front algorithm simply returns an empty array if the requested length for the subsequences are longer than the actual list. That makes sense - you can't produce a subsequence that is longer than the original list (by the definition of subsequence). A definition is:

> A subsequence of a given sequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the order of the remaining elements.

If the requested length is valid, it gets the list of subsequences of the requested size. `subsequencesBySize` produces a finite list of _lists of subsequences_. That is, each element in the list is a list of subsequences of a certain size. The size of the subsequence decreases for each list of subsequences. Take a look at this output to understand:

```haskell
ghci> subsequencesBySize [1,2,3]
[[[1,2,3]],[[2,3],[1,3],[1,2]],[[3],[2],[1]],[[]]]
```

So if we want to find the list that produces subsequences of size `n` we need to use the backwards index `l-n`.

## Benchmarks

## Possible improvements: Array, Vector
