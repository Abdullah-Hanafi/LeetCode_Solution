# [剑指 Offer 03]数组中重复的数字

## 思路

看到“重复”两个字，很容易想起Java的哈希表（Set）的特性：无序、不可重复。

思路很简单，对数组进行遍历，如果Set中已经存储有该元素则直接返回。



## 算法流程

1. 创建一个哈希表set
2. 线性遍历数组nums中的每一个元素num
   1. num在set中，直接返回num
   2. num不在set中，需要将num加入到set
3. 因为题目保证有重复数字，所以在没有遍历完所有元素时候方法就会结束。为了保证语法正确，最后可以return -1即可。



## 代码实现

```java
class Solution{
  	public int findRepeatNumber(int[] nums){
      	Set<Integer> set = new HashSet<>();
      	for(int num : nums){
          	if(set.contains(num)){
              	return num;
            }
          	set.add(num);
        }
      	return -1;
    }
}
```



## 复杂度分析

时间复杂度：线性遍历每一个num的时间复杂度为O(N)，检查set中是否存在num即set中加入num的时间复杂度为O(1)，即时间复杂度为O(N*1)=O(N)

空间复杂度：set占用O(N)的额外空间，即空间复杂度为O(N)



# [剑指 Offer 04]二维数组中的查找

## 方法1：暴力模拟

### 思路

因为本题没有对时间复杂度有要求，可以采用暴力模拟，但是其没有使用题中所给数组的特性：每一行从左至右依次增加，每一列从上至下依次增加。所以暴力模拟肯定不是最优解。

### 算法流程

1. 边界判断，如果matrix为null，或者matrix的长度为0，直接返回false
2. 按行遍历二维数组matrix的每一个元素，如果和target相同则直接返回true
3. 若正常遍历结束则说明数组中无该元素，直接返回false

### 代码实现

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        //边界判断一下
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                if (matrix[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    } 
}
```



### 复杂度分析

时间复杂度：对于m行n列的二维数组，事件复杂度为O(m*n)

空间复杂度：没有开辟辅助空间，故空间复杂度为O(1)



## 方法2：线性遍历+二分查找

### 思路

一个m✖️n的二维数组是由m个长度为n的一维数组构成的，由题目可知，每个一维数组是有序的，即可以在每个一维数组matrix[i]中用二分查找的方法查找target是否存在，调用m次二分查找即可查出target是否存在于二维数组中。



### 算法流程

主方法：

1. 边界判断，如果matrix为null，或matrix.length==0，直接返回false
2. 定义变量m为matrix的长度
3. 对matrix[i]（0<= i <= m）进行遍历，调用二分查找find方法，查找目标元素target是否存在于matrix[i]中，若存在则返回true
4. 正常遍历结束则说明目标元素target不在数组中，返回false

二分查找方法：

1. 定义left为一维数组nums的左边界，right为一维数组nums的右边界，区间为左闭右闭，即在nums[left,...,right]中查找目标元素target
2. 当left<=right时
   1. 定义中间元素的下标mid=left + (right-left)/2
   2. 若nums[mid] == target，返回true
   3. 若nums[mid] > target，则在左区间进行查找，即right = mid - 1
   4. 若nums[mid] < target，则在右区间进行查找，即left = mid + 1
3. 循环正常退出说明目标元素target不在数组中，返回false



### 代码实现

```java
class Solution {
    //主方法
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        //边界判断一下
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        int m = matrix.length;
        for (int i = 0; i < m; i++){
            if(find(matrix[i],target)){
                return true;
            }
        }
        return false;
    }
    //二分查找
    public boolean find(int[] nums, int target){
        int left = 0;
        int right = nums.length - 1;
        //左闭右闭
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums[mid] == target){
                return true;
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return false;
    }
}
```



### 复杂度分析

时间复杂度：线性遍历m个一维数组，时间复杂度为O(m)，遍历每个一维数组时候调用二分查找方法的时间复杂度为O(logn)，即总的时间复杂度为O(mlogn)

空间复杂度：虽然二分查找循环内部每次都创建了int型变量mid，但是用完即回收，故其空间复杂度为常数级，即O(1)



## 方法3：类比二叉搜索树



