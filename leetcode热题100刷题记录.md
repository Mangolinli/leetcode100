## 1. [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)
+ 题目：
> 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
   每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
+ 理解
	+ 采用递归函数+哈希表的方法
	+ 如果n是1就直接返回1，只有一种可能。
	+ 如果n是2就直接返回2，有俩种可能`[1,1],[2]`。
	+ 用一个全局的哈希表记录已经计算的n的可能数，所以先判断哈希表有没有，有就直接返回。
	+ 没有，再计算n-1和n-2的可能数相加，记录，在返回！
+ 题解
```cpp
class Solution {
public:
    map<int,int> storemap;
    int climbStairs(int n) {
        if(n==1)return 1;
        if(n==2)return 2;
        if(storemap[n] != 0){
            return storemap[n];
        }else{
            int result = climbStairs(n-1)+climbStairs(n-2);
            storemap[n] = result;
            return result;
        }
    }
};
```
## 2. [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)
+ 题目
>**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：
>F(0) = 0，F(1) = 1
>F(n) = F(n - 1) + F(n - 2)，其中 n > 1
>给定 `n` ，请计算 `F(n)` 。

+ 理解
	+ 题目简单可以使用递归函数，思路和上面爬楼梯一样。
	+ 也可以使用数学公式和贪心：直接使用哈希表记录所有可能。
+ 题解1
```cpp
class Solution {
public:
    int fib(int n) {
        if(n==0)return 0;
        if(n==1)return 1;
        return fib(n-1)+fib(n-2);
    }
};
```

+ 题解2
```cpp
class Solution {
public:
    int fib(int n) {
        double sqrt5 = sqrt(5);
        double fibN = pow((1 + sqrt5) / 2, n) - pow((1 - sqrt5) / 2, n);
        return round(fibN / sqrt5);
    }
};
```
+ 题解3
```cpp
class Solution { 
public: 
	int fib(int n) { static std::unordered_map<int, int> fib_map = {
		 {0, 0}, {1, 1}, {2, 1}, {3, 2}, {4, 3}, {5, 5}, {6, 8}, {7, 13}, {8, 21}, {9, 34}, {10, 55}, {11, 89}, {12, 144}, {13, 233}, {14, 377}, {15, 610}, {16, 987}, {17, 1597}, {18, 2584}, {19, 4181}, {20, 6765}, {21, 10946}, {22, 17711}, {23, 28657}, {24, 46368}, {25, 75025}, {26, 121393}, {27, 196418}, {28, 317811}, {29, 514229}, {30, 832040} 
		 };
		  return fib_map[n]; 
	} 
};
```

## 3. [1. 两数之和](https://leetcode.cn/problems/two-sum/)
+ 题目
>给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** _`target`_  的那 **两个** 整数，并返回它们的数组下标。
  你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。
  你可以按任意顺序返回答案。
+ 思路
	+ 使用哈希表和与目标值的差值来寻找。
+ 题解：
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int,int> storehash;
        int n = nums.size();
        for(int i = 0; i < n;i++){
            int temp = target-nums[i];
            if(storehash[temp]!=0){
                return {storehash[temp]-1,i};
            }
            storehash[nums[i]]=i+1;
        }
        return {-1,-1};
    }
};
```
## 4.  [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)
+ 题目：
	>给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。
	请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。
	**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。
+ 思路
	+ 第一种将俩个数组直接合拼之后使用内置函数sort直接排序。
	+ 第二种就是创建一个新的数组俩存储结果，在使用俩个指针遍历比较大小，插入到新数组中，要注意的时可能一方全部插入另外一方后面还有很多需要，遍历插入。
+ 题解1
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for (int i = 0; i != n; ++i) {
            nums1[m + i] = nums2[i];
        }
        sort(nums1.begin(), nums1.end());
    }
};
```
+ 题解2
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = 0, j = 0, counter = 0;
        int temp[m+n];
        while(i != m && j != n){
            if(nums1[i] < nums2[j]){
                temp[counter] = nums1[i++];
            }else{
                temp[counter] = nums2[j++]; 
            }
            counter++;
        }
        while(i != m){
            temp[counter] = nums1[i++];
            counter++;
        }
        while(j != n){
            temp[counter] = nums2[j++];
            counter++;
        }
        for(int z = 0; z < counter; z++){
            nums1[z] = temp[z];
        }
    }
};
```

+ 题解2
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = 0, p2 = 0;
        int sorted[m + n];
        int cur;
        while (p1 < m || p2 < n) {
            if (p1 == m) {
                cur = nums2[p2++];
            } else if (p2 == n) {
                cur = nums1[p1++];
            } else if (nums1[p1] < nums2[p2]) {
                cur = nums1[p1++];
            } else {
                cur = nums2[p2++];
            }
            sorted[p1 + p2 - 1] = cur;
        }
        for (int i = 0; i != m + n; ++i) {
            nums1[i] = sorted[i];
        }
    }
};
```

