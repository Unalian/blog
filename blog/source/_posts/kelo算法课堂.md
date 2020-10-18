---
date: 2020/2/8
tags:
- 自学笔记
- 算法
categories:
- 算法
---
# Kelo算法课堂(持续更新)

*kelo做算法题并在其博客上连载，窃以为其思想精妙，故书吾感，记以学之。*

***原作见友链***
<!-- more -->
1. 求最长回文串（Manacher）

   * 思想

     * 预处理 将原串中加入#，目的在于可以识别#a#b#b#a的回文串。

       ​			 在开头加入&,目的在于使回文串的识别从1开始

     * for循环对length[i]进行求解。length[i]的实际意义是**回文半径**

       ```
       # a # b # c # c # b # d #
       1 2 1 2 1 2 5 2 1 2 1 2 1 
       ```

       如上，第二行即为回文半径表，我们可以看出，以length[i].max为中心，根据其回文半径取得的回文字符串去掉#后即为最长回文串。

       问题转化为求length[i]，以及**最大的回文长度(max_length)**和**回文中心(max_mid)**。实际上，最大的max_length和max_mid在i循环求length[i]的过程中进行更新即可。

     * 当i在推算出来的现在所处的最大回文序列的右边界的左侧，则可以通过**简便方式推算**。

       * 当max_right对结果无影响时（max_right足够大），我们可以直接先令 length[i] = length[2 * pos - i]
       * 如果以i为中心的回文串长度超过max_right时候，由于我们无法判断max_right之后的情形，我们只能令length[i] = max_right - i
       * 如果在最大回文序列右侧，则将它初始化为一

     * 以i为中心，while循环测试以此时的i为中心向两边扩展，当两边没有达到边界且两边相同时，length[i]++

     * 更新当前i为中心的回文序列的右边界max_right

     * 将当前的回文序列长度和最大回文序列长度比较判断是否需要更新这个长度。

   * 代码实现

   ```javascript
   /**
    * @param {string} s
    * @return {string}
    */
   
   let s = "afkfkfkaaaafkfk";
   
   var longestPalindrome = function(s) {
       let f = "&#";
       for (let i = 0;i < s.length; i++){
           f = f + s[i];
           f = f + "#";
       }
       let length = [];
       let max_right = 0, max_length = 0, pos = 0, max_pos = 0;
       for (let i = 1;i < f.length;i++) {//求length[i]的循环
           if (i < max_right) {
               /*如果要求的i在推算出来的现在所处的最大回文序列的右边界的左侧，则可以通过简便方式进行推算：
               当max_right对结果无影响时（max_right足够大），我们可以直接先令 length[i] = length[2 * pos - i]
               如果以i为中心的回文串长度超过max_right时候，由于我们无法判断max_right之后的情形，我们只能令length[i] = max_right - i*/
               length[i] = Math.min(max_right - i, length[2 * pos - i]);
           } else length[i] = 1;
           while (i - length[i] >=0 && i + length[i] < f.length && f[i - length[i]] === f[i + length[i]]){
               length[i] = length[i] + 1;
           }//这个循环就是在测试以此时的i为中心向两边扩展，1.没有到达数组边界 2.两方相等。如果两个条件都满足，则length[i]++
           if (i + length[i] - 1 > max_right) {
               max_right = i + length[i] - 1;
               pos = i;
           }//更新当前所在的最大回文序列的右边界
           if (length[i] > max_length) {
               max_length = length[i];
               max_pos = i;
           }//更新最大的回文长度和回文中心
       }
       let max_sub_string = f.substring(max_pos - max_length + 1, max_pos + max_length);
       let real_sub = "";
       for (let i = 0;i < max_sub_string.length;i++) {
           if (max_sub_string[i] === "#") continue;
           real_sub = real_sub + max_sub_string[i];
       }
       return real_sub;
   };
   
   console.log(longestPalindrome(s));
   ```

2. 给定 n 个非负整数 a1，a2，…，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。（双指针法）

   设置l，r两个指针，l指向数组开头，r指向数组结尾，计算容量，然后将较短的指针向前中间移动。每次移动指针后计算容量判断是否更新max。

   *注：移动较短的是因为移动结果为容量变大或者不变。但是移动较长的只会容量变小或者不变*

   ```javascript
   var maxArea = function(height) {
       let l = 0;
       let r = height.length - 1;
       let max_area = 0;
       while (l !== r) {
           max_area = Math.max(max_area, (r - l) * Math.min(height[l], height[r]));
           if (height[l] < height[r]) l++;
           else r--;
       }
       return max_area;
   };
   ```

   * 数组.length
   * Math.mac()
   * L !== r
   
