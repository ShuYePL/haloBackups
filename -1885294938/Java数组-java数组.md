---
title: Java数组
date: 2021-11-11 16:54:26.0
updated: 2022-11-04 20:56:35.542
url: /archives/java数组
categories: 
- Javase
tags: 
---


#### 文章简介：记录一下java数组的相关内容，方便日后对相关内容进行温习。
<!--more-->

# 一维数组
#### 一维数组的创建：

#### 第一种创建方式（静态初始化）：
代码示例 ：`数组数据类型[] 数组名 = {数组元素1, 数组元素2, 数组元素3, ...};`
例如：`int[] nums = {1, 2, 3, 4, 5};`
这里创建了一个含有元素1，2，3，4，5的一维整型数组。每个元素都可以通过数组名和元素下标的方式来获取。（下面会有相关内容的讲解） 

#### 第二种创建方式（动态初始化）：
代码示例：`数组数据类型[] 数组名 = new 数组数据类型[数组元素个数]; `  
例如：`int[] nums2 = new int[5];`  
此时，这里创建了一个包含5个元素的整型数组。
注意：使用这种方式创建数组的时候，每个数组元素的值都是默认的值，如int类型的默认值是0，所以该数组中的每个元素都是0，即数组中存储了5个0，之后我们需要对数组的元素进行赋值操作才能存储我们想要的数据。

#### 数组的下标：
了解完一维数组的创建之后，接下来我们来了解一些和数组有关的小内容，如数组的下标。  
数组的下标是对数组元素的一个排序，就像是现实中的排队一样，数组的每一个元素都有自己的一个编号，从左到右依次为：0, 1, 2, 3, 4, 5, ...  
例如`int[] nums = {1, 2, 3, 4, 5};`，对于nums数组而言，数组中有五个元素，其元素与下标的对应关系如下：  

