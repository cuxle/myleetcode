### 20210804
  #### 题目1描述:有序数组转成平衡二叉树 
  给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。
  高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。
  输入：nums = [-10,-3,0,5,9]
  输出：[0,-3,9,-10,null,5]
  解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
  #### 答案
  ```
    /**
   * Definition for a binary tree node.
   * struct TreeNode {
   *     int val;
   *     TreeNode *left;
   *     TreeNode *right;
   *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
   *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
   *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
   * };
   */
  class Solution {
  public:
      TreeNode* sortedArrayToBST(vector<int>& nums) 
      {
          if (nums.size() == 0) return nullptr;
          return sortedArrayToBST(nums, 0, nums.size() - 1);
      }
      TreeNode *sortedArrayToBST(vector<int>& nums, int start, int end) 
      {
          if (start > end) {
              return nullptr;
          }
          int mid = (start + end) / 2;
          TreeNode *root = new TreeNode(nums[mid]);
          root->left = sortedArrayToBST(nums, start, mid - 1);
          root->right = sortedArrayToBST(nums, mid + 1, end);
          return root;
      }
  };
  ```
#### 题目2 合并两个有序数组
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。
示例 1：

输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
示例 2：

输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
 

提示：

nums1.length == m + n
nums2.length == n
0 <= m, n <= 200
1 <= m + n <= 200
-109 <= nums1[i], nums2[i] <= 109

##### 答案
```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int nums1Index = m - 1, nums2Index = n - 1;
        for (int i = m + n - 1; i >= 0; i--) {
            if (nums1Index == -1 && nums2Index != -1) {
                while (i >= 0) {
                    nums1[i] = nums2[nums2Index--];
                    i--;
                }
                break;
            } 
            if (nums2Index == -1 && nums1Index != -1) {
                break;
            }
            if (nums1[nums1Index] > nums2[nums2Index]) {
                nums1[i] = nums1[nums1Index--];
            } else {
                nums1[i] = nums2[nums2Index--];
            }
            
        }
    }
};

```

#### 题目3描述 
你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

 
示例 1：

输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false 
调用 isBadVersion(5) -> true 
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。
示例 2：

输入：n = 1, bad = 1
输出：1
 

提示：

1 <= bad <= n <= 231 - 1

#### 答案
```
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            int mid = (r - l) / 2 + l ;
            if (isBadVersion(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```
#### 题目4 爬楼梯
  假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
#### 答案
```
class Solution {
public:
    
    int climbStairs(int n) {
       int first = 1, second = 1;
       if (n == 1) return 1;
       int i = 1;
       while (i != n) {
           int c = first + second;
           first = second;
           second = c;
           i++;
       }
       return second;
    }
};
```

#### 题目5 买卖股票最佳时机
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

 

示例 1：

输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2：

输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
 

提示：

1 <= prices.length <= 105
0 <= prices[i] <= 104

#### 答案
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxP = -1;
        int minV = 0;
        for (int i = 0, minV = prices[0]; i <= prices.size() - 1; i++) {
            maxP = max(maxP, prices[i] - minV);
            minV = min(minV, prices[i]);
        }
        return maxP;
    }
};
```

#### 题目6：最大子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
示例 1：

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
示例 2：

输入：nums = [1]
输出：1
示例 3：

输入：nums = [0]
输出：0
示例 4：

输入：nums = [-1]
输出：-1
示例 5：

输入：nums = [-100000]
输出：-100000
 

提示：

1 <= nums.length <= 3 * 104
-105 <= nums[i] <= 105