## 5. [283. 移动零](https://leetcode.cn/problems/move-zeroes/)
+ 题目：
>	给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

+ 思路
	+ 使用快慢双指针
	+ 就是使用一个慢指针用来遍历非零数字已填好的下标
	+ 用一个快指针遍历整个数组，遇到非零数就传给慢指针
	+ 最后需要将慢指针继续遍历填写0，数组结束。
+ 题解：
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
          int n = nums.size();
          int l = 0;
          for(int i = 0; i<n;i++){
              if(nums[i] != 0){
                  nums[l++] = nums[i]; 
              }
          }
          while(l != n){
              nums[l++]=0;
          }
    }
};
```
## 6. [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)
+ 题目：
> 	给你一个含 `n` 个整数的数组 `nums` ，其中 `nums[i]` 在区间 `[1, n]` 内。请你找出所有在 `[1, n]` 范围内但没有出现在 `nums` 中的数字，并以数组的形式返回结果。

+ 思路
	+ 三种种办法
	+ 第一种：遍历数组，将数值减一后的下标值所对应的值变成它对应的负数
		+ 然后遍历整个数组不是负数值得下标值加一就是缺失值。
		+ 但是要注意每次减一之前需要将变成负数得值，改成正数减一，但是数组值还是负数，操作数必须时正数。
	+ 第二种：遍历数组，将数值减一后的下标值所对应的值变成它对应的`+n`
		+ 然后遍历整个数组是否<n,小于就是缺失值。
		+ 但是需要注意未遍历数值，可能已经+n需要取n模，用原数来求下标。
	+ 第三种就是用一个数组记录是否出现过，最后遍历没有出现过得就好。
+ 题解1
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
          int n = nums.size();
          vector<int> res;
          for(int i = 0; i <n;i++){
              int temp = abs(nums[i])-1;
              if(nums[temp] > 0){
                  nums[temp]  = -nums[temp];
              }
          }
          for(int i = 0; i <n;i++){
              if(nums[i] > 0){
                  res.push_back(i+1);
              }
          }
          return res;
    }
};
```
+ 题解2
 ```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        for (auto& num : nums) {
            int x = (num - 1) % n;
            nums[x] += n;
        }
        vector<int> ret;
        for (int i = 0; i < n; i++) {
            if (nums[i] <= n) {
                ret.push_back(i + 1);
            }
        }
        return ret;
    }
};
 ```
+ 题解3
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
          int n = nums.size();
          int counter[n+1];
          for(int i = 0; i < n+1; i++){
              counter[i] = 0;
          }
          for(int i = 0; i < n; i++){
              counter[nums[i]]++;
          }
          vector<int> res;
          for(int i = 1; i <n+1;i++){
              if(counter[i]==0){
                  res.push_back(i);
              }
          }
          return res;
    }
};
```
## 7. [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)
+ 题目：
> 	将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

+ 思路
	+ 使用递归函数
+ 题解
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {   
          if(list1 == nullptr) return list2;
          if(list2 == nullptr) return list1; 
          if(list1->val<list2->val){
              list1->next =  mergeTwoLists(list1->next,list2);
              return list1;
          }
             list2->next = mergeTwoLists(list1, list2->next);
          return list2;
    }
};
```
## 8. [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)
+ 题目：
> 	给定一个已排序的链表的头 `head` ， _删除所有重复的元素，使每个元素只出现一次_ 。返回 _已排序的链表_ 。

+ 思路
	+ 直接遍历
	+ 使用递归函数
