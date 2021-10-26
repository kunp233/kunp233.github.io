---
title: monotone stack
date: 2021-10-26 17:27:01
tags:
---

[例题](https://leetcode-cn.com/problems/next-greater-element-i/)

#题目描述

---

给你两个 没有重复元素 的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。

请你找出 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 -1 。

---

#样例

---

输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于 num1 中的数字 4 ，你无法在第二个数组中找到下一个更大的数字，因此输出 -1 。
    对于 num1 中的数字 1 ，第二个数组中数字1右边的下一个较大数字是 3 。
    对于 num1 中的数字 2 ，第二个数组中没有下一个更大的数字，因此输出 -1

---

先贴上自己的代码

```c#
public class Solution
{
    public int[] NextGreaterElement(int[] nums1, int[] nums2)
    {
        int n1=nums1.Length,n2=nums2.Length;
        Stack<int> s=new Stack<int>();
        int[] value=new int[10000+10];
        int[] ans=new int[n1];
        s.Push(nums2[n2-1]);
        value[nums2[n2-1]]=-1;
        for(int i=n2-2;i>=0;--i)
        {
            while(s.Count()>0&&s.Peek()<=nums2[i]) s.Pop();
            if(s.Count==0) value[nums2[i]]=-1;
            else value[nums2[i]]=s.Peek();
            s.Push(nums2[i]);
        }
        for(int i=0;i<n1;++i)
        {
            ans[i]=value[nums1[i]];
        }
        return ans;
    }
}
```

一开始看到题目说最优解是O(n1+n2)，但是实在没想到怎么做，只好用n2做了个哈希然后暴力过了

当时看到线性复杂度，就知道扫一遍时除了处理自己还要顺带把别人给处理了

**不过既然暴力是从自己开始往右边搜，为什么不考虑一下一开始就自从最右边开始往左边来呢？**

既然要求右边第一个大的，那么不妨从右边开始维护一个单减的序列，那么就有两种情况：

1. 继续单减，此时很显然刚好右边那个就是第一个大于自己的；
2. 大于等于了，对于自己来说，自己此时要找到右边第一个大于自己的；对于自己左边的来说，**自己右边且小于自己的必不可能成为他们的解**，因此这时候倒着回去找符合要求的，把不符合的都出栈就好了

由于出栈操作最多执行n次，那么时间复杂度依然是线性，ok，最后记一下答案打印顺序就行了
