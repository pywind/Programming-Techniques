# **Longest Increasing Subsequence**
> Find *the longest increasing subsequence (LIS)* by dynamic
1. **Input**
* A list of N integer (1 <= N <= 30000). Find the amount of LIS and print LIS.
* Init: 
    * The first line contains an integer N.
    * The second line contains N numbers describing the range.
* Example: 
    * Input: 
    ```
    10
    9 1 2 4 7 5 2 5 6 8
    ```
    * Output:
    ```
    6
    1 2 4 5 6 8
    ```
> Explanation: The longest increasing subsequence A[1] = 1 < A[2] = 2 < A[3] = 4 < A[5] = 5 < A[8] = 6 < A[9] = 8, and this length is 6
2. **Concept**
* In other words, the LIS of A is a sequence A ![A(i)](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn.png)
* Formula:
> Call F(i) is the longest subsequence of increments ending in A[i], we have the formula
* ![formula](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn%20(1).png)
* With ![dk](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn%20(3).png) and ![dk1](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn%20(4).png)

## Following the original algorithm
> Complexity time is ![complex](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn%20(2).png) 
### Source code C++
``` C++
int longestSubset(int n) {
	// Create flag as the last element
	A[n] = MAX;
	for (int i = 0; i <= n; i++) {
		L[i] = 1;
		for (int j = 0; j < i; j++)
			if (L[j] + 1 > L[i] && A[i] > A[j]) 
                L[i] = L[j] + 1;
	}
	return L[n];  //Return this way, must minus 1.  
    //Because the flag is in the last and appears in the result.
}
```
## **Improved by using Binary Search**
> Concept: 
* We will have array F with the meaning:
F[k] is the ending value of a subsequence increasing the length k (at the time being calculated).
> We will use the same functions lower_bound and BinarySearch in below!
* Declare vector F, with all ![F](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn(5).png)
* Browse from ![N](https://github.com/phuocVu-IT/kyThuatLapTrinh/blob/master/Dynamic/LIS/Formal/CodeCogsEqn(6).png), with every element a[i]
* The lower_bound or BinarySearch function finds the position k is the element F[k] &gt;= a[i]
* Assign F[k] = a[i]
* The longest subset is the last k position that F[k] != MAX
### Source code C++ with lower_bound
> *Read more about [lower_bound](http://www.cplusplus.com/reference/algorithm/lower_bound/)*
```C++
vector<int> F;
int longestSubset(int n) {
    vector<int> F;
    F.resize(n, MAX);
    int result = 0;
    for (int i = 0; i < n; ++i) {
		int k = lower_bound(F.begin(), F.end(), a[i]) - F.begin();
		F[k] = a[i];
		result = max(result, k + 1);
	}
    return result;
}
```    



### Source code C++ with BinarySearch
```C++
vector<int> F;
int BinarySearch(int n, int k) {
	//Filter the first case
	if (k <= F[0]) return 0;
	if (k > F[n - 1]) return n;
	// If not, will do below
	int l = 0, r = n - 1, i = (l + r) / 2;
	while (l != i && i != r) {
		if (F[i] >= k) r = i;
		else l = i;
		i = (l + r) / 2;
	}
	for (i = l; i <= r; i++) {
		if (F[i] >= k) {
			return i;
		}
	}
	return n;
}
int longestSubset(int n) {
	F.resize(n, MAX);
	int result = 0;
	for (int i = 0; i < n; ++i) {
		int k = BinarySearch(F.size(), a[i]);
		F[k] = a[i];
		result = max(result, k + 1);
	}
	return result;
}
```
* **Complexity** 
> The complexity of these two functions is O(N*log N)
3. **Tracing to find LIS**
> Using improved by Binary Search, we have array F and length of LIS is k
* Now tracing from F[0] to F[k] :
```C++
void tracing(int k) {
	for (int i = 0; i < k; ++i) {
		cout << F[i] << " ";
	}
}
```

