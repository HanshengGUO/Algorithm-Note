# 数组Array和相关考点
数组可能的考点：
* 遍历
* 二分查找
* 双指针
* 前缀和
* 排序
  * 递归
  * 分治
* 滑动窗口

## 数组基础知识
数组：数组是存放在**连续内存空间**上的**相同类型数据**的集合，可以方便地通过下标索引的方式获取到下标下对应的数据。
但同时，因为数组的在内存空间的地址是连续的，所以我们在删除或者增添元素的时候，就难免要移动其他元素的地址。

* 数组不能被删除，只能被覆盖。删除非尾部数据的时候，就需要移动该元素后面的全部元素。
![img.png](../imgs/img.png)
* 二维数组就是数组的数组，C++中地址空间是连续的，而Java中并不连续。

## 数组有关的考点
### 二分查找
二分法的两种写法（用边界位置分类）：
* target 存在在一个在左闭右闭的区间[left, right]里，这时候：
  * while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
  * if (nums[middle] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 middle - 1
* target 存在在一个在左闭右开的区间[left, right)里，这时候：
  * while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
  * if (nums[middle] > target) right 更新为 middle，因为当前nums[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums[middle]
#### 704. 二分查找
使用左闭右闭区间
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right){
            int mid = (right + left) / 2;
            if (nums[mid] > target){
                right = mid - 1;
            }else if (nums[mid] < target){
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```
使用左闭右开区间
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length;
        while (left < right){
            int mid = (left + right) / 2;
            if (nums[mid] == target){
                return mid;
            }else if (nums[mid] > target){
                right = mid;
            }else{
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

### 双指针法
**27. 移除元素**

使用双指针解题
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i = 0, j = 0;
        int n = nums.length;
        while (j < n){
            if (nums[j] != val){
                nums[i] = nums[j];
                i++;
                j++;
            }else{
                j++;
            }
        }
        return i;
    }
}
```
**977. 有序数组的平方**

仍然是使用双指针的经典题目。也可以用栈装起来，总之需要实现FILO。
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int res[] = new int[n];

        int left = 0, right = 0;
        if (nums[n-1] < 0){
            left = n - 1;
            right = n;
        }else if (nums[0] >= 0){
            left = -1;
            right = 0;
        }else{
            for (int i = 0; i<n; i++){
                if (nums[i] < 0 && nums[i+1] >= 0){
                    left = i;
                    right = i+1;
                    break;
                }
            }
        }

        int p = 0;
        while (left >= 0 && right < n){
            if (nums[left] * nums[left] > nums[right] * nums[right]){
                res[p] = nums[right] * nums[right];
                right ++;
            }else{
                res[p] = nums[left] * nums[left];
                left --;
            }
            p++;
        }
        while (left >= 0){
            res[p] = nums[left] * nums[left];
            left --;
            p++;
        }
        while (right < n){
            res[p] = nums[right] * nums[right];
            right ++;
            p++;
        }

        return res;
    }
}
```
**209. 长度最小的子数组**

滑动窗口的经典题目，实现ON复杂度
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int res = n + 1;
        
        int left = 0, right = 0;
        int subSum = nums[left];
        while (left < n){
            while (subSum < target && right < n-1){
                right ++;
                subSum += nums[right];
            }
            if (subSum >= target) res = Math.min(res, right - left + 1);
            subSum -= nums[left];
            left ++;
            if (subSum >= target) res = Math.min(res, right - left + 1);
        }
        if (res == n+1) return 0;
        else return res;
    }
}
```
**59. 螺旋矩阵 II**

此题主要涉及对边界的处理，用模拟的方法解题。
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int res[][] = new int[n][n];
        int direction = 0;
        // 0 -> right
        // 1 -> down
        // 2 -> left
        // 3 -> up
        int[] bound = {n, n, 0, 1};
        int count = 1, x = 0, y = 0;
        while (count <= n * n){
            res[x][y] = count;
            if (direction == 0 && y + 1 >= bound[direction]){
                bound[direction] --;
                direction ++;
            }else if (direction == 1 && x + 1 >= bound[direction]){
                bound[direction] --;
                direction ++;
            }else if (direction == 2 && y - 1 < bound[direction]){
                bound[direction] ++;
                direction ++;
            }else if (direction == 3 && x - 1 < bound[direction]){
                bound[direction] ++;
                direction = 0;
            }
            if (direction == 0)
                y += 1;
            else if (direction == 1)
                x += 1;
            else if (direction == 2)
                y -= 1;
            else if (direction == 3)
                x -= 1;

            count ++;
        }
        return res;
    }
}
```