+ 题解1
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr){
            return head;
        }
        ListNode* currentNode = head; 
        while(currentNode->next != nullptr){
            if(currentNode->val == currentNode->next->val){
                currentNode->next = currentNode->next->next;
                continue;
            }
            currentNode = currentNode->next;
        }
        return head;
    }
};
```
## 9. [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)
+ 题目：
> 	给你一个链表的头节点 `head` ，判断链表中是否有环。
	如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。
	_如果链表中存在环_ ，则返回 `true` 。 否则，返回 `false` 。

+ 思路
	+ 使用快慢指针
		+ 慢指针一次跳一格
		+ 快指针一次跳俩个
		+ 判断是否为空，为空就是无环，判断快慢指针是否相等，相等就是有环。
	+ 也可以使用哈希表
		+ 记录是否出现过
+ 题解1
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == NULL) return false;

        ListNode* slow = head;
        ListNode* fast = head;
        while(fast->next != NULL && fast->next->next != NULL){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast){
                return true;
            }
        }
        return false;
    }
};
```
+ 题解2
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        map<ListNode*,int> mp;
        ListNode* currentNode = head;
        while(currentNode != NULL){
            if(mp.find(currentNode) != mp.end()){
                return true;
            }
            mp[currentNode] = head->val;
            currentNode = currentNode ->next;
        }
        return false;
    }
};
```
## 10.  [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)
+ 题解：
> 	给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 _如果链表无环，则返回 `null`。_
	如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。
	**不允许修改** 链表。
+ 思路
	+ 在上面一题得基础上添加需要找到循环点。
	+ 快慢指针
		+ 在判断时环形链表以后，将慢指针放回起点
		+ 快慢指针同时移动一格，最后会相遇到循环点处。（其中原理大家可以咨询查询和探索）
	+ 哈希表
		+ 同样可以使用哈希表记录已遍历得节点，最后重复节点也可以立刻返回。
+ 题解1
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == NULL) return NULL;

        ListNode* slow = head;
        ListNode* fast = head;
        bool loop = false;
        while(fast->next != NULL && fast->next->next != NULL){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast){
                loop = true;
                break;
            }
        }
        if(loop){
            slow = head;
            while(slow != fast){
                slow = slow->next;
                fast = fast->next;
            }
            return slow;
        }
        return NULL;
    }
};
```
+ 题解2
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == NULL)return NULL;
        map<ListNode*,int> mp;
        while(head != NULL){
            if(mp.find(head) != mp.end()){
                return head;
            }
            mp[head]=head->val;
            head = head->next;
        }
        return NULL;
    }
};
```

## 11.  [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)
+ 题目：
> 	给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。
	图示两个链表在节点 `c1` 开始相交**：**
	[![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
	题目数据 **保证** 整个链式结构中不存在环。
	**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

+ **自定义评测：**
+ **评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：
	- `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0`
	- `listA` - 第一个链表
	- `listB` - 第二个链表
	- `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
	- `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数
	评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

+ 思路
	+ 使用哈希表和俩个指针遍历的方法
	+ 采用双指针，利用a+b=b+a的办法使得最后一定会相同，为null则不相交，不会空则相交。
	+ 先遍历知道链表长度差，先将长的遍历长度差的距离再一起遍历，这样也一定会相同。
+ 题解1：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        map<ListNode*,int> mp;
        while(headA != NULL && headB != NULL){
            if(headA == headB){
                return headA;
            }
            if(mp.find(headA) != mp.end()){
                return headA;
            }
            if(mp.find(headB) != mp.end()){
                return headB;
            }
            mp[headA] = headA->val;
            mp[headB] = headB->val;
            headA = headA->next;
            headB = headB->next;
        }
        while(headA != NULL){
            if(mp.find(headA) != mp.end()){
                return headA;
            }
            headA = headA->next;
        }
        while(headB != NULL){
            if(mp.find(headB) != mp.end()){
                return headB;
            }
            headB = headB->next;
        }
        return NULL;
    }
};
```
+ 题解2
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
       if(headA == NULL || headB == NULL)return NULL;
       ListNode* pa = headA;
       ListNode* pb = headB;
       while(pa != pb){
        pa = pa == NULL ? headB : pa->next;
        pb = pb == NULL ? headA : pb->next;
       }
       return pa;
    }
};
```
+ 题解三
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
       int l1 = 0,l2 = 0,diff = 0;
       ListNode* p1 = headA;
       ListNode* p2 = headB;
       while(p1 != NULL){
        l1++;
        p1 = p1->next;
       }
       while(p2 != NULL){
        l2++;
        p2 = p2->next;
       }
       if(l1 < l2){
            p1 = headB;
            p2 = headA;
            diff = l2-l1;
       }else{
            p1 = headA;
            p2 = headB;
            diff = l1-l2;
       }
       for(int i = 0; i < diff;i++){
            p1 = p1->next;
       }
       while(p1 != NULL && p2 != NULL){
        if(p1 == p2){
            return p1;
        }
        p1 = p1->next;
        p2 = p2->next;
       }
       return NULL;
    }
};
```