3. 已知n个括号对，将输出这些括号对的合法搭配，比如2个括号对的合法组合是：()(), (())

   ```javascript
   var generateParenthesis = function(n) {
       let ans = [];
       for (let set = 0; set < 1 << (2 * n); set++) {
           let flag = 0;
           let sub_ans = "";
           for (let i = (1 << (2*n -1)); i ; i = i >> 1) {
               if ((set & i) === i){
                   flag = flag + 1;
                   sub_ans = sub_ans + "(";
               }else{
                   flag = flag - 1;
                   sub_ans = sub_ans + ")"
               }
               if (flag < 0 || flag > n) break;
           }
           if (flag === 0) {
               ans.push(sub_ans);
           }
       }
       return ans;
   };
   console.log(generateParenthesis(3));
   ```

1看做左括号，0看做右括号，set为整个括号序列的二进制代表 ()()->1010

遍历set即遍历0000 -> 1111

取每一个set 对i进行遍历，与set比对。比如set = 1100，i取1000，100，10，1，分别与set进行&操作，1100 & 1000 == 1000，1100 & 1== 0，由此可以判断set每一位是1/0。

1则flag加1，0则flag-1，当flag<0(右在左)或者flag>n(左括号已经肯定多于右括号)跳出。

i循环完或者break后，对flag进行判断，如果flag === 0，则此set合法，将其压栈。

4. 两数相加**（链表的应用）**

```javascript

/*Definition for singly-linked list.*/
function ListNode(val) {
    this.val = val;
    this.next = null;
}
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */

var changeType = function(num){
    let node = new ListNode(-1);
    let node1 = new ListNode(-1);
    for(let i = 10;; i*=10){
        let part = (num % i)/(i/10);
        num -=(part*(i/10));
        if(node.val !== -1){
            node1 = new ListNode(part);
            node1.next = node;
            node = node1;
        }else{
            node = new ListNode(part);
        }
        if(i > num)
            break;
    }
    return node1;
}

var addTwoNumbers = function(l1, l2) {
    let head = new ListNode;
    let present = head;
    let c = 0;
    while (1 === 1) {
        let tmp = new ListNode;
        if (l1 === null && l2 === null) {
            if (c === 1) {
                tmp.val = c;
                tmp.next = null;
                present.next = tmp;
                present = present.next
            }
            break;
        }
        if (l1 === null) {
            tmp.val = l2.val + c;
            if (tmp.val >= 10) {
                tmp.val = tmp. val - 10;
                c = 1;
            } else c = 0;
            tmp.next = null;
        } else
        if (l2 === null) {
            tmp.val = l1.val + c;
            if (tmp.val >= 10) {
                tmp.val = tmp. val - 10;
                c = 1;
            } else c = 0;
            tmp.next = null;
        } else {
            let tmp_ans = l1.val + l2.val + c;
            if (tmp_ans >= 10) {
                tmp_ans = tmp_ans - 10;
                c = 1;
            } else c = 0;
            tmp.val = tmp_ans;
        }
        tmp.next = null;
        present.next = tmp;
        present = present.next;
        if (l1 === null) l2 = l2.next;
        else if (l2 === null) l1 = l1.next;
        else {
            l1 = l1.next;
            l2 = l2.next;
        }
    }
    head = head.next;
    return head;
};

console.log(addTwoNumbers(changeType(12),changeType(12)));
```

用到了链表的数据结构，changType改变(int)num为链表，addTwoNumbers将两个链表对应的数相加。

5. 求一个字符串的最大无重复子串**（滑动窗口）**

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let map = new Map();
    let l = 0;
    let r = 1;
    let max_length = s.length;
    let max_ans = 1;
    let substring = s.substring(l, r);
    map.set(s[l], 1);
    while (r < max_length && max_length-r-1>max_ans) {
        if (!map.has(s[r])) {
            map.set(s[r], 1);
            r++;
            if (r - l > max_ans) {
                max_ans = r - l;
                substring = s.substring(l, r);
            }
        } else {
            map.delete(s[l]);
            l++;
        }
    }
    return substring.length;
};
```

类似于双指针，初始化l=0，r=1，r右移，新收入的s[r]与之前的比，如果有相同，则l右移直至没有相同的。

6. leetcode42 

![](https://img.cetacis.dev/uploads/big/8408af07f366c184d8ff194926fe450b.png)

**单调栈的使用**

```python
from typing import List


class Solution:
    def trap(self, height: List[int]) -> int:
        if not height:
            return 0
        ans = 0
        stack = []
        i = 0
        while i + 1 < len(height) and height[i] < height[i + 1]:
            i = i + 1
        stack.append((height[i], i))
        i = i + 1
        while i < len(height):
            if height[i] <= stack[-1][0]:
                stack.append((height[i], i))
            else:
                last_height = -1
                while len(stack) != 0 and stack[-1][0] < height[i]:
                    if last_height == -1:
                        last_height = stack[-1][0] 
                    else:
                        ans = ans + (i - stack[-1][1] - 1) * (stack[-1][0] - last_height)
                        last_height = stack[-1][0]
                    stack.pop()
                if len(stack) != 0:
                    ans = ans + (i - stack[-1][1] - 1) * (height[i] - last_height)
                stack.append((height[i], i))
            i = i + 1
        return ans


if __name__ == '__main__':
    a = [3,2,1,3]
    print(Solution().trap(a))

