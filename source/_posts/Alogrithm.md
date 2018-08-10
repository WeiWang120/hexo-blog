---
title: Sorting Algorithm
date: 2018-08-10 18:16:25
tags: algorithm
---

## Classic Sorting Algorithm


Some implementation of classic sorting alogrithm

##### 1. Selection Sort

```java
    // each pass find the smallest number, swap it with the first one Of UNSORTED parts
    // [7,2,4,5,3] -> [2,7,4,5,3] -> [2,3,4,5,7]
    // Time Complexity O(n^2)
    // Space Complexity O(1)
    public void selectionSort(int[] arr) {
        int len = arr.length;
      for (int i = 0; i < len; i++) {
        int iMin = i;
        for (int j = i + 1; j < len; j++) {
          if (arr[j] < arr[iMin]) iMin = j;
        }
        swap(arr, iMin, i);
      }
    }

```

##### 2. Insertion Sort

```java
    // [7,2,4,5,3] -> [2,7,4,5,3] -> [2,4,7,5,3] -> [2,4,5,7,3] -> [2,3,4,5,7]
    // Time Complexity O(n^2)
    // Space Complexity O(1)
    public void insertionSort(int[] arr) {
      int len = arr.length;
      for (int i = 1; i < len; i++) {
        int value = arr[i];
        int holeIndex = i;
        while (holeIndex > 0 && arr[holeIndex - 1] > value) {
          arr[holeIndex] = arr[holeIndex - 1];  // insertion, shift sorted parts one slot right, and move holeIndex one slot left
          holeIndex--;
        }
        arr[holeIndex] = value;
      }
    }
```

##### 3. Bubble Sort

```java
    // each pass bubble up the largest number
    // [7,2,4,5,3] -> [2,4,5,3,7] -> [2,4,3,5,7] -> [2,3,4,5,7]
    // Time Complexity O(n^2)
    // Space Complexity O(1)
    public void bubbleSort(int[] arr) {
        int len = arr.length;
        for (int i = 1; i <= len; i++) {
            int flag = 0; // set a flag to break the loop when already the array is already sorted
            for (int j = 0; j < len - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    swap(arr, j, j + 1);
                    flag = 1;
                }
            }
            if (flag == 0) break;
        }
    }
```

##### 4. Merge Sort

```java
    // Time Complexity O(nlogn)
    // Space Complexity O(n)
    public void mergeSort(int[] arr) {
        int len = arr.length;
        if (len < 2) return;
        int mid = len / 2;
        // divide arr into 2 parts
        int[] left = new int[mid];
        int[] right = new int[len - mid];
        for (int i = 0; i < mid; i++) left[i] = arr[i];
        for (int i = mid; i < len; i++) right[i - mid] = arr[i];
        mergeSort(left);
        mergeSort(right);
        // now left and right are sorted, then merge them
        merge(arr, left, right);
    }
    
    public void merge(int[] sorted, int[] arr1, int[] arr2) {
        int len1 = arr1.length;
        int len2 = arr2.length;
        int i = 0, j = 0, k = 0;
        while (i < len1 && j < len2) {
            if (arr1[i] < arr2[j]) {
                sorted[k] = arr1[i++];
            } else {
                sorted[k] = arr2[j++];
            }
            k++;
        }
        
        while (i < len1) sorted[k++] = arr1[i++];
        while (j < len2) sorted[k++] = arr2[j++];
    }
```

##### 5. Quick Sort

```java
    // Time Complexity O(nlogn)
    // worst case time complexity can be O(n^2) without randomly choose pivot
    // Space Complexity O(1)
    public void quickSort(int[] arr, int start, int end) {
        if (start >= end) return;
        int pIndex = randomPartition(arr, start, end);
        quickSort(arr, start, pIndex - 1);
        quickSort(arr, pIndex + 1, end);
    }
    
    // choose end as pivot and place all numbers less than pivot at left
    // place all numbers larger than pivot at right
    // [7, 2, 4, 5, 3]
    // 3 is the pivot, after one partition, array becomes [2, 3, 4, 5, 7]
    public int partition(int[] arr, int start, int end) {
        int pivot = arr[end];
        int pIndex = start;
        for (int i = start; i < end; i++) {
            if (arr[i] < pivot) swap(arr, i, pIndex++);
        }
        swap(arr, pIndex, end);
        return pIndex;
    }
    
    // now in case the array is given in descending order, which is the worst case
    // in this case, the time complexity will be O(n^2)
    // to prevent that, choose the pivot randomly, and swap it with the last number
    // say start = 2, end = 5, 5 - 2 + 1 = 4, Math.random => (0,4) => int (0, 4) => (0, 3) + 2 => (2, 5)
    public int randomPartition(int[] arr, int start, int end) {
        int random = start + (int)(Math.random() * ((end - start) + 1));
        swap(arr, end, random);
        return partition(arr, start, end);
    }
```