## 12.  [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)
+ 题目：
>	给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

+ 思路
	+ 使用头指针和一个后指针，一个前指针，遍历直接反转。
+ 题解
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr|| head->next == nullptr)return head;
        ListNode* currentNode = head->next;
        ListNode* preNode = head;
        head->next = nullptr;
        while(currentNode != nullptr){
            preNode = currentNode;
            currentNode = currentNode->next;
            preNode->next = head;
            head = preNode;
        }
        return head;
    }
};
```
## 13.  [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)
+ 题目
>	给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表

	如果是，返回 `true` ；否则，返回 `false` 。
+ 思路
	+ 思路1：直接创建一个新的列表来存储反转以后的列表，在对比是否相等，反转以后和源列表一样就是回文链表。（当然也可以使用数组代替列表）
	+ 思路2：使用快慢指针找到链表中间位置，将后面的链表反转以后再比较前后是否相等。
+ 题解1：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr)return head;
        ListNode* rerverseNode;
        ListNode* currentNode = head;
        while(currentNode != nullptr){
            ListNode* newNode = new(ListNode);
            newNode->val = currentNode->val;
            newNode->next = rerverseNode;
            rerverseNode = newNode;
            currentNode = currentNode->next;
        }
        while(head != nullptr){
            if(head->val != rerverseNode->val)return false;
            head = head->next;
            rerverseNode = rerverseNode->next;
        }
        return true;
    }
};
```

```cpp
class Solution { 
public: 
	bool isPalindrome(ListNode* head) { 
		if(head == nullptr)return head; 
		ListNode* ptr = head; 
		vector<int> num; 
		while(ptr != nullptr){ 
			num.push_back(ptr->val); 
			ptr = ptr->next; 
		} 
		int n = num.size() - 1;
		while(head != nullptr){ 
			if(head->val != num[n])return false; 
			n--; 
			head = head->next; 
		} 
		return true; 
	} 
};
```
+ 题解2
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // 如果链表为空或只有一个节点，直接返回true
        if (!head || !head->next) return true;

        // 使用快慢指针找到中间节点
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // 反转后半部分链表
        ListNode* prev = nullptr;
        ListNode* curr = slow->next;
        while (curr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }

        // 比较前半部分和反转后的后半部分
        ListNode* firstHalf = head;
        ListNode* secondHalf = prev;
        while (secondHalf) {
            if (firstHalf->val != secondHalf->val) {
                return false;
            }
            firstHalf = firstHalf->next;
            secondHalf = secondHalf->next;
        }

        return true;
    }
};
```

## 14.[876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

+ 题目：

> ​	给你单链表的头结点 `head` ，请你找出并返回链表的中间结点。如果有两个中间结点，则返回第二个中间结点。

+ 思路
  + 使用快慢双指针，但是需要注意判断快指针后一位和俩位是否为空，不用判断慢指针。

+ 题解

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        if(head->next == nullptr){
            return head;
        }
        ListNode* slow = head;
        ListNode* fast = head;
        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

## 15. [LCR 021. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/SLwz0R/)
+ 题目
>		给定一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点.
>		
>	![image-20240927120442381](leetcode%E7%83%AD%E9%A2%98100%E5%88%B7%E9%A2%98%E8%AE%B0%E5%BD%95.assets/image-20240927120442381-17274098846841.png)

+ 思路
  + 解法一就是使用栈，将链表遍历存储到栈里面，然后出栈。
  + 解法二就是遍历链表长度，然后遍历到length-n删除。
  + 俩个相同之处就是：都需要使用new创建一个新的节点，用来解决删除末尾和头节点的问题。
+ 题解1

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        stack<ListNode*> stk;
        ListNode* cur = dummy;
        while (cur) {
            stk.push(cur);
            cur = cur->next;
        }
        for (int i = 0; i < n; ++i) {
            stk.pop();
        }
        ListNode* prev = stk.top();
        prev->next = prev->next->next;
        ListNode* ans = dummy->next;
        delete dummy;
        return ans;
    }
};
```

+ 题解2

```cpp
class Solution {
public:
    int getLength(ListNode* head) {
        int length = 0;
        while (head) {
            ++length;
            head = head->next;
        }
        return length;
    }

    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        int length = getLength(head);
        ListNode* cur = dummy;
        for (int i = 1; i < length - n + 1; ++i) {
            cur = cur->next;
        }
        cur->next = cur->next->next;
        ListNode* ans = dummy->next;
        delete dummy;
        return ans;
    }
};
```