```

数据结构：

* stack的操作：[].append(***)入栈；[].pop()出栈；栈顶是stack[-1]
* list判空：if (not) list:/if list is (not) None:/if len(list)==0
* 二元组：a = (X, X)  访问的时候a[0]/a[1]。在此例中，(x, x)可作为栈元素，来确定元素在栈的位置。

思想：

* 开始先将单调递增的部分取出，这部分不可能装水
* 将单调递减的部分入栈，然后在出现比栈顶大的部分进入计算部分。
* 设置last_height，并且维护。

主函数调用：

```python
if __name__ == '__main__':
```

7. leetcode 29

快速除法

```python
def minus(x, y, step, origin_y) -> int:
    #  真正的除法过程
    ans = 0
    if x == 0:
        return 0
    while x - y >= 0:
        x = x - y
        ans = ans + pow(2, step)
    if y == origin_y:
        return ans
    return minus(x, y >> 1, step - 1, origin_y) + ans


class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        flag = 0
        if dividend < 0:
            # 被除数小于零
            dividend = - dividend
            flag = flag ^ 1
        if divisor < 0:
            divisor = - divisor
            flag = flag ^ 1
            # flag代表正负
        ans = minus(dividend, divisor << 32, 32, divisor)
        ans = ans if flag == 0 else -ans
        ans = ans if ans < 2147483647 else 2147483647
        return ans


if __name__ == '__main__':
    a = 2
    b = 1
    c = Solution()
    print(Solution.divide(c,a, b))

```

思想：

核心思想在于，每一个数都可以拆成二的不同次幂的线性组合

本题是除法，即将被除数拆成系数为除数倍数的不同二次幂的组合。

比如：8/2 即8=2x(2^4) 9/3即9 = 3x(2^1+2^0) 27/3 = 3x(2x2^2+2^0)

方法即从 除数x2^32 开始和被除数比较，并且每次比较后，如果被除数比除数乘2的次方小，则将除数乘2的次方右移（除以2）。用到了递归的方法。

语法积累：

* 

* ```python
   ans = ans if flag == 0 else -ans
  ```

* 抑或

  ```python
   flag = flag ^ 1
  ```

* 如果本类中的某方法要调用本类中的其他方法，则需要加入self的参数。因为要调用类方法，必须要对类进行实例化。

8. leetcode33

本来有一个有序数列，如[1,2,3,4,5,6,7]

从某个位置进行切割拼接 如[4, 5, 6, 7, 1, 2, 3]

思路：

* 主要还是二分法
* 在这里有所不同，就是开始的时候分类，当nums[left] <= nums[mid]，即改变点在中点右侧，则left到改变点之间是可以用二分法的，则当nums[left] <= target < nums[mid]，right = mid-1，之后即为普通二分法，如果其他，则将left移动到mid+1.这样做，迟早会找到一个符合nums[left] <= target < nums[mid]或者nums[mid] < target <= nums[right]:的区间，然后就是正常的二分法了。

```javascript
class Solution:
    def search(self, nums, target):
        if not nums:
            return -1
        left = 0
        right = len(nums) - 1
        while left < right:
            print(left, right)
            mid = (left + right) >> 1
            if nums[mid] == target:
                return mid
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return left if nums[left] == target else -1

```

9. leetcode 34

在排列数组中找到某一个target number的位置。

二分+递归，当二分失败l>r return，或者target == a[mid]return，或者ans[0] < mid < ans[1]，主要是对l/r和ans[0],and[1]进行大小比对。

```python
from typing import List


class Solution:
    def bin_search(self, a:List[int], l:int, r:int, target:int, ans:List[int]):
        if l > r:
            return
        mid = (l + r) >> 1
        if a[l] == target:
            ans[0] = min(ans[0], l)
            ans[1] = max(ans[1], l)
        if a[r] == target:
            ans[0] = min(ans[0], r)
            ans[1] = max(ans[1], r)
        if ans[0] < mid < ans[1]:
            return
        if target == a[mid]:
            ans[0] = min(ans[0], mid)
            ans[1] = max(ans[1], mid)
            self.bin_search(a, l, mid - 1, target, ans)
            self.bin_search(a, mid + 1, r, target, ans)
            return
        else:
            if target > a[mid]:
                self.bin_search(a, mid + 1, r, target, ans)
            else:
                self.bin_search(a, l, mid - 1, target, ans)

    def searchRange(self, nums: List[int], target: int) -> List[int]:
        ans = [200000000, -1]
        self.bin_search(nums, 0, len(nums) - 1, target, ans)
        if ans[1] == -1:
            return [-1, -1]
        else:
            return ans


if __name__ == '__main__':
    nums = [7, 7, 7, 7, 7, 7, 10]
    target = 7
    print(Solution().searchRange(nums, target))


```

10. leetcode 36

![题意](https://img.cetacis.dev/uploads/big/e379af099611ed74f5ef430b3ce538f1.png)

**题解**

看到这道题，很自然地想到嵌套循环，设置i,j分别遍历横排，竖排和3x3的块。难点在于python没有提供二维数组，所以要用自定义的数据结构存储和访问每一个元素。其中有一个flag要素，其实就是对每一个数字进行标记，如果某个数字的flag大于一，即出现了两次以上，则return false。

```python
from typing import List


