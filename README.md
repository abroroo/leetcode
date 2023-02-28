# My Solution to LeetCode and Codewars problems

List is periodically updated

## LeetCode

### #13 Roman Integer

```
var romanToInt = function(s) {
    let hash = {
        'I' : 1,
        'V' : 5,
        'X' : 10,
        'L' : 50,
        'C' : 100,
        'D' : 500,
        'M' : 1000
    }

    let res = 0;

    for (let i = 0; i < s.length; i++) {
        let curr = hash[s[i]]
        let next = hash[s[i + 1]]

        if (curr < next) {
            res += next - curr;
            i++ // only by 1 since loop will also increment by 1, which in total 2
        } else {
            res += curr
        }
    } 
    return res
};
```

### #125 Valid Palindrome

```

var isPalindrome = function(s) {
    let i = 0;
    let j = s.length - 1;

    while(i < j){
        let left = s[i].toLowerCase()
        let right = s[j].toLowerCase()

        if (/\W|_/g.test(left)){
            i++;
        } else if (/\W|_/g.test(right)){
            j--;
        } else if (left == right){
            i++;
            j--;
        } else {
            return false
        }
    }
    return true
};


```
### #1 Two Sum

```
var twoSum = function(nums, target) {
   let hash = {}
   for (let i = 0; i < nums.length; i++){
       if(hash[target - nums[i]] != undefined){
           return [hash[target - nums[i]], i]
       }
       hash[nums[i]] = i;
   }
};

```


 ### #724. Find Pivot Index
 
 ```
 let pivotIndex = function(nums) {
  if (nums.length === 0 ) return 1
  if (nums.length === 1 ) return 0
  
  let sum = nums.reduce((a,b) => a + b);
  let leftSum = 0;
  
  for (let i = 0; i < nums.length; i++){
    if (leftSum === sum - nums[i] - leftSum){
      return i
    }
    leftSum += nums[i]
  }

  return -1
};
 
 ```


 ### #1480. Running Sum of 1d Array
 
 ```
 let runningSum = function(nums) {
  let arr = [nums[0]];
    for(let i = 1; i < nums.length; i++){
        let sum = nums.slice(0, i+1)
        arr.push(sum.reduce((a, b) => a + b))
    }
  return arr
};
 ```
 
 
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

## CodeWars

### #The Hashtag Generator

```
function generateHashtag (str) {
  let res = [];
  if (str.trim() === ''){
    res = false
  } else {
    res = "#" + str.split(' ').filter(w => (w !== '')).map(x => x[0].toUpperCase() + x.replace(x[0], '')).join('')
}
  
  return res.length > 140 ? false : res
}

```

### #Extract the domain name from a URL

```
function domainName(url){
return url.replace('http://', '').replace('https://','').replace('www.','').replace('.com','').split(/[/.?#]/)[0];
}
```

### #Number of trailing zeros of N!

```
function zeros (n) {
  
let numZ = 0;

for (let i = 5; Math.floor(n / i) >= 1; i *= 5){
  numZ += Math.floor(n / i);
}

  return numZ;
}
```

### #Bit Counting

```
def count_bits(n):
  bnum = "{0:b}".format(n)
  return sum(int(digit) for digit in bnum)

```

### #Valid Parentheses

```
function validParentheses(parens){
  let arr = [];
  let model = {
    "(": ")"
  }

for (let i = 0; i < parens.length; i++){
  if(parens[i] === "("){
    arr.push(parens[i]);
  } else {
    let closing = arr.pop();
    if (parens[i] !== model[closing]){
      return false;
    }
  }
}
if (arr.length !== 0){return false;}

 return true;
}
```

### #Moving Zeros To The End

```
var moveZeros = function (arr) {
   let zero = []
  let others = []
  let together = []
  
    for (let i =0; i <= arr.length-1; i++){
      if (arr[i] === 0){
        zero.push(arr[i])
        }
      else{ 
        others.push(arr[i])
      }
    }
  together = others.concat(zero)
  return together
}
```

### #Find the next perfect square!

```
function findNextSquare(sq) {
  if (sq % Math.sqrt(sq) === 0){
    return Math.pow((Math.sqrt(sq) + 1), 2);
  } else {
      return -1;
  }

}
```

### #Binary Addition

```
function addBinary(a,b) {
 let sum = a + b; 
 return (sum).toString(2);
}
```

### #Does my number look big in this?

```
function narcissistic(value) {
  let arrValue = value.toString().split("");
  let newArr = [];

  for(var i=0; i<arrValue.length;i++) arrValue[i] = parseInt(arrValue[i], 10);

  for (let j = 0; j < arrValue.length; j++){
  newArr.push(Math.pow(arrValue[j], arrValue.length));
  }
  if (newArr.reduce((a, b) => a + b, 0) === value){
    return true
  } else {
    return false
  }

}
```

### #Sum of two lowest positive integers

```
function sumTwoSmallestNumbers(numbers) {  
   
  var arr = numbers.sort((a, b) => a - b).slice(0, 2);

  return arr[0] + arr[1];
     
}

```

### #Disemvowel Trolls

```
function disemvowel(str) {
  return str.replace(/[aeiou]/gim, "");
}
```

### #Stop gninnipS My sdroW!

```
function spinWords(str) {
  let arr = str.split(' ');
  
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].length > 4) {
      arr[i] = arr[i].split('').reverse().join('');
    }
  }
  return arr.join(' ');
}
```

### #