## 16. [面试题 02.02. 返回倒数第 k 个节点](https://leetcode.cn/problems/kth-node-from-end-of-list-lcci/)

+ 题目

> ​	实现一种算法，找出单向链表中倒数第 k 个节点。返回该节点的值。

+ 思路
  + 使用快慢指针，先使用快指针遍历n个节点，然后快慢指针同步遍历到快指针到nullptr。慢指针的位置就是k节点的位置。
+ 题解：

```cpp
class Solution {
public:
    int kthToLast(ListNode* head, int k) {
        ListNode* slow = head;
        ListNode* fast = head;
        while(k != 0){
            fast = fast->next;
            k--;
        }
        while(fast != nullptr){
            if(slow == nullptr){
                slow = head;
            }else{
                slow = slow->next;
            }
            fast = fast->next;
        }
        return slow->val;
    }
};
```

## 17. [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

+ 题目

> ​	请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：
	实现 `MyQueue` 类：
	- `void push(int x)` 将元素 x 推到队列的末尾
	- `int pop()` 从队列的开头移除并返回元素
	- `int peek()` 返回队列开头的元素
	- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`
	**说明：**
	- 你 **只能** 使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
	- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

+ 思路
	+ 将一个栈当作输入栈，用于压入 push 传入的数据；另一个栈当作输出栈，用于 pop 和 peek 操作。
	+ 每次 pop 或 peek 时，若输出栈为空则将输入栈的全部数据依次弹出并压入输出栈，这样输出栈从栈顶往栈底的顺序就是队列从队首往队尾的顺序。
+ 题解
```cpp
class MyQueue {
private:
    stack<int> inStack, outStack;

    void in2out() {
        while (!inStack.empty()) {
            outStack.push(inStack.top());
            inStack.pop();
        }
    }

public:
    MyQueue() {}

    void push(int x) {
        inStack.push(x);
    }

    int pop() {
        if (outStack.empty()) {
            in2out();
        }
        int x = outStack.top();
        outStack.pop();
        return x;
    }

    int peek() {
        if (outStack.empty()) {
            in2out();
        }
        return outStack.top();
    }

    bool empty() {
        return inStack.empty() && outStack.empty();
    }
};

```
## 18. [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)
+ 题目
	+ 给定一个二叉树的根节点 `root` ，返回 _它的 **中序** 遍历_ 。
+ 思路
	+ 递归
	+ 迭代；使用一个栈来存储前面遍历过的节点，用于回溯的时候可以返回即可。
+ 题解1
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        MidTree(root,res);
        return res;
    }

    void MidTree(TreeNode* root , vector<int>& res){
        if(root == nullptr) return;
        MidTree(root->left, res);
        res.push_back(root->val);
        MidTree(root->right, res);
    }
};
```

+ 题解2
```cpp
class Solution {
public: 
	vector<int> inorderTraversal(TreeNode* root) { 
		vector<int> res; 
		stack<TreeNode*> stk; 
		while (root != nullptr || !stk.empty()) { 
			while (root != nullptr) { 
			stk.push(root); 
			root = root->left; 
		} 
		root = stk.top(); 
		stk.pop(); 
		res.push_back(root->val); 
		root = root->right;
		} 
		return res; 
	}
}; 
```
## 19.[144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)
+ 题目
	+ 给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。
+ 思路
	+ 递归
	+ 迭代
	+ 有一种巧妙的方法可以在线性时间内，只占用常数空间来实现前序遍历。这种方法由 J. H. Morris 在 1979 年的论文「Traversing Binary Trees Simply and Cheaply」中首次提出，因此被称为 Morris 遍历。Morris 遍历的核心思想是利用树的大量空闲指针，实现空间开销的极限缩减。其前序遍历规则总结如下：
		+ 新建临时节点，令该节点为 root；
		+ 如果当前节点的左子节点为空，将当前节点加入答案，并遍历当前节点的右子节点；
		+ 如果当前节点的左子节点不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点：
		+ 如果前驱节点的右子节点为空，将前驱节点的右子节点设置为当前节点。然后将当前节点加入答案，并将前驱节点的右子节点更新为当前节点。当前节点更新为当前节点的左子节点。
		+ 如果前驱节点的右子节点为当前节点，将它的右子节点重新设为空。当前节点更新为当前节点的右子节点。
		+ 重复步骤 2 和步骤 3，直到遍历结束。
	+ 这样我们利用 Morris 遍历的方法，前序遍历该二叉树，即可实现线性时间与常数空间的遍历。
