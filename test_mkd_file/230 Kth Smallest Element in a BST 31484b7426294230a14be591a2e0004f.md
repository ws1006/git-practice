# 230. Kth Smallest Element in a BST

Level: Medium
Create: August 11, 2023 11:44 AM
Note: 非常常見的題目，是個學習DFS的應用與變化
Related Topic: BST, DFS
Status: 不熟
學習次數: 1

**Outline**

# 問題敘述

---

Given the `root` of a binary search tree, and an integer `k`, return *the* `kth` *smallest value (**1-indexed**) of all the values of the nodes in the tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
Input: root = [3,1,4,null,2], k = 1
Output: 1

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3

```

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

# 分析思路

---

要找到 kth 最小值，會直覺想到 ***DFS*** 找**最左邊 Leaf** 就是整棵樹最小值。而因為是利用遍歷的方式，我們會從左樹一路往上慢慢檢查，並且去利用 `k-=1` 來記錄這是第幾個最小值，直到 `k == 0` 就是我們要找的值。

### **注意事項**

- ***BST** 本*身是一個不論在哪個 node 上，其左邊子樹的值都比 sub_root 小、右邊子樹都比sub_root大
- **因此，每個左子樹的右子樹依然會我處在的 `node.val` 還要小，因此不要忘記：
我們也會有要檢查右子樹的！**

# Code

---

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        curr = root

        while curr or stack:
            while curr:  # 一路往最左邊先找，直到最末端開始往回查
                stack.append(curr)
                curr = curr.left
            curr = stack.pop()
            k -= 1
            if k == 0:
                return curr.val
            curr = curr.right # 因為某右子樹的左子樹
```

### **圖解為何要檢查右子樹：**

![image (1).jpeg](230%20Kth%20Smallest%20Element%20in%20a%20BST%2031484b7426294230a14be591a2e0004f/image_(1).jpeg)

- 假設這整個是某一個 **BST** 的子樹，當利用 `stack.pop()`從 **1 → 2→ 3** （*因為都都沒有右子樹）*後，當你進到 **4** 的子樹，**A 若要符合 BST，勢必會是這種關係：3 < A < 4。**
- 因此我們一步步回朔第**k個最小值時，一定要去檢查某個右子樹的左子樹中的各值才正確。**

# Reference Links

---

-