# My Solution to LeetCode and Codewars problems

List is periodically updated

## LeetCode

### #16. 3Sum Closest
```
const threeSumClosest = function(nums, target) {       
    nums.sort((a, b) => a - b);

    let closerSum = Infinity;

    // there hould be two nums left for l, r pointers, so in total we have: i, l ,r : to sum up. 
    for (let i = 0; i < nums.length - 2; i++) {
        // if [2,2,4,7,8] second two is skipped , not test 2 again for total sum  
        if (i > 0 && nums[i] === nums[i - 1]) continue;

        //since i started with 0, left is starting with i + 1
        let l = i + 1;
        // right is starting with last num, since i only comes till nums.length - 2
        let r = nums.length - 1

        while (l < r) {
            let currentTotal = nums[i] + nums[l] + nums[r]
            if (currentTotal === target) return currentTotal
                // if for example target = 12, closerSum = 15 and currentTotal 17
                // then 3 < 5 and closerSum stays as it is because it's more closer to 12
            closerSum = Math.abs(target - closerSum) < Math.abs(target - currentTotal) ? closerSum : currentTotal

            // move pointer to right if currentTotal is less than needed else move left 
            if (currentTotal < target) {
                l++;
            } else {
                r--;
            }
        }
    }
    return closerSum
}


```
### #209 Minimum Size Sub Array

```
const minSubArrayLen = function(target, nums) {
    let r = 0;
    let l = 0;
    let minLenSeen = Infinity
    let currentSum = nums[0];

    while (l <= r && r < nums.length) {
        if (currentSum >= target) {
            minLenSeen = Math.min(minLenSeen, r - l + 1);
            currentSum -= nums[l]
            l++;
        } else {
            r++;
            currentSum += nums[r];
        }
    }
    return minLenSeen === Infinity ? 0 : minLenSeen
};

```
### #7 Reverse Integer

Using Math formulas to achieve the result

```
var reverse = function(x) {
    let result = 0;
    // until x is zero
    while (x != 0){
        // add last digit of x derived as(x % 10) to the end of result as (num * 10) + anyNum
        result = (result * 10) + (x % 10)

        // remove the last digit of x from x using formula below
        x = (x - (x % 10))/10
    }

// check if within the range [-2^31, 2^31 -1] before returning result
    if (result <= -Math.pow(2, 31) || result >= Math.pow(2, 31) - 1) return 0
    return result
};
```
By converting to string, less optimal
```
var reverse = function(x) {
    // get the absolute value(non negative) and convert to String
    let absVal = Math.abs(x).toString()
    // reverse and join
    absVal = absVal.split("").reverse().join("")

// check if in range [-2^31, 2^31 -1] else return 0
    return x > 0 ? (absVal < Math.pow(2, 31) -1 ? absVal : 0) : (-absVal > -Math.pow(2, 31) ? -absVal : 0)
};

```

### #5 Longest Palindrome Substring

```
var longestPalindrome = function(s) {
    // it's palindrome if only two chars 
    if (s.length < 2) return s

    let maxLength = 1
    let start = 0

// loop over each char and point low and high left and right respectively until they reach the beginning or end
    for (let i = 0; i < s.length; i++){
        let low = i -1
        let high = i + 1
// shift to left until it points to a different character
        while (low >= 0 && s[low] == s[i]){
            low--;
        }

// move high to right until it points to a different character
        while (high <= s.length && s[high] == s[i]){
            high++;
        }
        // move further left right until they points to diff char
        while (low >= 0 && high <= s.length && s[low] == s[high]){
            low--;
            high++;
        }
// calculate the length of resulted palindrome 
        let length = high - low -1
        // set start and max position of the resulted palindrome
        if (maxLength < length){
            maxLength = length
            start = low + 1 
        }
    }
    // return resulted palindrome 
    return s.substring(start,start + maxLength)
};
```
### #2 Add Two Numbers

```
const addTwoNumbers = function(l1, l2) {
    // new node to store result
    let result = new ListNode(0);
    let tail = result;
    let carry = 0

// while there are nodes in any of them
    while( l1 || l2 || carry){
        let val1 = l1 ? l1.val : 0
        let val2 = l2 ? l2.val : 0
        let sum = val1 + val2 + carry

        // val if sum is 13, val is 3
        let val = sum % 10

        // carry is 1 if sum 13 
        carry = sum >= 10 ? 1 : 0
        
        // append val of 3 to the result
        tail.next = new ListNode(val)

        // poin tail to next node, we need to do so to keep track of last node of the list to be able to append in the end each time
        tail = tail.next

// go to next node for another iteration
        if (l1) l1 = l1.next
        if (l2) l2 = l2.next
    }
    return result.next
};

```

### #3 Longest Substring Without Repeating Characters

```
const lengthOfLongestSubstring = function(s) {
    // set to store not repating chars
    let set = new Set();

    let left = 0;
    let size = 0;

    // save time
    if (s.length == 0 ) return 0
    if (s.length == 1 ) return 1

    for (i = 0; i < s.length; i++){
        // if s[i] exists delete it and everything before it 
        while(set.has(s[i])){
            // delete everything beofre s[i]
            set.delete(s[left])
            left++
        }
        // add new s[i] to set
        set.add(s[i])
        // position of desired substring starts at left and ends at i 
        // since i and left initially stared with 0, we add 1
        size = Math.max(size, i - left + 1) 
    }
    return size

};
```
### #101 Symmetric Tree

```

/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
const isSymmetric = function(root) {
    // tree with null root considered symmetric
    if (root == null) return true
    
    // check if same first two nodes of the root
    return isSame(root.left, root.right)
};

const isSame = (leftN, rightN) => {
    // if both nodes are null then tree symmetric
    if (leftN == null && rightN == null) return true
    
    // if only one of them null, no longer symmetric
    if (leftN == null || rightN == null) return false
    
    // if two values are not equal , not symmetric
    if (leftN.val !== rightN.val) return false
    
    // recursivly check both outer sides of the seconf node and inner sides respectively all over the height of the tree
    return isSame(leftN.left, rightN.right) && isSame(leftN.right, rightN.left);
}

```
### #26 Remove Duplicate From Sorted Array

```

const removeDuplicates = function(nums) {
    // we take first value of array as uniq
    let uniq = 0 
    for (let i = 1; i < nums.length; i++){
        // if next value after uniq is not same number, we found not duplicate number, otherwise skip to the next iteration 
        if (nums[i] !== nums[uniq]){
            // shift uniq to that place 
            uniq++
            // set it to new found not duplicate number, which is at i
            nums[uniq] = nums[i]
        } 
    }
    // nums[uniq] is last value , so we add + 1 to return length of nums
    return uniq + 1
};

```
### #21 Merge Two Sorted Linked Lists
```

var mergeTwoLists = function(list1, list2) {
    if (!list1) return list2
    else if (!list2) return list1
    else if (list1.val <= list2.val){
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
};

```

### #20 Valid Parentheses
```
var isValid = function(s) {
    let stack = [];

    for(let i = 0; i < s.length; i++){
        if(s[i] == '('){
            stack.push(')');
        } else if (s[i] == '{'){
            stack.push('}');
        } else if (s[i] == '['){
            stack.push(']');
        } else if (stack.pop() !== s[i]){
            return false
        }   
    }
    return !stack.length
};

```
### #14 Longest Common Prefix
```

var longestCommonPrefix = function(strs) {
    if (strs.length == 0) return ""

    for (let i = 0; i < strs[0].length; i++){
        if(!(strs.every((str) => str[i] == strs[0][i]))) {
            return strs[0].slice(0, i)
        }
    }
    return strs[0]
};


```

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
