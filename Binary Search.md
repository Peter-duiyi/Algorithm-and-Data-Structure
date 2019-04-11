## Binary Search

### Principles
* binary search can only be used on a sorted array since it is the assumption.
* It's basically divide and conquer: Every time we look at the number that in the middle of the array and compare it with our target,
then move to the left or right part accordingly.
* Time Complexity: O(lgn). Suppose we have a size n problem and we have to divide k times to get 1 element left, 
then we will have this formular below:
```
n * (1/2)^k = 1
log2n = k
```
That means we need to divide log2n(which is k we mentioned before) times untill we get 1 element left.

### Pure Binary Search
notice: _v is a private vector<int> I used in the code below 
```
int binarySearch(int begin, int end, int target) {
		if (begin > end) return -1;  // if cannot find the number

		int mid = begin + (end - begin) / 2;
		if (_v[mid] < target) { // target is at the right side
			return binarySearch(mid + 1, end, target); // return here is necessary
		}
		if (_v[mid] > target) {                  //target is at the left side
			return binarySearch(begin, mid - 1, target);
		}
		if (_v[mid] == target) {
			return mid;
		}
}

```
This is a iterative version of BinarySearch, but I don't like the return type of this function because it always confuses me that where I should add a return.
Therefore, personally, I prefer to write a void function and store the result at a variable.
```
int res = 0;
void binarySearch(int begin, int end, int target) {
		if (begin > end) res = -1;  // if cannot find the number
		int mid = begin + (end - begin) / 2;
		if (_v[mid] < target) { // target is at the right side
			binarySearch(mid + 1, end, target); // return here is necessary
		}
		if (_v[mid] > target) {                  //target is at the left side
			binarySearch(begin, mid - 1, target);
		}
		if (_v[mid] == target) {
			res = mid;
		}
}
```
That looks much clear for me.
And we also have a while loop version: 
```
int bianrySearch(int begin, int end, int target) {
		while (begin <= end) {
			int mid = begin + (end - begin) / 2;
			if (_v[mid] < target) { // target is on the right
				begin = mid + 1;
			}
			if (_v[mid] > target) { // target is on the left
				end = mid - 1;
			}
			if (_v[mid] == target) { // find the number
				return mid;
			}
		}
		return -1; // cannot find 
	}
```
I think I like this one better than the previous one. And I feel free to use return type without worrying about where I should set exits for the program.

Howerver, after doing several problems of binary search on Leetcode, I found there are hundreds of different ways of writing binary search. And the one below is another common way that we will use to write binary search.
Notice: we don't have to change /2 to >> 1 here, it may cause problem either when occurs to negative number or the logical of calculation.
```
int binarySearch(int left, int right, int target) {
		while (left < right) {
			int mid = left + (right - left) / 2;
			if (_v[mid] < target) {
				left = mid + 1;
			}
			if (_v[mid] > target) {
				right = mid;
			}
			if (_v[mid] == target) {
				return mid;
			}
		}
		return -1;
	}

```
Then we can implement the function to get the lowerBound or upperBound, this code also from a video and I will post the link below
A = [1, 2, 2, 2, 4, 4, 5]
        l        u
lower_bound is the first found target number
upper_bound is the number after last target number
lower_bound(A, 2) = 1, lower_bound(A, 3) = 4(does not exist)
upper_bound(A, 2) = 4, lower_bound(A, 5) = 7(does not exist)
in these cases, `we cannot stop once we find the matched number because we are supposed to find the earliest/latest one in the array` and that's why we don't have this line, if(arr[mid] == target), in our code.  
### Lower Bound and Upper Bound
```
int lowerBound(int left, int right, int target) {
		while (left < right) {
			int mid = left + (right - left) / 2;
			if(_v[mid] < target){
				left = mid + 1;
			}else{
				right = mid;
			}
		}
		return left;
	}

int upperBound(int left, int right, int target) {
		while (left < right) {
			int mid = left + (right - left) / 2;
			if(_v[mid] <= target){
				left = mid + 1;
			}else{
				right = mid;
			}
		}
		return left;
	}
```
### Problems
Now, let's take a look at certain problems.