class Solution:
    def isValidSudoku(self, board: List[List[str]]):
        for i in range(0, 9):
            flag = [0] * 10
            for j in range(0, 9):
                if board[i][j] == ".":
                    continue
                if flag[int(board[i][j])] == 0:
                    flag[int(board[i][j])] = 1
                else:
                    return False
        j = 0
        while j < 9:
            flag = [0] * 10
            for i in range(0, 9):
                if board[i][j] == ".":
                    continue
                if flag[int(board[i][j])] == 0:
                    flag[int(board[i][j])] = 1
                else:
                    return False
            j = j + 1
        for i in range(0, 9, 3):
            for j in range(0, 9, 3):
                flag = [0] * 10
                for step_i in range(0, 3):
                    for step_j in range(0, 3):
                        if board[i + step_i][j + step_j] == ".":
                            continue
                        if flag[int(board[i + step_i][j + step_j])] == 0:
                            flag[int(board[i + step_i][j + step_j])] = 1
                        else:
                            return False
        return True
a = Solution()
if Solution.isValidSudoku(a,[["7","3",".",".","7",".",".",".","."],
                             ["6",".",".","1","9","5",".",".","."],
                             [".","9","8",".",".",".",".","6","."],
                             ["8",".",".",".","6",".",".",".","3"],
                             ["4",".",".","8",".","3",".",".","1"],
                             ["7",".",".",".","2",".",".",".","6"],
                             [".","6",".",".",".",".","2","8","."],
                             [".",".",".","4","1","9",".",".","5"],
                             [".",".",".",".","8",".",".","7","9"]]):
    print ("True")
else:
    print ("False")
