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

### Code
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

But that's not enough. Now suppose we are given a array and a target, 
and we are requested to find the first matched element in the array.
Return 0 if it cannot be found.
```
vector: [1, 2, 3, 4, 4, 5]
target: 4
```
The code is written by others, and I will post the refererence below.
I choose this code beacuse it only changes a little bit about binary search, which will make the code much more understandable.
Basically, the algorithm will use a temp variable to store index of mateched element and keep updating it. And the searching will never stop untill begin > end
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
You can find a similar quesion here [leetcode.34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

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


### Reference
wiki: https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95
BinarySearch: https://blog.csdn.net/qq_21688757/article/details/53907379