Problem Type 1:  typical binary search. It's the easiest one.  
[374. Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/)
```
class Solution {
public:
    int guessNumber(int n) {
        int left = 0;
        int right = n + 1;
        while(left < right){
            int mid = left + (right - left)/2;
            if(guess(mid) == 1){
                left = mid + 1;
            }
            if(guess(mid) == -1){
                right = mid;
            }
            if(guess(mid) == 0){
                return mid;
            }
        }
        return -1;
    }
};

```
[69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)
```
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 0) return 0;
        int left = 1, right = x;
        while(left <= right){
            long mid = (right - left) / 2 + left;
            if(mid * mid > x){
                right = (int)mid - 1;
            }else if(mid * mid < x){
                left = (int)mid + 1;
            }else if(mid * mid == x){
                return (int)mid;
            }
        }
        
        return right;
    }
};
    
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 0) return 0;
        if(x == 1) return 1;
        int left = 1, right = x;
        while(left < right){
            long mid = (right - left) / 2 + left;
            if(mid * mid > x){
                right = (int)mid;
            }else if(mid * mid < x){
                left = (int)mid + 1;
            }else if(mid * mid == x){
                return (int)mid;
            }
        }
        
        return right - 1;
    }
};
```
Problem Type 2: find the first and last matched number --> get the lower bound  
[278. First Bad Version](https://leetcode.com/problems/first-bad-version/)
```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res;
        int first = lowerBound(nums, target);
        int last = upperBound(nums, target);
        res.push_back(first);
        res.push_back(last);
        return res;
    }
    
    int lowerBound(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
		while (left < right) {
			int mid = left + (right - left) / 2;
			if (nums[mid] < target) {
                left = mid + 1;
			}
			else {
				right = mid;
			}
		}
		if (left < nums.size() && nums[left] == target) return left; // find it
		else return -1;	
	}

	int upperBound(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
		while (left < right) {
			int mid = left + (right - left) / 2;
			if (nums[mid] <= target) {
				left = mid + 1;
			}
			else {
				right = mid;
			}
		}
        int temp = left - 1;
		if (temp < nums.size() && nums[temp] == target) return temp; // find it
		else return -1;
	}
};

```
Problem Type 3:
get the upper bound

Now suppose we are given a array and a target, 
and we are requested to find the first matched element in the array.
Return 0 if it cannot be found.
```
vector: [1, 2, 3, 4, 4, 5]
target: 4
```
The code is written by others, and I will post the refererence below.
I choose this code beacuse it only changes a little bit about binary search, which will make the code much more understandable.
Basically, the algorithm will use a temp variable to store index of mateched element and keep updating it. And the searching will never stop untill begin > end.
```
int binarySearchFindLast(int begin, int end, int target){
        int temp = -1;
        while(begin <= end){
            //base case
            int mid = begin + (end - begin)/2;
            if(_v[mid] < target){ // target is on the right
                begin = mid + 1;
            }
            if(_v[mid] > target){ // target is on the left
                end = mid - 1;
            }
            if(_v[mid] == target){ // find the target
                temp = mid;        //record mached number
                end = mid - 1;     
            }
        }
        return temp;
    }
```
You can find a similar quesion here. [leetcode.34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

After we are done with the problem above. We can take a look at a similar quesion that I just found but it is much easier.
[leetcode.278](https://leetcode.com/problems/first-bad-version/)  
Here I used the same idea with a few modifications.
```
int firstBadVersion(int n) {
        int temp = -1;
        int left = 0;
        int right = n;
        while(left <= right){
            int mid = left + (right - left)/2;
            if(!isBadVersion(mid)){ //bad is on the right
                left = mid + 1;
            }
            if(isBadVersion(mid)){  //bad is on the left or this one
                temp = mid;  // store this index
                right = mid -1;
            }
        }
        return temp;
    }
```
------
One Thing should be noticed is that we are suggested to use start+(end-start)/2 instead of using (start+end)/2 since the last may cause data overflow, if both start and end are closed to INT_MAX(sounds stupid).  
You can see more details [here](https://leetcode.com/problems/first-bad-version/discuss/71311/A-good-warning-to-me-to-use-start%2B(end-start)2-to-avoid-overflow)

One more similar eacy problem, but with confusing description.  
[leetcode.374](https://leetcode.com/problems/guess-number-higher-or-lower/)  
Here is the [confusing part](https://leetcode.com/problems/guess-number-higher-or-lower/discuss/84665/The-key-point-is-to-read-the-problem-carefully.)
```
class Solution {
public:
    int guessNumber(int n) {
        int left = 1;
        int right = n;
        while(left <= right){
            int mid = left + (right-left)/2;
            if(guess(mid) == 1){ //the answer is on the right 
                left = mid + 1;
                cout << "left: " << left << endl;
            }
            if(guess(mid) == -1){ // the answer is on the left
                right = mid - 1;
                cout << "right: " << right << endl;
            }
            if(guess(mid) == 0){ // get the answer
                return mid;
            }
        }
        return -1;
    }
};
```


### Reference
wiki: https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95
BinarySearch: https://blog.csdn.net/qq_21688757/article/details/53907379
BinarySearch: https://www.youtube.com/watch?v=v57lNF2mb_s
