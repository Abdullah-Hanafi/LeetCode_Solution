# [剑指 Offer 03]数组中重复的数字

## 思路

看到“重复”两个字，很容易想起Java的哈希表（Set）的特性：无序、不可重复。

思路很简单，对数组进行遍历，如果Set中已经存储有该元素则直接返回。



### 算法流程

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

#### 算法流程

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



#### 算法流程

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





# 剑指 Offer 05. 替换空格

## 思路

模拟：遍历目标字符串，遇到字符为' '直接替换即可



### 算法流程

1.创建一个StringBuilder的变量answer用于保存最终的返回值

2.线性遍历目标字符串s的每个字符c

​    1.若c是空格，则调用append方法加入"%20"

​    2.若c不是空格，则调用append方法加入c

3.返回answer.toString();



## 代码实现

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuffer answer = new StringBuffer();
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == ' '){
                answer.append("%20");
            }else{
                answer.append(s.charAt(i));
            }
        }
        return answer.toString();
    }
}
```



## 复杂度分析

时间复杂度：O(n)，其中n为s.length(),StringBuilder的append方法为O(1)，故而时间复杂度为O(n*1)=O(n)

空间复杂度：开辟StringBuilder类型的变量answer的空间为O(n)

 

## 思考

为什么不用String直接进行拼接，而要用StringBulider进行拼接

因为String是不可变的字符串，所以进行拼接的时候不是在原字符串上进行操作，其实际操作流程为：

1. 先将原字符串new为StringBuilder，

2. 调用StringBuilder的append方法进行拼接

3. 返回toString()

以如下代码为例，说明String的拼接过程

 ```java
 public class Main {
     public static void main(String[] args) {
         String str1 = "John";
         String str2 = "Doe";
         System.out.println(str1 + str2);
     }
 
 }
 ```

1. 调用javac Main.java，将java类编译为字节码.class

2. 调用javap -c Main.class，反编译字节码查看其细节内容，如下所示

```java
Compiled from "Main.java"
public class Main {
  public Main();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: ldc           #2                  // String John
       2: astore_1
       3: ldc           #3                  // String Doe
       5: astore_2
       6: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
       9: new           #5                  // class java/lang/StringBuilder
      12: dup
      13: invokespecial #6                  // Method java/lang/StringBuilder."<init>":()V
      16: aload_1
      17: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      20: aload_2
      21: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      24: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      27: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      30: return
}

```



3. 其中第9行new，代表new一个StringBuilder的变量

4. 其中第17行invokevirtual代表调用了StringBuilder的append方法加入John
5. 其中第21行invokevirtual代表调用了StringBuilder的append方法加入Doe

5. 翻译为StringBuilder的Java代码如下所示

 ```java
 public class Main {
     public static void main(String[] args) {
         String str1 = "John";
         String str2 = "Doe";
         System.out.println(new StringBuilder().append(str1).append(str2).toString());
     }
 }
 ```



综上，String的拼接过程其底层是由StringBuilder的append方法实现的，字符串频繁拼接的时候，需要不断的重复new、append、toString这一过程，显然开销要比StringBuilder的append进行字符串拼接的开销更大



# 剑指 Offer 06. 从尾到头打印链表

## 方法1：栈

### 思路

题目要求从后到前返回每个节点的值，而单链表只支持从头到尾进行遍历。利用栈后入先出的特性，可以很好的解决这一问题。

#### 算法流程

1. 边界考虑，若链表为空，直接返回长度为0的数组

2. 创建一个栈stack，用于保存链表中每个节点值

3. 线性遍历每个链表节点，将链表节点值加入到栈中

4. 创建一个大小为栈大小stack.size()的数组res

5. 若栈非空

   将栈中的值不断弹出存入数组res中

6. 返回数组res

### 代码实现

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        if(head == null){
            return new int[0];
        }
        Deque<Integer> stack = new LinkedList<>();
        while(head!=null){
            stack.push(head.val);
            head = head.next;
        }
        int[] res = new int[stack.size()];
        int index = 0;
        while(!stack.isEmpty()){
            res[index++] = stack.pop();
        }
        return res;
    }
}
```



### 复杂度分析

时间复杂度：设链表的长度为n，线性遍历一次链表的时间复杂度为O(n)，遍历栈的时间复杂度为O(n)，故总的时间复杂度为O(n)+O(n)=O(n)

空间复杂度：开辟了一块大小为n的栈，故而空间复杂度为O(n)



## 方法2：递归

### 思路

可以利用递归先走到链表的最末端，在回溯的阶段访问当前节点值，即可实现链表的反向遍历

#### 算法流程

1. 创建一个ArrayList类型的类变量list用于存储反向遍历的链表节点值
2. 边界判断，如果链表为空，则直接返回长度为0的数组
3. 调用递归函数
   1. 递归阶段：若当前节点值为null则表明已经越过为节点，直接返回，否则继续向head.next移动
   2. 回溯阶段：层层回溯时，将当前节点值加入列表list
