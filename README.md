# My Solution to LeetCode problems

List is periodically updated

 ### #118. Pascal's Triangle
 
```
const generate = function(numRows) {
  let arr = new Array(numRows).fill().map(() => new Array(0));

  for (let i = 0; i < numRows; i++){
      arr[i][0] = 1;
      arr[i][i] = 1;
  
    for(let j=1; j < i; j++){
      arr[i][j] = arr[i-1][j-1] + arr[i-1][j]
    }
  }
   
return arr
    
};
```

### #509. Fibonacci Number
```
const fib = function(n) {
    if (n == 0) return 0
    else if (n == 1 || n == 2) return 1
    else return fib(n-1) + fib(n-2) 
};

```
### Fibonacci for large numbers (memoization)

```
let memo = {}
const fib = function(n) {
    if (n in memo) return memo[n]

    let value;
    if (n == 0) return 0
    else if (n == 1 || n == 2) return 1
    else value = fib(n-1) + fib(n-2) 
    
    memo[n] = value
    return value
};
```
### #128. Longest Consecutive Sequence

```
const longestConsecutive = function(nums) {
   if (nums.length === 0) return 0

  let longest = 0;
  let cur_longest = 1;

  let sorted = nums.sort((a, b) => a - b)

  for (let i = 0; i < sorted.length; i++){
    if (sorted[i] == sorted[i-1]){
      continue;
     
    } else if (sorted[i] == sorted[i-1] + 1){
      cur_longest++;
       
    } else {
      longest = Math.max(longest, cur_longest);
      cur_longest = 1;
      
    }
  }
  return Math.max(longest, cur_longest)
  
}
```
### #1354. Construct Target Array With Multiple Sums
```
const isPossible = function(T) {
  if (T.length === 1 && T[0] !== 1) return false
  let sum = T.reduce((a, b) => a + b)
  T.sort((a, b) => b - a)

  while (sum !== T.length) {
    let m = T[0] - (sum - T[0]) * (Math.trunc(T[0] / (sum - T[0]) - 1) || 1);

    [sum, T[0]] = [sum - T[0] + m, m]
    if (T[0] < 1) return false
    for (let i = 0; T[i] < T[i+1]; i++) [T[i], T[i+1]] = [T[i+1], T[i]]
  }
  
  return true
};
```
### #1657. Determine if Two Strings Are Close

```
const closeStrings = function(word1, word2) {
    let w1 = word1.split('').sort().join("")
    let w2 = word2.split('').sort().join("")
    
    if(w1 === w2) {
        return true
    } else {
        return false
    }
};
```

### #215. Kth Largest Element in an Array

```
const findKthLargest = function(nums, k) {
    let i = nums.length - k
  let sorted = nums.sort((a, b) => a - b)
  console.log(sorted)
    return sorted[i]
};
```

### #5. Longest Palindromic Substring

```
const longestPalindrome = function(s) {
    let rev = s.split('').reverse().join('');
    let arr = [];
    let i = 0;
    let j = s.length - 1;
    
    while ( i < s.length){
        if (s[i] === rev[j]){
            arr.push(s[i])
        } 
      i++;
      j--;
    }
    return arr.join('');
};

```
### #167. Two Sum II - Input Array Is Sorted

```
const twoSum = function(numbers, target) {
  let l = 0; 
  let r = numbers.length - 1;
  
   while (l < r){
     let s = numbers[l] + numbers[r];
     
      if (s === target) {
        return [l+1, r+1]
      } else if ( s < target) {
        l++;
      } else {
        r--;
      }
   }
};
```

### #88.merge sort array subimted answer

```
const merge = function(nums1, m, nums2, n) {
  
nums1.splice(m, nums1.length)
nums2.splice(n, nums2.length)
nums1.push(...nums2)
nums1.sort((a, b) => a - b);
return nums1

};
```
