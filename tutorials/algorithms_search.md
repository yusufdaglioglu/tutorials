############################

############################
# ALGORITHM SEARCH
############################

############################

# arama algoritmaları

## İkili Arama Algoritması (or Binary Search Algorithm)
- This algorithm design to work in a sorted array only.
- It based on __parçala fethet (or divide and conquere)__.
- Initially it starts to search from the center of array.
- Lets say we search "S" and the middle element of the array is M. This algorithm does:
  - If S=M then return index of M (element founded)
  - If S<M then return binarySearch(0, index-of-M, S)
  - If S>M then return binarySearch(index-of-M, array-max-index, S)

## doğrusal arama Algoritması (or linear search Algorithm or sıralı arama or Sequential Search)
- On this algorithm it does not matter if the array is sorted or not.
- It compares each element one by one.

# Comparison table

| name   | best | average | worst | worst space |
|--------|------|---------|-------|-------------|
| binary | 1    | log n   | log n | 1           |
| lineer | 1    | n       | n     | 1           |
