# Lab Report 3
Phillip Schiffman
***
# Part 1 - Bugs
## Buggy Code
The Buggy Code I will be analyzing is: 
```
// Changes the input array to be in reversed order
static void reverseInPlace(int[] arr) {
  for (int i = 0; i < arr.length; i++) {
    arr[i] = arr[arr.length - i - 1];
  }
}
```
