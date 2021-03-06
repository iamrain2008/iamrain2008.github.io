---
title: 二分查找法从入门到精通
date: 2020-02-08 00:05:03
categories:
- Algorithm
tags:
- binary search
- algorithm
- leetcode
---

# 二分法的本质


二分法通常用于在有序的集合中查找目标。通过O(1)的操作，将规模为`n`的问题分解为`n/2`，最终时间复杂度是`log(n)`。通常要求做到`log(n)`的时间复杂度，基本上都是二分法。


# 二分法的痛点


* 死循环
  * 二分法通过`循环条件`+`指针变化`来推进，如果这两个组合不正确，就会导致死循环。
* 是否可以使用递归(Recursion)
  * 使用递归时，代码会更加清晰，写法更简单。不能用递归时，写法会复杂很多。有时候需要注意问题规模，使用递归是否可能导致溢出。


# 二分法的基本写法


```
private int binarySearch(int[] nums, int target) {

    // 对数据合法性进行判断
    int badResult = -1;     // 表示没有target或nums不合法的数字，例如-1
    if (nums不合法) {
        return badResult;
    }

    // 设定左右指针
    int left = 0;
    int right = nums.length - 1;

    // 开始循环
    while (循环条件) {
        
        // 设定mid，这里的小技巧是使用右减左除以2再加上左，避免溢出，更好的写法是相加后无符号右移一位
        int mid = (right + left) >>> 1;
        
        // 判断mid是否是所求目标，是的话返回
        if (nums[mid] == target) {
            return mid;
        }

        // 不是的话，判断移动左还是右指针，将问题规模减小一半
        if (target在左侧) {
            移动右指针
        } else {
            移动左指针
        }
    
    }

    // 视上述循环条件和移动指针的方式，执行额外的最终判断
    最终判断
    // 如果最终没有满足条件的结果，返回失败
    return badResult;
}
```

总结一下就是通过循环，不停的移动左右指针将问题规模减半，直到解决问题。

这里的难点是`循环条件`和`移动左右指针的方法`，这两者使用不同的组合会有不同的效果，使用错了会导致死循环。下面我介绍一些常见的循环条件和移动左右指针的方法的组合。


# 二分法常见模板


基本上所有的二分查找算法都可以用下面3种模板之一来解决。

## 模板1

项目 | 代码
--- | ---
左指针 | left = 0
右指针 | right = length - 1
循环条件 | left <= right
左指针移动 | left = mid + 1
右指针移动 | right = mid - 1
最终判断 | 无

这个模板在退出循环时，left与right会交错而过(即left - 1 == right)，此时所有可能项目都查找过了，所以不需要额外的操作。


## 模板2

项目 | 代码
--- | ---
左指针 | left = 0
右指针 | right = length
循环条件 | left < right
左指针移动 | left = mid + 1
右指针移动 | right = mid
最终判断 | left是否是target？

模板2的right初始值需要注意一下，是length，这是为了配合left < right这个循环条件。

这个模板在退出循环的时候，一定有left == right，此时因为不满足循环条件，所以这个值还没有判断过，于是在退出循环之后，还需要判断一次left是否是target（判断right也行，因为退出循环时left必然等于right）。


## 模板3

项目 | 代码
--- | ---
左指针 | left = 0
右指针 | right = length - 1
循环条件 | left + 1 < right
左指针移动 | left = mid
右指针移动 | right = mid
最终判断 | left是否是target？right是否是target？

模板3在退出循环的时候，left + 1 == right，也就是查找到了最后2个项目，这两个项目没有在循环中判断过，所以最终判断时需要分别看一下left和right是否是target


# 模板的选择

上面介绍了3种模板，他们的核心区别是

<div style="width: 80px">项目</div> | <div style="width: 33%">模板1</div> | <div style="width: 33%">模板2</div> | <div style="width: 33%">模板3</div>
--- | --- | --- | ---
左指针 | left = 0 | left = 0 | left = 0
右指针 | right = length - 1 | right = length | right = length - 1
循环条件 | left <= right | left < right | left + 1 < right
左指针移动 | left = mid + 1 | left = mid + 1 | left = mid
右指针移动 | right = mid - 1 | right = mid | right = mid
最终判断 | 无 | left是否在界限内，是不是target？ | left是否是target？right是不是target？
核心区别 | 便于解决只需要通过元素本身就能判断是否为目标的问题 | 便于解决需要元素本身及邻居来判断是否为目标的问题 | 模板1、2的加强版，万能模板


## 模板1 案例

只要通过元素本身即可判断是否为目标的问题，用模板1会很方便。

* [LeetCode 704. Binary Search](https://leetcode.com/problems/binary-search/)
    > 经典二分查找法


## 模板2 案例

不仅要靠元素本身，还需要邻居才能判断是否为目标，用模板2更方便。

* [LeetCode 278. First Bad Version](https://leetcode.com/problems/first-bad-version/) 
    > 处理`AA...AABB...BB`类型的问题，找出最后一个A或者第一个B。
* [LeetCode 34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
    > 处理`AABBCCDD`类型的问题，找出B的开始和结束。
* [LeetCode 162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)
    > 处理山峰的问题，非常典型。需要通过元素和邻居来判断指针如何移动。


## 模板3 案例

模板3是模板1、2的加强版，是一个万能二分法模板，基本上所有的二分法问题都能解决。
有时候我们用模板2去做一些`AABB`类型的题目时，会碰到死循环，原因是左右中位数选择的问题，这个时候需要将mid换成右中位数( mid = right + left + 1 >>> 1; )，不熟练的话，你都不知道自己为什么死循环了。
但我们用模板3，则简单无脑，所以你可以所有二分法都用模板3，反正肯定不会出错就是了。

* [LintCode 183. Wood Cut](https://www.lintcode.com/problem/wood-cut/description)
    > 这个例子如果用模板2，需要搞清楚用左右中位数的哪个，否则容易死循环，用模板3则轻松愉快。


# 总结

模板1到模板3是一个进化的关系，目的就是为了更方便的解决问题，不让自己陷入死循环。通常能用模板1解决的问题，都能用模板2解决；能用模板2解决的问题，都能用模板3解决。而且模板3是最不容易出错的写法。
所以你可以只记住模板3的写法，它基本上能解决所有二分法问题。

