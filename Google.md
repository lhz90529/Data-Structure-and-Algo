## Google Interview Prep

### Sorting
#### Merge Sort
- Merge Sort uses a ***divide and conquer*** strategy
    - **Divide**: _split_ the list into two or more equal sublists
    - **Conquer:** _merge_ the sorted sub-list
    - Can be done **recursively**
        - Divide the problem into sub-problems until the sub-problems is trivial **(i.e. the sub-problem is already sorted)**

    - **Complexity:**
        - **`log2(N) * N`**
            - **`log2(N)`:** # of divides needed
            - **`N`:** cost of merge

    - **Pros**
        - O(nlogn) for all cases
        - stable sort
    - **Cons**
        - extra space ---> not `in-place`


    - **Code**
        ```c++
        /*  Documentation:
            Divide(i, j, v): split the vector v ranges in [i, j] in half recursively until v is sorted already

            Merge(i, j, k, v): merge two sub-arrays:
                1: ranges in [i, j]
                2: ranges in [j + 1, k]
        */     
        void Merge(size_t start, size_t mid, size_t end, vector<int>& v) {
            
            /*  left subarray [start, mid]
                right subarray [mid + 1, end] */
            size_t left = start;
            size_t right = mid + 1;
            vector<int> temp;
            
            while (left <= mid && right <= end) {
                if (v[left] < v[right]) {
                    temp.push_back(v[left++]);
                } else {
                    temp.push_back(v[right++]);
                }
            }

            /* clean the remaining */
            while (left <= mid) {
                temp.push_back(v[left++]);
            }

            while (right <= end) {
                temp.push_back(v[right++]);
            }
            
            copy(temp.begin(), temp.end(), v.begin() + start);
            return;
        }

        void Divide(size_t start, size_t end, vector<int>& v) {
            if (start >= end) {//the sub-problem is trivial enough
                return;
            } else {
                size_t mid = (start + end) / 2;
                Divide(start, mid, v); Divide(mid + 1, end, v);
                Merge(start, mid, end, v);
            }
        }


        //goal: sort v which is an unsorted list
        Driver(vector<int> v) {
            const size_t n = v.size();
            Divide (0, n - 1, v, ret);
            //the input v is sorted from this point
        }
        ```

#### Quick Sort
- Also uses a ***divide and conquer*** strategy 
    > partitionining the array into two sub­arrays and then calling itself recursively to sort the left sub­array and then the right sub­array
- **Pros:**
    - `in-place` ---> no `extra space`

- **Code**
    ```c++
    int MedianOf3 (int start, int end, vector<int>& v) {
        int mid = (start + end) / 2;
        if (v[start] > v[mid]) {
            swap(v[start], v[mid]);
        }
        if (v[mid] > v[end]) {
            swap(v[mid], v[end]);
        }
        if (v[start] > v[mid]) {
            swap(v[start], v[mid]);
        }
        return mid;
    }

    Partinioning(int start, int end, int pivot_index, vector<int>& v) {
        swap(v[pivot_index], v[end]);
        int pivot = v[end];
        int left = start - 1; int right = end;
        while (true) {
            while (v[++left] <= pivot); 
            while (v[--right] >= pivot);
            if (left >= right) {
                break;
            } else {
                swap(v[left], v[right]);
            }
        }
        swap(v[left], v[end]);
        return left;
    }

    void QuickSort(int start, int end, vector<int>& v) {
        if (start >= end) {
            return;
        } else {
            int pivot_index = MedianOf3(start, end, v);//chose pivot carefully
            pivot_index = Partition(start, end, pivot_index, v);
            QuickSort(start, pivot_index - 1, v);//sort the left part
            QuickSort(pivot_index + 1, end, v);//sort the right part
        }
    }

    //goal: sort v which is an unsorted list
    Driver(vector<int> v) {
        const int n = v.size();
        Divide (0, n - 1, v, ret);
        //the input v is sorted from this point
    }
    ```