4. 递归回溯完成以后，将list转换为数组返回即可

### 代码实现

```java
class Solution {
    List<Integer> list = new ArrayList<>();

    public int[] reversePrint(ListNode head) {
        if (head == null) {
            return new int[0];
        }
        recur(head);
        int[] res = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    public void recur(ListNode node) {
        //递归出口
        if (node == null) return;
        //递归阶段
        recur(node.next);
        //回溯阶段
        list.add(node.val);
    }
}
```



### 复杂度分析

时间复杂度：设链表的长度为n，将列表转换为数组的时间复杂度为O(n)，递归遍历链表的时间复杂度为O(n)，总的时间复杂度为O(n)+O(n)=O(n)

空间复杂度：递归栈需要开辟一块长度为n的空间，其空间复杂度为O(n)，列表list的空间复杂度也为O(n)，总的空间复杂度为O(n)+O(n)=O(n)



# 剑指 Offer 07. 重建二叉树

## 思路

前序遍历的结果为【根节点｜左子树前序遍历序列｜右子树前序遍历序列】

中序遍历的结果为【左子树中序遍历序列｜根节点｜右子树中序遍历序列】

利用左子树的前序遍历序列和左子树的中序遍历序列可以重建左子树，即重建二叉树是可以划分出子问题的，可以采用分治的算法，即

1. 创建根节点
2. 重建二叉树的左子树
3. 重建二叉树的右子树

设前序遍历序列的下标范围为[pBegin,pEnd]，中序遍历序列的下标范围为[iBegin,iEnd]

所以问题转化为以下几个问题

1. 如何创建根节点

   前序遍历的第一个节点即为二叉树的根节点

2. 定位左子树的前序遍历序列

   左子树的前序序列的左边界容易确定，即pBegin+1

   左子树的前序序列的右边界需要知道左子树的长度lLength，即pBegin+lLength，lLength可以通过根节点在中序遍历中的位置position求出

3. 确定根节点在中序遍历序列中的位置position和左子树的长度

   建立一个哈希表，存储节点值到中序遍历序列下标的映射。如中序遍历结果[9,3,15,20,7]，节点9的下标为0，节点3的下标为1。我们确定了根节点，即可查哈希表查出该节点在中序遍历中的下标，左子树的长度lLength=position-iBegin

4. 定位左子树的中序遍历序列

   左子树的中序序列的左边界为iBegin

   左子树的中序序列的右边界为position-1

5. 定位右子树的前序遍历序列

   右子树的前序序列的左边界为左子树前序序列的右边界+1，即pBegin+lLength+1

   右子树的前序序列的右边界为pEnd

6. 定位右子树的中序遍历序列

   右子树的中序序列的左边界为position+1

   右子树的中序序列的右边界为iEnd

### 算法流程

主方法：

1. 边界考虑，如果传入的数组为空，或者数组长度为0，直接返回null
2. 创建一个哈希表pos的类变量用于存储节点值到中序序列下标的映射关系
3. 调用helper方法完成二叉树的重建

helper方法定义：利用分治法，传入二叉树的前序序列、中序序列构造二叉树，前序序列左右边界、中序序列左右边界等参数，返回值为根节点

1. 出口，如果前序序列的左边界pBegin大于pEnd，说明越过了叶子结点，直接返回null即可
2. 通过计算根节点在中序遍历序列的位置position
3. 利用preorder[pBegin]创建根节点
4. 计算左子树的长度ILength=position-iBegin
5. 调用helper生成左子树
6. 调用helper生成右子树
7. 返回根节点

## 代码实现

```java
class Solution {
    Map<Integer, Integer> pos = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            pos.put(inorder[i], i);
        }
        TreeNode root = helper(preorder,0,preorder.length-1,inorder,0,inorder.length-1);
        return root;
    }

    //构造树的辅助函数
    public TreeNode helper(int[] preorder, int pBegin, int pEnd, int[] inorder, int iBegin, int iEnd) {
        //出口
        if (pBegin > pEnd) {
            return null;
        }
        int position = pos.get(preorder[pBegin]);
        TreeNode root = new TreeNode(preorder[pBegin]);
        int lLength = position-iBegin;
        root.left = helper(preorder, pBegin + 1, pBegin +lLength, inorder, iBegin, position - 1);
        root.right = helper(preorder, pBegin + lLength+1, pEnd, inorder, position + 1, iEnd);
        return root;
    }
}
```



## 复杂度分析

设二叉树的节点数位n

时间复杂度：主方法中线性遍历二叉树的中序遍历序列的时间复杂度为O(n)，helper方法中需要创建n个节点搜索n个节点，其时间复杂度也为O(n)，故而总的时间复杂度为O(n)+O(n)=O(n)

空间复杂度：HashMap的空间复杂度为O(n)，递归深度平均情况下为O(logn)，最差情况下，当输入的二叉树为单链表时，递归深度为O(n)，故而总的空间复杂度为O(n)+O(logn)=O(n)
