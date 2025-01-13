# CPP-CheatSheet
# Complete C++ LeetCode Reference Guide

## Table of Contents
1. [Basic Data Structures](#1-basic-data-structures)
2. [STL Containers](#2-stl-containers)
3. [Common Problem-Solving Patterns](#3-common-problem-solving-patterns)
4. [Advanced Techniques](#4-advanced-techniques)
5. [Time & Space Complexity Guide](#5-time--space-complexity-guide)

## 1. Basic Data Structures

### String Operations
```cpp
string str = "leetcode";

// Basic Operations
str.length();                    // Get length
str.size();                      // Same as length()
str.empty();                     // Check if empty
str += "test";                   // Append string
str.substr(start, length);       // Get substring
str.find("pattern");            // Find first occurrence (-1 if not found)
str.find_first_of("aeiou");     // Find first vowel
str.find_last_of("aeiou");      // Find last vowel
str.replace(pos, len, "new");   // Replace substring

// Conversions
stoi(str);                      // String to integer
stol(str);                      // String to long
stoll(str);                     // String to long long
to_string(42);                  // Number to string

// Character Operations
isalpha(c);                     // Check if letter
isdigit(c);                     // Check if digit
isalnum(c);                     // Check if alphanumeric
islower(c);                     // Check if lowercase
isupper(c);                     // Check if uppercase
tolower(c);                     // Convert to lowercase
toupper(c);                     // Convert to uppercase

// String Stream (for splitting)
istringstream iss(str);
string token;
while(getline(iss, token, ',')) {
    // Process token
}
```

### Arrays/Vectors
```cpp
// Vector Initialization
vector<int> vec;                              // Empty vector
vector<int> vec(5, 0);                        // Size 5, all 0s
vector<int> vec = {1, 2, 3};                  // Initialize with values
vector<vector<int>> matrix(m, vector<int>(n)); // 2D vector

// Basic Operations
vec.push_back(element);        // Add to end
vec.pop_back();               // Remove from end
vec.front();                  // First element
vec.back();                   // Last element
vec.size();                   // Number of elements
vec.empty();                  // Check if empty
vec.clear();                  // Remove all elements
vec.resize(new_size);         // Resize vector

// Vector Algorithms
sort(vec.begin(), vec.end());                          // Sort ascending
sort(vec.begin(), vec.end(), greater<int>());          // Sort descending
reverse(vec.begin(), vec.end());                       // Reverse elements
vec.erase(unique(vec.begin(), vec.end()), vec.end());  // Remove duplicates
rotate(vec.begin(), vec.begin() + k, vec.end());       // Rotate by k

// Binary Search Operations
binary_search(vec.begin(), vec.end(), target);         // Check if exists
lower_bound(vec.begin(), vec.end(), target);           // First >= target
upper_bound(vec.begin(), vec.end(), target);           // First > target
```

### Linked List Structures
```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(NULL) {}
};

// Common Operations
// Reverse List
ListNode* reverseList(ListNode* head) {
    ListNode *prev = NULL, *curr = head;
    while(curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

// Find Middle
ListNode* findMiddle(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while(fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}

// Detect Cycle
bool hasCycle(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while(fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast) return true;
    }
    return false;
}

// Merge Two Sorted Lists
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* tail = &dummy;
    
    while(l1 && l2) {
        if(l1->val <= l2->val) {
            tail->next = l1;
            l1 = l1->next;
        } else {
            tail->next = l2;
            l2 = l2->next;
        }
        tail = tail->next;
    }
    
    tail->next = l1 ? l1 : l2;
    return dummy.next;
}
```

### Tree Structures
```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

// Tree Traversals
// DFS - Inorder (Recursive)
void inorder(TreeNode* root) {
    if(!root) return;
    inorder(root->left);
    // Process root->val
    inorder(root->right);
}

// DFS - Inorder (Iterative)
vector<int> inorderIterative(TreeNode* root) {
    vector<int> result;
    stack<TreeNode*> st;
    TreeNode* curr = root;
    
    while(curr || !st.empty()) {
        while(curr) {
            st.push(curr);
            curr = curr->left;
        }
        curr = st.top(); st.pop();
        result.push_back(curr->val);
        curr = curr->right;
    }
    return result;
}

// BFS - Level Order
vector<vector<int>> levelOrder(TreeNode* root) {
    if(!root) return {};
    vector<vector<int>> result;
    queue<TreeNode*> q;
    q.push(root);
    
    while(!q.empty()) {
        int size = q.size();
        vector<int> level;
        for(int i = 0; i < size; i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        result.push_back(level);
    }
    return result;
}
```

## 2. STL Containers

### Stack
```cpp
stack<int> st;

// Operations
st.push(val);            // Add element
st.pop();               // Remove top
st.top();              // View top
st.empty();            // Check if empty
st.size();             // Get size

// Monotonic Stack Pattern
vector<int> nextGreater(vector<int>& nums) {
    stack<int> st;
    vector<int> result(nums.size(), -1);
    
    for(int i = 0; i < nums.size(); i++) {
        while(!st.empty() && nums[st.top()] < nums[i]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    return result;
}
```

### Queue
```cpp
queue<int> q;
deque<int> dq;      // Double-ended queue

// Queue Operations
q.push(val);         // Add to back
q.pop();            // Remove front
q.front();         // View front
q.back();          // View back
q.empty();         // Check if empty
q.size();          // Get size

// Deque Operations
dq.push_front(val); // Add to front
dq.push_back(val);  // Add to back
dq.pop_front();     // Remove front
dq.pop_back();      // Remove back
dq.front();        // View front
dq.back();         // View back
```

### Priority Queue (Heap)
```cpp
// Max Heap
priority_queue<int> maxHeap;

// Min Heap
priority_queue<int, vector<int>, greater<int>> minHeap;

// Custom Comparator
struct Compare {
    bool operator()(const pair<int,int>& a, const pair<int,int>& b) {
        return a.first > b.first;  // Min heap based on first element
    }
};
priority_queue<pair<int,int>, vector<pair<int,int>>, Compare> pq;

// Operations
pq.push(val);        // Add element
pq.pop();           // Remove top
pq.top();          // View top
pq.empty();        // Check if empty
pq.size();         // Get size
```

### Set and Map
```cpp
// Set
set<int> s;                    // Ordered set
unordered_set<int> us;         // Unordered set

// Set Operations
s.insert(val);                 // Insert element
s.erase(val);                 // Remove element
s.count(val);                 // Check if exists (0 or 1)
s.find(val);                 // Iterator to element
s.lower_bound(val);          // First >= val
s.upper_bound(val);          // First > val

// Map
map<string, int> mp;           // Ordered map
unordered_map<string, int> ump;// Unordered map

// Map Operations
mp[key] = value;              // Insert/Update
mp.insert({key, value});      // Insert only
mp.erase(key);               // Remove key
mp.count(key);               // Check if exists
mp.find(key);               // Iterator to element
```

## 3. Common Problem-Solving Patterns

### Two Pointers
```cpp
// Two Sum
vector<int> twoSum(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while(left < right) {
        int sum = nums[left] + nums[right];
        if(sum == target) return {left, right};
        else if(sum < target) left++;
        else right--;
    }
    return {};
}

// Remove Duplicates
int removeDuplicates(vector<int>& nums) {
    if(nums.empty()) return 0;
    int i = 0;
    for(int j = 1; j < nums.size(); j++) {
        if(nums[j] != nums[i]) {
            nums[++i] = nums[j];
        }
    }
    return i + 1;
}
```

### Sliding Window
```cpp
// Fixed Size Window
vector<double> movingAverage(vector<int>& nums, int k) {
    vector<double> result;
    double sum = 0;
    for(int i = 0; i < k; i++) sum += nums[i];
    result.push_back(sum / k);
    
    for(int i = k; i < nums.size(); i++) {
        sum += nums[i] - nums[i-k];
        result.push_back(sum / k);
    }
    return result;
}

// Variable Size Window
int minSubArrayLen(int target, vector<int>& nums) {
    int left = 0, sum = 0, minLen = INT_MAX;
    
    for(int right = 0; right < nums.size(); right++) {
        sum += nums[right];
        while(sum >= target) {
            minLen = min(minLen, right - left + 1);
            sum -= nums[left++];
        }
    }
    return minLen == INT_MAX ? 0 : minLen;
}
```

### Binary Search
```cpp
// Standard Binary Search
int binarySearch(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target) return mid;
        else if(nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

// Binary Search on Answer
int findMin(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    
    while(left < right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] > nums[right]) left = mid + 1;
        else right = mid;
    }
    return nums[left];
}
```

### Dynamic Programming
```cpp
// 1D DP
int climbStairs(int n) {
    if(n <= 2) return n;
    vector<int> dp(n+1);
    dp[1] = 1;
    dp[2] = 2;
    
    for(int i = 3; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}

// 2D DP
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.length(), n = text2.length();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    
    for(int i = 1; i <= m; i++) {
        for(int j = 1; j <= n; j++) {
            if(text1[i-1] == text2[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[m][n];
}
```

## 4. Advanced Techniques

### Backtracking
```cpp
// Generate Permutations
void backtrack(vector<int>& nums, vector<bool>& used,
              vector<int>& curr, vector<vector<int>>& result) {
    if(curr.size() == nums.size()) {
        result.push_back(curr);
        return;
    }
    
    for(int i = 0; i < nums.size(); i++) {
        if(!used[i]) {
            used[i] = true;
            curr.push_back(nums[i]);
            backtrack(nums, used, curr, result);
            curr.pop_back();
            used[i] = false;
        }
    }
}

// Generate Combinations
void backtrack(int n, int k, int start,
              vector<int>& curr, vector<vector<int>>& result) {
    if(curr.size() == k) {
        result.push_back(curr);
        return;
    }
    
    for(int i = start; i <= n; i++) {
        curr.push_back(i);
        backtrack(n, k, i + 1, curr, result);
        curr.pop_back();
    }
}
```

### Graph Algorithms
```cpp
// DFS
void dfs(vector<vector<int>>& graph, vector<bool>& visited, int node) {
    visited[node] = true;
    for(int next : graph[node]) {
        if(!visited[next]) {
            dfs(graph, visited, next);
        }
    }
}

// BFS
void bfs(vector<vector<int>>& graph, int start) {
    vector<bool>