```

11. leetcode 50

快速幂 即通过简单乘法实现幂运算

**题解**

有一些像之前lt29做过的减法实现除法，一个指数可以拆分成很多子项相乘，比如3^8 = 3^4 x 3^4，此时，我们计算3^4再将结果相乘就可得到正确答案，这样节省了算两次3^4 的时间，可以满足时间复杂度。

又 3^4 = 3^2 x 3^2，此时计算3^2 = 3*3。可以看出，这是一个递归的过程，每次将幂 >> 1，结束条件是幂=0 return 1。当然，幂也有奇数的可能性，当幂是奇数，只要将这一次的return改成 底数 x temp x temp即可。如果幂是偶数，则返回temp x temp。

但是这个求幂的过程，底数和指数只能是正数，当底数和指数是负数的时候，需要将负数变为正数，再使用如上递归方程。并且对求得的幂进行符号/倒数修改，这就要求对底数、指数进行分类讨论。

una代码如下：

````python
class Solution(object):
    def Pow(self, x, n):
        if n == 0:
            return 1
        else:
            temp = self.Pow(x, n >> 1)
        if n % 2 == 0:
            return temp * temp
        else:
            return x * temp * temp

    def myPow(self, x, n):
        if x < 0 and n % 2 == 0:
            return self.Pow(-x, n) if n > 0 else self.Pow(-x, -n)
        elif x < 0 and n > 0:
            return -self.Pow(-x, n)
        if n < 0:
            return 1/self.Pow(x, -n) if x > 0 else 1/-self.Pow(-x, -n)
        else:
            return self.Pow(x, n)

a = Solution()
print (Solution.myPow(a, -1, -2))

````

kelo代码如下：

```python
class Solution:
    def quick(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        if n == 1:
            return x
        res = 1
        if n & 1 == 1:
            res = x
        half = self.quick(x, n >> 1)
        return half * half * res

    def myPow(self, x: float, n: int) -> float:
        if n > 0:
            return self.quick(x, n)
        else:
            return self.quick(1/x, -n)
```

12. leetcode 37 解数独

顾名思义，解决数独，默认题目可解且有唯一解。

**题解**

第一想法是直接试错法，但是一想这个实验次数岂不是指数爆炸而且极其不美观，现实代码中使用递归试错（穷举）。

值得注意的点除了这种递归的方法进行穷举以外，还有对某个数字是否出现过的控制方式。具体的做法和解析写在了注释里。

```python
from typing import List


class Solution:
    COL = [0] * 10
    ROW = [0] * 10
    BLOCK = [0] * 10
    Flag = 0

    def solve(self, board: List[List[str]], i, j):
        if self.Flag == 1:
            return
        if j == 9:
            self.solve(board, i + 1, 0)
            return
        if i == 9:
            self.Flag = 1
            return
        if board[i][j] != ".":
            self.solve(board, i, j + 1)
            return
        for x in range(1, 10):
            if self.COL[i] & (1 << x) == 1 << x or self.ROW[j] & (1 << x) == 1 << x or \
                    self.BLOCK[int(i / 3) * 3 + int(j / 3)] & (1 << x) == 1 << x:
                # 当行/列/块为x，则跳出本步循环。
                # 即当x这个数已经在i行/列/块存在，不对需要col/row/block进行操作
                # 因为已经有此数，则意味着不能填入此数
                continue
            board[i][j] = str(x)
            # 填入x
            self.COL[i] = self.COL[i] | (1 << x)
            self.ROW[j] = self.ROW[j] | (1 << x)
            self.BLOCK[int(i / 3) * 3 + int(j / 3)] = self.BLOCK[int(i / 3) * 3 + int(j / 3)] | (1 << x)
            # 填入x后在col，row，block中进行维护
            self.solve(board, i, j + 1)
            if self.Flag == 1:
                return
            # 当i=9，即将所有的数字按照规则填充完成，则flag = 1，退出。
            self.COL[i] = self.COL[i] ^ (1 << x)
            self.ROW[j] = self.ROW[j] ^ (1 << x)
            self.BLOCK[int(i / 3) * 3 + int(j / 3)] = self.BLOCK[int(i / 3) * 3 + int(j / 3)] ^ (1 << x)
            board[i][j] = "."
     # flag != 1,即未正确填充，则将此数，将col,row,block标记修改回之前的样子，并且将board[]
    def solveSudoku(self, board: List[List[str]]) -> None:
        for i in range(0, 10):
            self.COL[i] = 0
            self.ROW[i] = 0
            self.BLOCK[i] = 0
        for i in range(0, 9):
            for j in range(0, 9):
                if board[i][j] == ".":
                    continue
                self.COL[i] = self.COL[i] | (1 << int(board[i][j]))
                self.ROW[j] = self.ROW[j] | (1 << int(board[i][j]))
                self.BLOCK[int(i / 3) * 3 + int(j / 3)] = self.BLOCK[int(i / 3) * 3 + int(j / 3)] | (1 << int(board[i][j]))
            # 举例理解，当为i = 0，j = 0,board[0][0]=2, col[0] = 00000000100
            # 当j++, i = 0,j = 1,board[0][1] = 1 col[0]= 0000000110
            # 即，col[i]记录的，是i行的那些数字出现过。如0000000110就是1，2出现过（注意是第零位为0）
        self.solve(board, 0, 0)


if __name__ == '__main__':
    bd = [[".", ".", "9", "7", "4", "8", ".", ".", "."], ["7", ".", ".", ".", ".", ".", ".", ".", "."],
     [".", "2", ".", "1", ".", "9", ".", ".", "."], [".", ".", "7", ".", ".", ".", "2", "4", "."],
     [".", "6", "4", ".", "1", ".", "5", "9", "."], [".", "9", "8", ".", ".", ".", "3", ".", "."],
     [".", ".", ".", "8", ".", "3", ".", "2", "."], [".", ".", ".", ".", ".", ".", ".", ".", "6"],
     [".", ".", ".", "2", "7", "5", "9", ".", "."]]
    print(Solution().solveSudoku(bd))
    print(bd)

```

13. leetcode 38 外观数组

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

1.     1
2.     11
3.     21
4.     1211
5.     111221
1 被读作  "one 1"  ("一个一") , 即 11。
11 被读作 "two 1s" ("两个一"）, 即 21。
21 被读作 "one 2",  "one 1" （"一个二" ,  "一个一") , 即 1211。

给定一个正整数 n（1 ≤ n ≤ 30），输出外观数列的第 n 项。

注意：整数序列中的每一项将表示为一个字符串。

**题解**

因为ans[i]由ans[i-1]得来，则需从ans[2]开始求直到求到ans[n]。

为了求ans[i]，可对ans[i-1]进行遍历，对ans[i-1] [j]的j进行循环，当ans[i-1] [j] = ans[i-1] [j+1]，则time ++，相当于如果一个数字重复出现，对出现次数进行++。

直到ans[i-1] [j] ！= ans[i-1] [j+1]，则ans[i]+(str)time+ans[i-1] [j]，然后对time清零，再次进行如上循环操作。

**代码如下**

```python
class Solution(object):
    def countAndSay(self, n):
        ans = [""] * 100
        ans[1] = "1"
        i = 1
        j = 0

        while i < n:
            time = 1
            while j + 1 < len(ans[i]) and ans[i][j] == ans[i][j + 1]:
                time += 1
                j = j + 1
            ans[i + 1] = ans[i + 1] + str(time)+ans[i][j]
            j = j + 1
            if j == len(ans[i]):
                i = i + 1
                j = 0
        return ans[n]

print (Solution().countAndSay(5))
```

14. leetcode 39 40

给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

说明：

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 
示例 1:

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

示例 2:

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

**题解**

这道题我想到先从小到大sort candidates，然后累加最小的元素 candidates[0]，直到 = target 或者大于target。当 = 成立，则将此种list加入set，

后续的操作两种情况一样，将最后两个candidates[0]出栈，尝试加入一个candidates[1]，如果= target 则将此种list加入set。之后，= target 或者大于target都将最后两个元素出栈，尝试加入candidates[1]，若小于则直接加入candidates[3]... 

这种不免为一种思路，我的想法是减少显而易见的优化方法，将工作交给计算机，就是每次减少一个在获得全部由candidates[0]组成的target（可能会大于或相等）然后尝试加入candidates[1]，如果能加入，则再加入candidate[1]，直到加到 和为target或者大于target，如同之前对candidates[0]的操作，将candidates[1]减少一个，试图加入candidates[2]..

这样应该可以用到递归。

之后，我看了kelo的代码，差不多是一样的思路，都用到了res = res - candidates[step]的思想（初试res即为target），这样，可以将res当做一个新的target，对其进行操作。

```python
from typing import List


class Solution:
    ANS = set()
    SUB_ANS = []

    def find(self, step, res, candidates):
        if res == 0:
            self.ANS.add(tuple(self.SUB_ANS))
            return

        if step == len(candidates):
            return

        if candidates[step] <= res:
            res = res - candidates[step]
            self.SUB_ANS.append(candidates[step])
            self.find(step, res, candidates)
            self.find(step + 1, res, candidates)
            res = res + candidates[step]
            del self.SUB_ANS[-1]
        self.find(step + 1, res, candidates)

    def combinationSum(self, candidates, target):
        self.ANS.clear()
        self.SUB_ANS = []
        candidates.sort()
        self.find(0, target, candidates)
        fin = []
        for x in self.ANS:
            fin.insert(len(fin), list(x))
        return fin


if __name__ == '__main__':
    candidates = [1,2,3]
    ta = 3
    print(Solution().combinationSum(candidates, ta))
```

当不能重复使用时，将代码改成如下：

```python
from typing import List


class Solution:
    ANS = set()
    SUB_ANS = []

    def find(self, step, res, candidates):
        if res == 0:
            self.ANS.add(tuple(self.SUB_ANS))
            return

        if step == len(candidates):
            return

        if candidates[step] <= res:
            res = res - candidates[step]
            self.SUB_ANS.append(candidates[step])
            self.find(step + 1, res, candidates)
            res = res + candidates[step]
            del self.SUB_ANS[-1]
        self.find(step + 1, res, candidates)

    def combinationSum(self, candidates, target):
        self.ANS.clear()
        self.SUB_ANS = []
        candidates.sort()
        self.find(0, target, candidates)
        fin = []
        for x in self.ANS:
            fin.insert(len(fin), list(x))
        return fin


if __name__ == '__main__':
    candidates = [1,2,3]
    ta = 3
    print(Solution().combinationSum(candidates, ta))
```

15. leetcode 41

得到一个数组中最小正整数

如：[1,-1,3,5] 得到2

**题解**

这道题……确实只要遍历就可以了，但是对于时间和空间复杂度都有要求，所以可以采用一种自身哈希的方法，在前面的题中也提过。

首先，根据抽屉定理，answer <= n+1

将其中大于n的，小于等于零的设为1。（在这一步之前，先测其中是否有1，如果没有1则输出1）

关键是将索引值作为哈希key。

[1, 6, 2 , -1 , 3, 8]--->] [1, 6, 2, 1,3, 1]  （即包含1 - n元素的数组）

对这个从1-n进行遍历：如果读到数字a，则将第几个元素的符号改变。（使用下表0代表n）

最后返回第一个正数。如果没有，则判断0位置，如果还没有正数则返回n+1。

代码：

```python
class Solution:
    def firstMissingPositive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        
        # 基本情况
        if 1 not in nums:
            return 1
        
        # nums = [1]
        if n == 1:
            return 2
        
        # 用 1 替换负数，0，
        # 和大于 n 的数
        # 在转换以后，nums 只会包含
        # 正数
        for i in range(n):
            if nums[i] <= 0 or nums[i] > n:
                nums[i] = 1
        
        # 使用索引和数字符号作为检查器
        # 例如，如果 nums[1] 是负数表示在数组中出现了数字 `1`
        # 如果 nums[2] 是正数 表示数字 2 没有出现
        for i in range(n): 
            a = abs(nums[i])
            # 如果发现了一个数字 a - 改变第 a 个元素的符号
            # 注意重复元素只需操作一次
            if a == n:
                nums[0] = - abs(nums[0])
            else:
                nums[a] = - abs(nums[a])
            
        # 现在第一个正数的下标
        # 就是第一个缺失的数
        for i in range(1, n):
            if nums[i] > 0:
                return i
        
        if nums[0] > 0:
            return n
            
        return n + 1
```

16. leetcode 45

题意：

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

```
输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**题解**

例子是[2, 3, 1, 1, 4]

结合例子进行解释，nums[0] = 2, 因此跳一次最多到 nums[1]或者nums[2], 将2固定为max_pos，因为这是第一次跳跃后能够到达的最远位置。

- i从0到1到max_pos进行遍历，每次更新max_arrive = max(max_arrive,nums[i]+i)。

  *比如i=1，max_arrive = max(2, 4) = 4*

  这个遍历其实是一个对第一步跳跃后，第二步跳跃可能情况的一个遍历，选出最大的max_arrive。

- 当i和第一次固定的max_pos重叠时，将此时的max_pos 固定在max _arrive，并且跳跃步数++，相当于其实是选中了从确定max_arrive那次的i开始第二次起跳，max_pos 固定在此时的max _arrive, 进行与第一次相同的循环，直到i和max_pos 重叠。

这个的实质其实是忽略i每次跳的位置（可以在max_arrive确定时确定），只记录跳的次数，进行多次相同的循环操作。

代码如下：

```python
class Solution:
    def jump(self,nums):
        max_pos = 0
        max_arrive = 0
        ans = 0
        for i in range(0,len(nums)-1):
            max_arrive = max(max_arrive,nums[i]+i)
            if i>= max_pos:
                max_pos = max_arrive
                ans = ans + 1
        return  ans
```

17. Leetcode 43字符串数的乘法

如“67”‘89“，不能直接乘

**题解：**

* 按照乘法的规律，乘数为num1，被乘数为num2，先对num2进行遍历，跟num1的第一位相乘，num2中每取一个数，将答案的数组ans往后挪移一位。

  同时对num1进行遍历，每次遍历，ans的起始下标加一。对每个num1元素跟num2的每个元素乘，加到ans的对应的位数。

  ```python
    for i in range(0, len(num1)):
              local = i
              for j in range(0, len(num2)):
                  ans[local] += int(num1[len(num1) - i - 1]) * int(num2[len(num2) - j - 1])
                  local = local + 1
  ```

* 如果ans的某位>10 则说明需要进位：

  ```python
          for i in range(0, local):
              if ans[local - 1] >= 10:
                  local = local + 1
              ans[i + 1] += int(ans[i] / 10)
              ans[i] %=10
  ```

* 判断最高位是否为0

  ```python
          while True:
              if local == 1:
                  break
              if ans[local - 1] == 0:
                  local = local - 1
              else:
                  break
  ```

* 将ans整合为final ans

  ```python
          for i in range(0, local):
              final_ans = final_ans + str(ans[local - i - 1])
  ```

总体的代码如下：

```python
class Solution:
    def multiply(self, num1, num2) :
        ans = [0] * 15000
        local = 0
        for i in range(0, len(num1)):
            local = i
            for j in range(0, len(num2)):
                ans[local] += int(num1[len(num1) - i - 1]) * int(num2[len(num2) - j - 1])
                local = local + 1
        for i in range(0, local):
            if ans[local - 1] >= 10:
                local = local + 1
            ans[i + 1] += int(ans[i] / 10)
            ans[i] %=10
        final_ans = ""
        while True:
            if local == 1:
                break
            if ans[local - 1] == 0:
                local = local - 1
            else:
                break
        for i in range(0, local):
            final_ans = final_ans + str(ans[local - i - 1])
        return final_ans


if __name__ == '__main__':
    a = "12"
    b = "12"
    c = Solution()
    print(Solution().multiply(a, b))
```

18. Leetcode 46 全排列（无元素重复）

用到了**回溯**的算法，这是一种通过探索所有可能的候选解来找出所有的解的算法。如果候选解被确认 不是 一个解的话（或者至少不是 最后一个 解），回溯算法会通过在上一步进行一些变化抛弃该解，即**回溯**并且再次尝试。

一个回溯函数，使用第一个整数的索引作为参数 `backtrack(first)`

如果整数索引为n则return, 并且在一次return后，将做的变化换回（即试错，重新进行其他尝试，在本题中不算试错，本题只是要算得每一种情况）

**题解**

* 如果第一个整数有索引 n，意味着当前排列已完成。
* 遍历索引 first 到索引 n - 1 的所有整数。Iterate over the integers from index first to index n - 1.
  * 在排列中放置第 i 个整数，
    即 swap(nums[first], nums[i]).
  * 继续生成从第 i 个整数开始的所有排列: backtrack(first + 1).
  * 现在回溯，即通过 swap(nums[first], nums[i]) 还原.

回溯法伪代码实现：

```python
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        def backtrack(first = 0):
            # if all integers are used up
            if first == n:  
                output.append(nums[:])
            for i in range(first, n):
                # place i-th integer first 
                # in the current permutation
                nums[first], nums[i] = nums[i], nums[first]
                # use next integers to complete the permutations
                backtrack(first + 1)
                # backtrack
                nums[first], nums[i] = nums[i], nums[first]
        
        n = len(nums)
        output = []
        backtrack()
        return output
```

*本题的思想在解数独中也出现过*

下面是kelo的具体实现：

```python
from typing import List


class Solution:
    ANS = []
    SUB_ASN = []
    set = 0
    tot = 0
    INF = 55

    def dfs(self, nums: List[int], step):
        if step == len(nums):
            self.ANS.append(self.SUB_ASN[:])
            return
        for i in range(0, len(nums)):
            if self.set & (1 << i) == 1 << i:
                continue
            self.SUB_ASN.append(nums[i])
            self.set |= 1 << i
            self.dfs(nums, step + 1)
            self.set ^= 1 << i
            del self.SUB_ASN[-1]

    def permute(self, nums: List[int]):
        self.ANS: List[List[int]]
        self.ANS = []
        self.tot = 0
        self.SUB_ASN = []
        self.dfs(nums, 0)
        temp = []
        for item in self.ANS:
                temp.append(item)
        return temp


if __name__ == '__main__':
    a = [1, 1, 3]
    print(Solution().permute(a))
```

19. leetcode 47 全排列（有元素重复）

只需要在无重复的基础上，将相同的去除即可。

```python
 				for item in self.ANS:
            if item not in temp:
                temp.append(item)
        return temp
```

20. leetcode49 单词的同分异构体

例子：输入 a = ["eat", "tea", "tan", "ate", "nat", "bat"]，将其变为List(List(string))

* "".join(sorted(string)) 

"".join()  : 将字符用“” 串联起来

sorted(string) : 将string元素排序，得到一串字符集合。但是并不会改变原string

* 此题用到哈希表

**代码如下**

```python
from typing import List


class Solution:
    def groupAnagrams(self, strs):
        hash_map = {}
        sort_string = ["".join(sorted(string)) for string in strs]
        for i in range(0, len(sort_string)):
            if sort_string[i] in hash_map:
                hash_map[sort_string[i]].append(strs[i])
            else:
                hash_map[sort_string[i]] = [strs[i]]
        return [x for x in hash_map.values()]


if __name__ == '__main__':
    a = ["eat", "tea", "tan", "ate", "nat", "bat"]
    print(Solution().groupAnagrams(a))
```

Hash_map 是一个字典，key为排序过的str

21. leetcode 44

给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。

'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。

**示例 1:**

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

**示例 2:**

输入:
s = "aa"
p = "*"
输出: true
解释: '*' 可以匹配任意字符串。

**题解**

最普遍的情况是，当p[j]=s[i]，或者为？，*时，如果s[i-1]，p[j-1]也匹配，则p[j]，s[i]匹配。

历遍i j，如果p[-1] 与 s[-1]匹配，则结果为ture.要实现遍历，构建bool类型“二维数组”。

```python
 f = [[False] * (len(p)) for _ in range(len(s))]
# for _ in range(len(s)) 中 _ 表示占位符 不管是什么，反正只要循环就行了
```

实际上，有很多启动元素和特殊情况

* f\[0][0]的情况单独判断

```python
if s[0] == p[0] or p[0] == "*" or p[0] == "?":
	f[0][0] = True
```

* p开头为*, 需要单独判断所有的p与s[0]的匹配情况

```python
if p[0] == "*":
    while len(p) > first and p[first] == "*":
        f[0][first] = True
        first = first + 1
    if len(p) > first and (s[0] == p[first] or p[first] == "?"):
        f[0][first] = True
```

* 接下来就是遍历

```python
  for i in range(0, len(s)):
            for j in range(0, len(p)):
                if p[j] == "*" and i > 0:
                    f[i][j] |= f[i - 1][j]
                if p[j] == "*" and j > 0:
                    f[i][j] |= f[i][j - 1]
                    # 如果p中前一个匹配，则
                if (s[i] == p[j] or p[j] == "?" or p[j] == "*") and i > 0 and j > 0:
                    f[i][j] |= f[i - 1][j - 1]
                    # 当i>0 j>0 当s[i] == p[j] or p[j] == "?" or p[j] == "*" 前一个匹配，则此对匹配
        return f[-1][-1]
```

代码如下

```python
class Solution:
    def isMatch(self, s, p):
        if not s and p:
            for i in range(0, len(p)):
                if p[i] != "*":
                    return False
            return True
        if s and not p:
            return False
        if not s and not p:
            return True
        f = [[False] * (len(p)) for _ in range(len(s))]
        # for _ in range(len(s)) 中 _ 表示占位符 不管是什么，反正只要循环就行了
        if s[0] == p[0] or p[0] == "*" or p[0] == "?":
            f[0][0] = True
        first = 1
        if p[0] == "*":
            while len(p) > first and p[first] == "*":
                f[0][first] = True
                first = first + 1
            if len(p) > first and (s[0] == p[first] or p[first] == "?"):
                f[0][first] = True
            # 这个while 和 这个 if 的意义在于解决那些在p开头的*
        for i in range(0, len(s)):
            for j in range(0, len(p)):
                if p[j] == "*" and i > 0:
                    f[i][j] |= f[i - 1][j]
                if p[j] == "*" and j > 0:
                    f[i][j] |= f[i][j - 1]
                    # 如果p中前一个匹配，则
                if (s[i] == p[j] or p[j] == "?" or p[j] == "*") and i > 0 and j > 0:
                    f[i][j] |= f[i - 1][j - 1]
                    # 当i>0 j>0 当s[i] == p[j] or p[j] == "?" or p[j] == "*" 前一个匹配，则此对匹配
        return f[-1][-1]


if __name__ == '__main__':
    s = "mississippsssssi"
    p = "*sssi"
    print(Solution().isMatch(s, p))

```