![202111004](http://img.shuyepl.com/202207042002485.png)

现在下标和元素的关系我们已经知道了，那么我们就可以对数组的元素进行我们想要的操作了。

#### 获取一维数组元素的值：  
代码示例：`数组名[元素下标]`  
例子（这里顺便将得到的值进行打印输出）：  
~~~java
int[] nums = {1, 2, 3, 4, 5};       // 静态初始化一维数组
for(int i = 0; i < 5; i++){         // for循环对数组的元素进行遍历输出
    System.out.print(nums[i] + " ");
}
System.out.println();               // 换行   
String[] str = new String[5];       // 动态初始化一维数组，初始化后数组元素的值统一为默认值
for(int i = 0; i < 5; i++){         // for循环对数组的元素进行遍历输出
    System.out.print(str[i] + " ");
}
~~~
输出结果：  

~~~test
1 2 3 4 5  
null null null null null   
~~~

#### 修改一维数组中元素的值：  
代码示例：`数组名[元素下标] = 修改后元素的值;`
例子（这里只给出main方法中的代码）：  
~~~java
int[] nums = {1, 2, 3, 4, 5};
System.out.println(nums[2]);     //这里取出了nums数组中下标为2的元素，即整型3
~~~
输出结果：

~~~test
3
~~~


PS:用这个代码就可以修改上面‘数组的创建’中，用动态初始化方法创建的一维数组元素的值。

#### 获取一维数组元素的个数：
可以通过数组的length属性获取数组元素的个数。
代码示例：`数组名.length`
例子（这里只给出main方法中的代码）：

~~~java
int[] nums = {1, 2, 3, 4, 5, 6};
System.out.println(nums.length);
~~~
输出结果：

~~~test
6
~~~

#### 一维数组的遍历操作：
例子：
~~~java
int[] nums = {1, 2, 3, 4, 5, 6, 7};
for(int i = 0; i < nums.length; i++){
    System.out.print(nums[i] + " ");
}
~~~
输出结果：

~~~test
1 2 3 4 5 6 7 
~~~

#### 一维数组的扩容与拷贝：
在java中，数组的扩容是通过将小容量数组中元素，拷贝到大容量数组中的方式实现的，所以，在学习数组的扩容之前，我们需要知道怎么对java中的数组进行拷贝。  
数组的拷贝有两种方式：
#### 第一种（手动拷贝）：
先创建一个大容量的数组，将小容量数组中的元素一个个搬到大数组中去。
例子 ：
~~~java
int[] src = {1, 2, 3};                  // 原来小容量的数组
int[] dest = new int[10];               // 创建一个大容量的数组
for(int i = 0; i < src.length; i++){    // 打印查看拷贝前小容量数组的元素值
    System.out.print(src[i] + " ");
}
System.out.println();                   // 换行
for(int i = 0; i < dest.length; i++) {  // 打印查看拷贝前大容量数组的元素值
    System.out.print(dest[i] + " ");
}
System.out.println();                   // 换行
for(int i = 0; i < src.length; i++){    // 通过for循环将小容量数组中的元素一个个搬到大容量数组中
    dest[i] = src[i];
}
for(int i = 0; i < src.length; i++){    // 打印查看拷贝后小容量数组的元素值
    System.out.print(src[i] + " ");
}
System.out.println();                   // 换行
for(int i = 0; i < dest.length; i++){   // 打印查看拷贝后大容量数组的元素值
    System.out.print(dest[i] + " ");
}
~~~
输出结果：  

~~~test
1 2 3   
0 0 0 0 0 0 0 0 0 0   
1 2 3   
1 2 3 0 0 0 0 0 0 0  
~~~

在这个例子中，我们在拷贝的同时还实现了对数组的扩容。

#### 第二种方式（使用System类的arraycopy方法）：    
例子（只放出main方法中的代码）：  
~~~java
int[] src = {1, 2, 3, 4, 5};
int[] dest = new int[20];
System.arraycopy(src, 0, dest, 3, 4);
for(int i = 0; i < dest.length; i++)
{
    System.out.print(dest[i] + " ");
}
~~~
输出结果： 

~~~test
0 0 0 1 2 3 4 0 0 0
~~~


arraycopy方法的源码：

~~~java
public static native void arraycopy(Object src,  int  srcPos,
                                  Object dest, int destPos,
                                  int length);
~~~
从源码中可以看出，该方法有五个参数，从左到右分别是要复制的数组，复制起点的下标，要复制到的数组，复制到的位置的下标，以及复制的长度。  
在这个例子中可以看到，使用arraycopy方法可以轻松地对数组进行扩容操作。  

#### 在方法调用的参数列表中创建一维数组的一种方式：
代码示例：`方法名(new 数组类型[]{元素1, 元素2, 元素3, ...})`  
例子：
~~~java
public static void main(String[] args) {
    printArray(new int[]{1, 2, 3, 4});          // 调用静态输出方法
}
public static void printArray(int[] array){     // 创建一个输出一维数组元素的静态方法
    for(int i = 0; i < array.length; i++){
        System.out.print(array[i] + " ");
    }
}
~~~
输出结果：

~~~test
1 2 3 4 
~~~

#### 对一维数组中的元素进行排序：

选择排序：选择排序算法的思路是将数组元素按从小到大（或从大到小）的顺序排列在数组的左边，与冒泡排序算法相比其优点是元素交换位置的频率较低，减少了一些无意义的交换。
代码示例：

```java
public class Array16 {
    public static void main(String[] args) {
        int[] arr = {3, 2, 4, 7, 1};
        for (int i = 0; i < arr.length - 1; i++) {
            int min = i;    // 假定下标为i的元素是最小的元素，并将下标记录下来
            for (int j = i + 1; j < arr.length; j++) {
                if(arr[j] < arr[min]){  // 如果发现比下标为min更小的元素，把该元素下标记录为最小元素的下标
                    min = j;
                }
            }
            if(min != i){   //如果下标为i的元素并不是最小的元素，说明我们的假设失败，将最小的元素依次交换到左边
                int temp = arr[min];
                arr[min] = arr[i];
                arr[i] = temp;
            }
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

输出结果：

~~~test
1 2 3 4 7 
~~~

冒泡排序：冒泡排序的排序步骤就和它听上去的差不多，将数组中的元素按从大到小的顺序依次一个一个像冒泡泡一样排到数组的最右边（下标最大的地方）。  
代码示例：

~~~java
public class Array15 {
    public static void main(String[] args) {
        int arr[] = {3, 5, 23, 14, 12};
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                if(arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
~~~

输出结果：

~~~test
3 5 12 14 23
~~~

当然，在Java中已经有现成的方法可以实现数组排序的功能了，在开发中我们只需要调用相应的方法即可。   

使用Arrays工具类对数组进行排序：

代码示例：

~~~java
import java.util.Arrays;    // 引入Arrays工具类
public class Array14 {
    public static void main(String[] args) {
        int[] arr = {2, 3, 4, 9, 78, 23, 228, 98};
        Arrays.sort(arr);   // Array工具类中的sort方法是静态的方法，可以直接调用
        for (int i = 0; i < arr.length; i++) {  // 遍历输出数组的元素查看排序的结果
            System.out.print(arr[i] + " ");
        }
    }
}
~~~

输出结果：

~~~test
2 3 4 9 23 78 98 228 
~~~

#### 查找数组中的元素：

查找数组中元素的方式有直接硬找的方式也有用二分法进行查找的方式。  
直接查找的方式基本上对数组元素的排序没有什么特别的要求，只要是一个数组，它就能在数组里查找相应的元素，但缺点是需要对数组中的每一个元素进行比对，查找的效率较低。  
下面的例子是硬找的方式：  

~~~java
public class ArraySearch {
    public static void main(String[] args) {
        int[] arr = {1, 2, 4, 5, 6};
        System.out.println(indexSearch(arr, 4));
        System.out.println(indexSearch(arr, 9));
    }

    /**
     * 这个方法用来查找数组中是否含有自己想要查找的元素
     * @param arr       是我们想在其中查找对应元素的数组
     * @param element   是我们想在数组中查找的元素
     * @return          大于等于0的返回值表示我们想查找的元素在对应数组中的下标，-1的返回值表示没有找到相应的元素
     */
    public static int indexSearch(int[] arr, int element){
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == element) {
                return i;
            }
        }
        return -1;
    }
}
~~~

输出结果： 

~~~test
2
-1
~~~

二分法查找的好处是查找的速度整体上相对于硬找要快一些，因为它并不需要对每一个元素都进行比较，但缺点是对查找的数组有一定的要求，数组中的元素要有一定的顺序，所以，从某种意义上来讲，二分法查找只是针对于数字的查找。
下面是一个二分法查找的例子：

~~~java
public class ArraySearch2 {
    public static void main(String[] args) {
        int[] nums = {1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024};
        int result = binarySearch2(nums, 3);
        if(result >= 0){
            System.out.println("目标元素3在数组中的下标为" + result);
        }else{
            System.out.println("在该数组中没有找到目标元素3");
        }
        // 顺便复习下三目运算符
        System.out.println(binarySearch2(nums, 4) >= 0 ?
                "目标元素4在数组中的下标为" + binarySearch2(nums, 4) : "在该数组中没有找到目标元素4");
    }

    /**
     * 静态方法，查找目标元素在数组中是否存在，并返回元素下标
     * @param arr 待查找目标元素的数组，要提前排好序，从小排到大
     * @param target 待查找的目标元素
     * @return  大于等于0的返回值是目标元素的下标，-1表示在该数组中没有目标元素
     */
    public static int binarySearch2(int[] arr, int target){
        // 记录数组第一个元素的下标和最后一个元素的下标
        int arrHead = 0;
        int arrEnd = arr.length - 1;
        // 只要待查找数组的头下标小于或等于尾下标
        while(arrHead <= arrEnd){
            // 得到中间元素的下标
            int mid = (arrHead + arrEnd) / 2;
            // 如果中间的元素与我们要查找的元素一样，返回此时的mid，即相应元素的下标
            if(arr[mid] == target) {
                return mid;
            }else if(arr[mid] < target){    // 如果此时，中间元素比目标元素小，则目标元素可能在中间元素的右边
                arrHead = mid + 1;           // 头下标变为中间元素的下标加1，此时，待查找的区域减少了一半
            }else{                          // 到这里，目标元素只有可能比中间元素小，在中间元素的左边
                 arrEnd = mid - 1;          // 尾下标变为中间元素的下标减1，此时，待查找区域减少了一半
            }
        }
        return -1;  // 如果上面的循环能跳出，则说明没有找到要查找的目标元素，返回-1
    }
}
~~~

输出结果：

~~~test
在该数组中没有找到目标元素3
目标元素4在数组中的下标为2
~~~



# 二维数组

在了解二维数组之前我们有必要先了解一下行与列的知识点。 
#### 行与列：我们一般将横向的数据排列称为行，竖向的数据排列称为列。（如下图所示）
![202111005](http://img.shuyepl.com/202207042002455.png)

假设上图中的1，2，3，4，5，6，7，8，9是二维数组中的元素，对于其中的任一元素我们都可以用几行几列的形式来描述它的位置。
如对于其中的5这一个元素，我们可以说它位于1行，第1列的位置，同样的，4这一元素的位置可以描述为第0行，第1列...后面的大家以此类推。

#### 二维数组的创建：
#### 第一种创建方式（静态初始化）：
代码示例：
~~~java
数组数据类型[][] nums = {
  {元素1, 元素2, ...},
  {元素3, 元素4, ...},
  ...
}
~~~
#### 第二种创建方式（动态初始化）：
例子：
~~~java
int[][] nums = new int[2][3];
for (int i = 0; i < nums.length; i++) {
    for (int j = 0; j < nums[i].length; j++) {
        System.out.print(nums[i][j] + " ");
    }
    System.out.println();
}
~~~
输出结果： 

~~~test
0 0 0  
0 0 0  
~~~

或者使用下面这种方式  
例子：

~~~java
int[][] nums = new int[2][];                    // 动态初始化一个有两行的二维数组
nums[0] = new int[3];                           // 动态初始化第0行的元素个数为3
nums[1] = new int[7];                           // 动态初始化第1行的元素个数为7
for(int i = 0; i < nums.length; i++){           // 外层for循环挑选二维数组的每一行
    for(int j = 0; j < nums[i].length; j++){    // 内层for循环输出i行的每个元素
        System.out.print(nums[i][j] + " ");
    }
    System.out.println();                       // 换行
}
~~~
输出结果：  

~~~test
0 0 0 
0 0 0 0 0 0 0  
~~~

Tip：使用这种方法初始化一维数组时，每一行数组的列数可以不同。

#### 获取二维数组的行数和列数：

#### 行数的获取：

代码示例：`数组名.length`  
例子：

~~~java
int[][] nums = {
        {1, 2, 3},
        {1, 3, 4, 8, 90},
        {7, 9, 10, 12},
};
System.out.println(nums.length);
~~~
输出结果：

~~~test
3
~~~

#### 某一行列数的获取：
代码示例：`数组名[某一行的下标].length`  
例子：

~~~java
int[][] nums = {
        {1, 2, 3},
        {1, 3, 4, 8, 90},
        {7, 9, 10, 12},
};
System.out.println(nums[1].length);
~~~
输出结果：

~~~test
5
~~~

#### 小总结：  
可以把二维数组看成是特殊的一维数组，这个一维数组中的元素是一些一维数组，所以，它里面有多少个一维数组就有几行，而里面每个一维数组的元素数量，又是它所在那一行的列数。
PS：在java中二维数组每一行的列数可以不相等。

#### 二维数组元素的获取
代码示例：`数组名[第几行][第几列]`  
上面这个示例表示取出第几行第几列的元素。  
例子：

~~~java
int[][] nums = {
        {1, 5},
        {4, 7}
};
System.out.println(nums[0][1]);
~~~
输出结果：

~~~test
5
~~~

#### 二维数组元素值的修改
代码示例：`数组名[第几行][第几列] = 想要赋的值`  
上面这个示例表示将相应的值赋给第几行第几列的元素。  
例子：

~~~java
int[][] nums = {
        {1, 5},
        {4, 7}
};
nums[0][1] = 10;
System.out.println(nums[0][1]);
~~~
输出结果：

~~~test
10
~~~

#### 二维数组的遍历操作：
例子：
~~~java
String[][] vegetables = {                           // 创建二维数组
        {"大白菜", "韭菜", "芹菜"},
        {"番茄", "土豆", "包菜", "菠菜"},
        {"生菜", "菜花", "荷兰豆"}
};
for(int i = 0; i < vegetables.length; i++){         // 外层的for循环用来选中数组中的每一行
    for(int j = 0; j < vegetables[i].length; j++){  // 内层的for循环用来选中行内的每一列
        System.out.print(vegetables[i][j] + " ");   // 输出i行j列的元素
    }
    System.out.println();                           // 换行
}
~~~
输出结果：  

~~~test
大白菜 韭菜 芹菜   
番茄 土豆 包菜 菠菜   
生菜 菜花 荷兰豆 
~~~