+ 题解1
```cpp
class Solution {
public:
    void leftTreeNode(TreeNode* root, vector<int>& res){
        if(root == nullptr)return;
        res.push_back(root->val);
        leftTreeNode(root->left,res);
        leftTreeNode(root->right,res);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        leftTreeNode(root, res);
        return res;
    } 
};
```
+ 题解2
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        stack<TreeNode*> stk;
        TreeNode* node = root;
        while (!stk.empty() || node != nullptr) {
            while (node != nullptr) {
                res.emplace_back(node->val);
                stk.emplace(node);
                node = node->left;
            }
            node = stk.top();
            stk.pop();
            node = node->right;
        }
        return res;
    }
};
```
+ 题解3
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        TreeNode *p1 = root, *p2 = nullptr;

        while (p1 != nullptr) {
            p2 = p1->left;
            if (p2 != nullptr) {
                while (p2->right != nullptr && p2->right != p1) {
                    p2 = p2->right;
                }
                if (p2->right == nullptr) {
                    res.emplace_back(p1->val);
                    p2->right = p1;
                    p1 = p1->left;
                    continue;
                } else {
                    p2->right = nullptr;
                }
            } else {
                res.emplace_back(p1->val);
            }
            p1 = p1->right;
        }
        return res;
    }
};
```
## 20.[145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
+ 题目
	+ 给你一棵二叉树的根节点 `root` ，返回其节点值的 **后序遍历** 。
+ 思路
	+ 递归
	+ 迭代
	+ Morris 遍历
		+ 有一种巧妙的方法可以在线性时间内，只占用常数空间来实现后序遍历。这种方法由 J. H. Morris 在 1979 年的论文「Traversing Binary Trees Simply and Cheaply」中首次提出，因此被称为 Morris 遍历。
		+ Morris 遍历的核心思想是利用树的大量空闲指针，实现空间开销的极限缩减。其后序遍历规则总结如下：
			+ 新建临时节点，令该节点为 root；
			+ 如果当前节点的左子节点为空，则遍历当前节点的右子节点；
			+ 如果当前节点的左子节点不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点；
			+ 如果前驱节点的右子节点为空，将前驱节点的右子节点设置为当前节点，当前节点更新为当前节点的左子节点。
			+ 如果前驱节点的右子节点为当前节点，将它的右子节点重新设为空。倒序输出从当前节点的左子节点到该前驱节点这条路径上的所有节点。当前节点更新为当前节点的右子节点。
			+ 重复步骤 2 和步骤 3，直到遍历结束。
		+ 这样我们利用 Morris 遍历的方法，后序遍历该二叉搜索树，即可实现线性时间与常数空间的遍历。
+ 题解1
```cpp
class Solution {
public:
    void postorder(TreeNode *root, vector<int> &res) {
        if (root == nullptr) {
            return;
        }
        postorder(root->left, res);
        postorder(root->right, res);
        res.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> res;
        postorder(root, res);
        return res;
    }
};
```
+ 题解2
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        stack<TreeNode *> stk;
        TreeNode *prev = nullptr;
        while (root != nullptr || !stk.empty()) {
            while (root != nullptr) {
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            if (root->right == nullptr || root->right == prev) {
                res.push_back(root->val);
                prev = root;
                root = nullptr;
            } else {
                stk.push(root);
                root = root->right;
            }
        }
        return res;
    }
};
```
+ 题解3
```cpp
class Solution {
public:
    void addPath(vector<int> &vec, TreeNode *node) {
        int count = 0;
        while (node != nullptr) {
            ++count;
            vec.emplace_back(node->val);
            node = node->right;
        }
        reverse(vec.end() - count, vec.end());
    }

    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        TreeNode *p1 = root, *p2 = nullptr;

        while (p1 != nullptr) {
            p2 = p1->left;
            if (p2 != nullptr) {
                while (p2->right != nullptr && p2->right != p1) {
                    p2 = p2->right;
                }
                if (p2->right == nullptr) {
                    p2->right = p1;
                    p1 = p1->left;
                    continue;
                } else {
                    p2->right = nullptr;
                    addPath(res, p1->left);
                }
            }
            p1 = p1->right;
        }
        addPath(res, root);
        return res;
    }
};
```
## 21.[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)
+ 题目：
	+ 给你一个二叉树的根节点 `root` ， 检查它是否轴对称。
+ 思路
	+ 