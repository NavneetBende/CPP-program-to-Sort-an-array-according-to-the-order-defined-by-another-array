Sort an array according to the order defined by the other array
Here we will learn about C++ program to Sort an array according to the order defined by another array. We are given two arrays A [] and B [], sort A in such a way that the relative order among the elements will be the same as those are in B. The elements in array A has to be printed accordingly to the sequence of order of elements specified in array B, the rest of the elements, remaining in array A are printed at the end.

Sort an array according to the order defined by another array in c++
While loop in C
Here, we will discuss the following method for sorting the array according to the order that defined by another array.

Method 1 : Using Sorting and Binary searching.
Method 2 : By making customized comparator
Method 3 : Using concept of hashing.
Let’s discuss above methods one by one in brief.

Method 1 :
Declare a temporary array temp of size m and copy the contents of arr1[] to it.
Declare another array visited[] and initialize all entries in it as false. visited[] is used to mark those elements in temp[] which are copied to arr1[].
Sort temp[] using any sorting technique
Declare the output index ind as 0.
Do following for every element of arr2[i] in arr2[]
Binary search for all occurrences of arr2[i] in temp[], if present then copy all occurrences to arr1[ind] and increment ind. Also mark the copied elements visited[]
Copy all unvisited elements from temp[] to arr1[]
Method 1 : Code in C++
#include<bits/stdc++.h>
using namespace std;
 
int first(int arr[], int low, int high, int x, int n)
{
 
    if (high >= low) {
 
        int mid = low + (high - low) / 2;
 
        if ((mid == 0 || x > arr[mid - 1]) && arr[mid] == x)
            return mid;
 
        if (x > arr[mid])
            return first(arr, (mid + 1), high, x, n);
 
        return first(arr, low, (mid - 1), x, n);
    }
 
    return -1;
}
 
void sortAccording(int A1[], int A2[], int m, int n)
{
    int temp[m], visited[m];
    for (int i = 0; i < m; i++) {
        temp[i] = A1[i];
        visited[i] = 0;
    }
    
    sort(temp, temp+m);
    
    int ind = 0;
 
    for (int i = 0; i < n; i++) {
        int f = first(temp, 0, m - 1, A2[i], m);
 
        if (f == -1)
            continue;
 
        for (int j = f; (j < m && temp[j] == A2[i]); j++) {
            A1[ind++] = temp[j];
            visited[j] = 1;
        }
    }
 
    for (int i = 0; i < m; i++)
        if (visited[i] == 0)
            A1[ind++] = temp[i];
}
 
void printArray(int arr[], int n)
{
 
    for (int i = 0; i < n; i++)
        cout<<arr[i]<<" ";
        
    cout<<endl;
}
 
// Driver Code
int main()
{
    int arr1[] = { 20, 1, 20, 5, 7, 1, 9, 39, 6, 18, 18 };
    int arr2[] = { 20, 1, 18, 39 };
    int m = sizeof(arr1) / sizeof(arr1[0]);
    int n = sizeof(arr2) / sizeof(arr2[0]);
 
    sortAccording(arr1, arr2, m, n);
    printArray(arr1, m);
    return 0;
}
Output
20 20 1 1 18 18 39 5 6 7 9
Related Pages
Determine can all numbers of an array be made equal

Finding Minimum sum of absolute difference of given array

Replace each element of the array by its rank in the array

Finding equilibrium index of an array

Rotation of elements of array- left and right 

Method 2 :
In this method we will create our own customized compare method.
Method 2 : Code in C++
#include<bits/stdc++.h>
using namespace std;
 
void sortA1ByA2(vector &arr1 , vector &arr2){
         
    unordered_map index;
 
    for(int i=0; i<arr2.size(); i++){
          if (index[arr2[i]] == 0) {
            index[arr2[i]] = i+1;
        }
    }
         
    auto comp = [&](int a , int b){
        if(index[a] == 0  && index[b]==0) return a<b;
 
        if(index[a] == 0)  return false;
 
        if(index[b] == 0)   return true;
 
          return index[a] < index[b];
         
        };
 
    sort(arr1.begin(), arr1.end() , comp);
         
}
 
int main(){
 
    vector arr1{ 20, 1, 20, 5, 7, 1, 9, 39, 6, 18, 18 };
    vector arr2{ 20, 1, 18, 39};
  
    sortA1ByA2(arr1 , arr2);
 
 
    for(auto i: arr1){
            cout<<i<<" ";
    }
 
    return 0;
 
}
Output
20 20 1 1 18 18 5 6 7 9 39
Method 3 :
Run a loop over the arr1, store the count of every element in the map.
Run a loop over arr2, check if it is present in map, if so, put in output array that many times and remove the number from map.
Sort the rest of the numbers present in map and put in the output array.
Method 3 : Code in C++
#include <bits/stdc++.h>
using namespace std;
 
void sortA1ByA2(int A1[], int N, int A2[], int M, int ans[])
{
    map<int, int> mp;
 
    int ind = 0;
 
    for (int i = 0; i < N; i++) {
        mp[A1[i]] += 1;
    }
 
    for (int i = 0; i < M; i++) {
 
       if (mp[A2[i]] != 0) {
 
            for (int j = 1; j <= mp[A2[i]]; j++)
                ans[ind++] = A2[i];
        }
 
        mp.erase(A2[i]);
    }
 
    for (auto it : mp) {
 
    for (int j = 1; j <= it.second; j++)
            ans[ind++] = it.first;
    }
}
 
// Utility function to print an array
void printArray(int arr[], int n)
{
    // Iterate in the array
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    cout << endl;
}
 
// Driver Code
int main()
{
    int arr1[] = { 20, 1, 20, 5, 7, 1, 9, 39, 6, 18, 18 };
    int arr2[] = { 20, 1, 18, 39 };
    int n = sizeof(arr1) / sizeof(arr1[0]);
    int m = sizeof(arr2) / sizeof(arr2[0]);
 
    // The ans array is used to store the final sorted array
    int ans[n];
    sortA1ByA2(arr1, n, arr2, m, ans);
 
    // Prints the sorted array
    printArray(ans, n);
 
    return 0;
}
Output
20 20 1 1 18 18 39 5 6 7 9
