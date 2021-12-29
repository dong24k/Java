**数据结构的分类**

1. 线性结构

   1. 线性结构作为最常用的数据结构，其特点是数据**元素之间存在一对一**应的线性关系
   2. 线性结构有两种不同的存储结构，即**顺序存储结构（数组）**和**链式存储结构（链表）**，顺序存储的线性表称为顺序表，顺序表中的**存储元素是连续**的
   3. 链式存储的线性表称为链表，链表中的**存储元素不一定是连续的**，元素节点中存放数据元素以及相邻元素的地址信息。
   4. 线性结构常见的有：**数据，队列，链表和栈**

2. 非线性结构

   非线性结构包括：二维数组，多维数组，广义表，树结构，图结构。

# 1. 稀疏数组和队列

![image-20200725203318246](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI1MjAzMzE4MjQ2LnBuZw?x-oss-process=image/format,png)

1. 编写的五子棋程序中，有存盘退出和续上盘的要求
2. 分析问题：因为该二维数组的很多值是默认值0，因此记录了很多没有意义的数据——>**稀疏数组**（可以达到一个压缩的效果）。

## 1.1 稀疏数组的基本介绍

当一个数组中大部分元素为0，或者为用一个值的数组时，可以使用稀疏数组来保存该数组

稀疏数组的处理方法是：

1. 记录数组一共几行几列，有多少不同的值
2. 把具有不同值得元素得行列及值记录在一个小规模得数组中，从而缩小程序的规模。

![image-20200725203635616](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI1MjAzNjM1NjE2LnBuZw?x-oss-process=image/format,png)

1. 稀疏数组中的第一行（[0]）表示的是：第一个值表示的是原始数组有多少行，第二个值表示的是原始数组有多少列。第三个值原始数组有多少个值（非0）

2. [1]至[8]表示的是对应行列的值

![image-20200725211316748](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI1MjExMzE2NzQ4LnBuZw?x-oss-process=image/format,png)

## 1.2 代码复现

```java
package DataStructures;

public class SparseArray {
    public static void main(String[] args) {
        //创建一个原始的而二维数组11*11
        //0：表示没有棋子，1表示黑色2表示蓝色
        int chessArray1[][] = new int[11][11];
        chessArray1[1][2] = 1;
        chessArray1[2][3] = 2;
        //输出原始的二维数组
        System.out.println("原始的二维数组");
        for (int[] row : chessArray1){
            for (int data : row){
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }
//        将二维数组转成稀疏数组的思路
//        1.先遍历二维数组 得到非0数据的个数
        int sum = 0;
        for (int i = 0;i < 11;i++){
            for (int j =0;j < 11;j++){
                if (chessArray1[i][j] != 0){
                    sum++;
                }
            }
        }
//        2.创建对应的稀疏数组
        int sparseArr[][] = new int[sum+1][3];
//        给稀疏数组赋值
        sparseArr[0][0] = 11;
        sparseArr[0][1] = 11;
        sparseArr[0][2] = sum;
//        遍历二维数组，将非0的值存放到 sparseArr中
        int count = 0;
        for (int i = 0;i < 11;i++){
            for (int j =0;j < 11;j++){
                if (chessArray1[i][j] != 0){
                    count++;
                    sparseArr[count][0] = i;
                    sparseArr[count][1] = j;
                    sparseArr[count][2] = chessArray1[i][j];
                }
            }
        }
//        输出稀疏数组的形式
        System.out.println();
        System.out.println("得到稀疏数组~~~~~~");
        for (int i = 0;i < sparseArr.length; i++){
            System.out.printf("%d\t%d\t%d\t\n",sparseArr[i][0],sparseArr[i][1],sparseArr[i][2]);
        }
        System.out.println();
//        将稀疏数组转换为原始的二维数组
        /*
        * 1.在读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数组，
        * 2.在读取稀疏数组后几行的数据，并赋给原始的二维数组即可
        * */
//        先读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数据
        int chessArr2[][] = new int[sparseArr[0][0]][sparseArr[0][1]];
        for (int i = 1;i < sparseArr.length;i++){
            chessArr2[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
        }
//        输出恢复后的二维数组
        System.out.println();
        System.out.println("恢复后的二维数组");

        for (int[] row:chessArr2){
            for (int data:row){
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }
    }
}
```

输出的结果如下：

```java
原始的二维数组
0	0	0	0	0	0	0	0	0	0	0	
0	0	1	0	0	0	0	0	0	0	0	
0	0	0	2	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	

得到稀疏数组~~~~~~
11	11	2	
1	2	1	
2	3	2	


恢复后的二维数组
0	0	0	0	0	0	0	0	0	0	0	
0	0	1	0	0	0	0	0	0	0	0	
0	0	0	2	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0	
0	0	0	0	0	0	0	0	0	0	0
```



## 1.3 队列的基础知识

![image-20200713133427876](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzEzMTMzNDI3ODc2LnBuZw?x-oss-process=image/format,png)

1. 队列是一个有序的列表，可以用**数组**或是**链表**来实现
2. 遵循**先入先出**的原则，即：先存入队列的数据，要先取出，后存入的要后取出

## 1.4 队列的实现方式

**数组模拟队列**

1. 队列本身是有序列表，若使用数组的结构来存储队列的数据，则队列数组的声明如下图，其中maxSize是该队列的最大容量
2. 因为队列的输出，输入是分别从前后端来处理，因此需要两个变量**front**和**rear**分别标记队列的前后端，front会随着数据输出而改变，而rear则是随着数据输入改变而改变，如图所示：

![image-20200713133502315](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzEzMTMzNTAyMzE1LnBuZw?x-oss-process=image/format,png)

3. 将尾指针往后移，rear+1
4. 当front == rear，队列为空
5. 若尾指针rear小于队列的最大下标maxSize - 1，则将数据存入rear所指的数组元素中，否则无法存入数据。
6. 队列满的时候：rear == maxSize - 1

程序如下：

```java
package DataStructures;

import java.util.Scanner;

public class arrayQueueDemo {
    public static void main(String[] args) {
        //测试一下，创建一个队列
        ArrayQueue queue =new ArrayQueue(3);
        char key = ' ';//接收用户输入
        Scanner scanner =new Scanner(System.in);
        boolean loop = true;
        //输出一个菜单
        while (loop){
            System.out.println("s(show)：显示队列");
            System.out.println("e(exit)：退出程序");
            System.out.println("a(add)：添加数据到队列");
            System.out.println("g(get)：从队列取出数据");
            System.out.println("h(head)：查看列头的数据");
            key = scanner.next().charAt(0);//接收一个字符
            switch (key){
                case 's':
                    queue.showQueue();
                    break;
                case 'a':
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    queue.addQueue(value);
                    break;
                case 'g'://取出数据
                    try{
                        int res = queue.getQueue();
                        System.out.printf("取出的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h'://查看队列头的数据
                    try{
                        int res = queue.headQueue();
                        System.out.printf("队列头的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出了~~~");
    }
}

//使用数组模拟队列编写一个ArrayQueue类
class ArrayQueue{
    private int maxSize;//表示数组的最大容量
    private int front;//队列头
    private int rear;//队列尾
    private int[] arr;//该数据用于存放数据，模拟队列
//    创建队列的构造器
    public ArrayQueue(int arrMaxiSize){
        maxSize = arrMaxiSize;
        arr = new int[maxSize];
        front = -1;//指向队列头部，分析出front是指向队列头的前一个位置。
        rear = -1;//指向队列尾，指向队列的数组（即就是队列最后一个数据）
    }
//    判断队列是否满
    public boolean isFull(){
        return rear == maxSize - 1;
    }
//    判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }
//    添加数据到队列
    public void addQueue(int n){
//        判断队列是否为空
        if (isFull()){
            System.out.println("队列满，不能加入数据~");
            return;
        }
        rear++;
        arr[rear] = n;
    }
//    获取队列的数据，出队列
    public int getQueue(){
//        判断队列是否空
        if (isEmpty()){
//            通过抛出异常
            throw new RuntimeException("队列空，不能取数据");
//            此时不可以写return，因为throw本身就是会返回值，不会再去执行下面的操作代码了
        }
        front++;
        return arr[front];
    }
//    显示队列的所有数据
    public void showQueue(){
        if (isEmpty()){
            System.out.println("队列为空，没有数据~~~");
            return;
        }
        for (int i = 0;i < arr.length; i++){
            System.out.printf("arr[%d]=%d\n",i,arr[i]);
        }
    }
//    显示队列的头部数据，注意不是取出数据
    public int headQueue(){
        //判断
        if (isEmpty()){
            throw new RuntimeException("队列为空，没有数据~~~");
        }
        return arr[front + 1];
    }
}
```







程序的部分运行结果如下：

```java
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
10
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
20
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
30
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
s
arr[0]=10
arr[1]=20
arr[2]=30
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
e
程序退出了~~~
```

队列的缺陷：

1. 目前数组使用一次就不能用了，没有达到复用的效果
2. 将这个数据使用算法，改进成一个环形的队列

## 1.5 数组模拟环形队列

对于前面的数组模拟队列的优化，充方利用数组，因此将数组看成是一个环形的（通过取模的方法实现即可）

**分析说明**

1. 尾索引的下一个为头索引时表示队列满，即将**队列容量空出一个作为约定**，这个在做判断队列满的时候需要注意（rear + 1）% maxSize == front
2. rear == front 队列为空

![image-20200725211335689](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI1MjExMzM1Njg5LnBuZw?x-oss-process=image/format,png)

## 1.6 代码复现

代码实现

```java
package DataStructures;

import java.util.Scanner;

public class circleArrayQueue {
    public static void main(String[] args) {
        //测试一下
        System.out.println("测试数组模拟环形队列的案例");

        //测试一下，创建一个环形队列
        CircleArray queue =new CircleArray(4);//设置为4，其队列的有效数据最大是3
        char key = ' ';//接收用户输入
        Scanner scanner =new Scanner(System.in);
        boolean loop = true;
        //输出一个菜单
        while (loop){
            System.out.println("s(show)：显示队列");
            System.out.println("e(exit)：退出程序");
            System.out.println("a(add)：添加数据到队列");
            System.out.println("g(get)：从队列取出数据");
            System.out.println("h(head)：查看列头的数据");
            key = scanner.next().charAt(0);//接收一个字符
            switch (key){
                case 's':
                    queue.showQueue();
                    break;
                case 'a':
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    queue.addQueue(value);
                    break;
                case 'g'://取出数据
                    try{
                        int res = queue.getQueue();
                        System.out.printf("取出的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h'://查看队列头的数据
                    try{
                        int res = queue.headQueue();
                        System.out.printf("队列头的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出了~~~");


    }
}

class CircleArray{
    private int maxSize;//表示数组的最大容量
    private int front;//front指向队列的第一个元素，也就是说arr[front]使元素的第一个值，front的初始值等于0
    private int rear;//rear指向队列的最后一个元素的后一个位置，希望空出一个空间作为约定。rear的初始值为0
    private int[] arr;//该数据用于存放数据，模拟队列

    public CircleArray(int arrMaxSize){
        maxSize = arrMaxSize;
        arr = new int[maxSize];
    }

    //判断队列是否满
    public boolean isFull(){
        return (rear + 1) % maxSize == front;
    }

    //    判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }


    //    添加数据到队列
    public void addQueue(int n){
    //    判断队列是否为空
        if (isFull()){
            System.out.println("队列满，不能加入数据~");
            return;
        }
        //直接将数据加入
        arr[rear] = n;
        //将rear后移，这里必须考虑取模，因为当在最后的位置的时候，它需要往前面的空位置走
        rear = (rear + 1) % maxSize;
    }


    //    获取队列的数据，出队列
    public int getQueue(){
//        判断队列是否空
        if (isEmpty()){
//            通过抛出异常
            throw new RuntimeException("队列空，不能取数据");
//            此时不可以写return，因为throw本身就是会返回值，不会再去执行下面的操作代码了
        }
        /*
        * 这里需要分析出front是指向队列的第一个元素
        *1. 先把front 对应的值保留到一个临时变量（如果不保存的话将会直接返回，这样的话就无法后移了）
        * 2. 将front后移，考虑取模
        * 3. 将临时保存的变量返回
        * */
        int value = arr[front];
        front = (front + 1) % maxSize;
        return value;
    }


    //    显示队列的所有数据
    public void showQueue(){
        if (isEmpty()){
            System.out.println("队列为空，没有数据~~~");
            return;
        }
        //这里需要思考！！！！
        for (int i = front;i < front + size() ; i++){
            System.out.printf("arr[%d]=%d\n",i % maxSize,arr[i % maxSize]);
        }
    }

    //求出当前队列有效数据的个数
    public int size() {
        return (rear + maxSize - front) % maxSize;
    }


    //    显示队列的头部数据，注意不是取出数据
    public int headQueue(){
        //判断
        if (isEmpty()){
            throw new RuntimeException("队列为空，没有数据~~~");
        }
        return arr[front];
    }
}
```

结果的实现

```java
测试数组模拟环形队列的案例
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
10
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
20
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
30
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
40
队列满，不能加入数据~
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
g
取出的数据是10
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
a
输出一个数
40
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
s
arr[1]=20
arr[2]=30
arr[3]=40
s(show)：显示队列
e(exit)：退出程序
a(add)：添加数据到队列
g(get)：从队列取出数据
h(head)：查看列头的数据
```

## 1.7 如何更好的理解环形队列

1. 我们首先需要明白，什么是取模？

**取模的意义在于循环中找到本来的循环位置，不会根据次数的叠加而叠加，会在每次循环中找到顺序中本来的位置**

2. 为什么会预留一个空位置，作为约定。

**个人觉得是因为这样写代码会更加的方便，假如不预留一个空位置的话，对于rear的处理会比较的复杂些。**

3. 整个编程过程中，比较重要的是一段代码就是判断出有效数字

```
(rear + maxSize - front) % maxSize;
```

**需要注意的是，rear一般都是搭配maxSize使用的，因为rear会出现在front前面，但其实此时的rear已经循环了一圈，所以需要加上maxSize，其实我们将这个公式分开来写会更加的直观**

```javascript
(rear + maxSize) % maxSize - (front % maxSize)
```

我们需要思考的代码还有：

```
System.out.printf("arr[%d]=%d\n",i % maxSize,arr[i % maxSize]);
```

**理解取模之后 i % maxSize的含义就会明白的很透彻了**

# 2. 链表

## 2.1 链表原理介绍

链表是有序的列表，他在内存中的存储如下：

![image-20200713192621025](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzEzMTkyNjIxMDI1LnBuZw?x-oss-process=image/format,png)

1. 链表是以节点的方式存储
2. 每个节点包含data域，next域：指向下一个节点
3. 如图：发现链表的各个节点不一定是连续存储
4. 链表分带头节点的链表和没有头节点的链表，根据实际的需求来确定

单链表（带头结点）逻辑结构示意图：**最后指向null表示链表结束**

![image-20200713192624809](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzEzMTkyNjI0ODA5LnBuZw?x-oss-process=image/format,png)

## 2.2 **代码实现**

1. 这个并没有考虑顺序，直接添加到链表的尾部。

![image-20200725211512478](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI1MjExNTEyNDc4LnBuZw?x-oss-process=image/format,png)



```java
package DataStructures;

public class SingleLinkedListDemo {
    public static void main(String[] args) {
        //进行测试
        //先创建节点
        HeroNode hero1 = new HeroNode(1,"松江","及时雨");
        HeroNode hero2 = new HeroNode(2,"卢俊义","玉麒麟");
        HeroNode hero3 = new HeroNode(3,"吴用","智多星");
        HeroNode hero4 = new HeroNode(4,"林冲","豹子头");

        //创建链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        //加入数据
        singleLinkedList.add(hero1);
        singleLinkedList.add(hero2);
        singleLinkedList.add(hero3);
        singleLinkedList.add(hero4);
        //显示一下
        singleLinkedList.list();


    }
}

//定义SingleLinkedList管理我们的英雄
class SingleLinkedList{
    //先初始化一个头节点，头节点不动，不存放具体的数据
    private HeroNode head = new HeroNode(0,"","");

    //添加节点到单向链表
    /*
    * 思路：当不考虑编号顺序时
    * 1. 找到当前链表的最后节点
    * 2. 将最后这个节点的next 指向新的节点
    * */
    public void add(HeroNode heroNode){
        //因为head节点不能动，因此我们需要一个遍历的temp
        HeroNode temp = head;
        //遍历链表，找到最后
        while(true){
            //找到链表的最后
            if (temp.next == null){
                break;
            }
            //如果没有找到最后，将temp后移
            temp = temp.next;
        }
        //当退出while循环时，temp就指向了链表的最后
        //将最后这个节点的next 指向新的节点
        temp.next = heroNode;
    }

    //显示链表【遍历】
    public void list(){
        //判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head.next;
        while (true){
            //判断是否到链表的最后
            if (temp == null){
                break;
            }
            //输出节点信息
            System.out.println(temp);
            //将temp后移，一定小心
            temp = temp.next;
        }
    }


}

//定义HeroNode, 每个HeroNode对象就是一个节点
class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next; //指向下一个节点
    //构造器
    public HeroNode(int no, String name,String nickname){
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }
    //为了显示方法，我们重新toString
    @Override
    public String toString() {
        return "HeroNode [no=" + no + ", name=" + name + ", nickename=" + nickname + "]";
    }
}
```

**运行结果**

```java
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
```

2. 第二种方式，根据排名将英雄插入到指定位置，（如果有这个排名，则添加失败，并给出提示）

![image-20200726111924129](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI2MTExOTI0MTI5LnBuZw?x-oss-process=image/format,png)

**代码实现**

```java
package DataStructures;

public class SingleLinkedListDemo {
    public static void main(String[] args) {
        //进行测试
        //先创建节点
        HeroNode hero1 = new HeroNode(1,"松江","及时雨");
        HeroNode hero2 = new HeroNode(2,"卢俊义","玉麒麟");
        HeroNode hero3 = new HeroNode(3,"吴用","智多星");
        HeroNode hero4 = new HeroNode(4,"林冲","豹子头");

        //创建链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        //加入数据
//        singleLinkedList.add(hero1);
//        singleLinkedList.add(hero4);
//        singleLinkedList.add(hero3);
//        singleLinkedList.add(hero2);
        singleLinkedList.addByOrder(hero1);
        singleLinkedList.addByOrder(hero4);
        singleLinkedList.addByOrder(hero3);
        singleLinkedList.addByOrder(hero2);
        //显示一下
        singleLinkedList.list();


    }
}

//定义SingleLinkedList管理我们的英雄
class SingleLinkedList{
    //先初始化一个头节点，头节点不动，不存放具体的数据
    private HeroNode head = new HeroNode(0,"","");

    //添加节点到单向链表
    /*
    * 思路：当不考虑编号顺序时
    * 1. 找到当前链表的最后节点
    * 2. 将最后这个节点的next 指向新的节点
    * */
    public void add(HeroNode heroNode){
        //因为head节点不能动，因此我们需要一个遍历的temp
        HeroNode temp = head;
        //遍历链表，找到最后
        while(true){
            //找到链表的最后
            if (temp.next == null){
                break;
            }
            //如果没有找到最后，将temp后移
            temp = temp.next;
        }
        //当退出while循环时，temp就指向了链表的最后
        //将最后这个节点的next 指向新的节点
        temp.next = heroNode;
    }

    public void addByOrder(HeroNode heroNode){
        //因为指针节点不能动，因此我们仍然通过一个辅助指针（变量）来帮助找到添加的位置
        //因为是单链表，我们找的temp 是位于添加位置的前一个节点，否则插入失败
        HeroNode temp = head;
        boolean flag = false;//flag 标志添加的编号是否存在，默认为false
        while (true){
            if (temp.next == null){
                //说明temp已经在链表的最后
                break;
            }
            if (temp.next.no > heroNode.no){
                //位置找到，就在temp的后面插入
                break;
            }else if (temp.next.no == heroNode.no){
                //说明希望添加的heroNode的编号已经存在
                flag = true;//说明编号已存在
                break;
            }
            temp = temp.next;//后移，遍历当前链表
        }
        //判断flag的值
        if (flag){
            //不能添加说明编号存在
            System.out.printf("准备插入的英雄的编号 %d 已经存在了，不能加入\n",heroNode.no);
        }else {
            heroNode.next = temp.next;
            temp.next = heroNode;
        }

    }

    //显示链表【遍历】
    public void list(){
        //判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head.next;
        while (true){
            //判断是否到链表的最后
            if (temp == null){
                break;
            }
            //输出节点信息
            System.out.println(temp);
            //将temp后移，一定小心
            temp = temp.next;
        }
    }


}

//定义HeroNode, 每个HeroNode对象就是一个节点
class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next; //指向下一个节点
    //构造器
    public HeroNode(int no, String name,String nickname){
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }
    //为了显示方法，我们重新toString
    @Override
    public String toString() {
        return "HeroNode [no=" + no + ", name=" + name + ", nickename=" + nickname + "]";
    }
}
```

**运行结果**

```java
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
```

3. **修改链表的数据**

```java
    //修改节点的信息，根据no编号来修改，即no编号不嫩改
    //说明：根据newHeroNode的no来修改即可
    public void update(HeroNode newHeroNode){
        //判断为空
        if (head.next == null){
            System.out.println("链表为空~~~");
            return;
        }
        //找到需要修改的节点，根据no编号
        //定义一个辅助变量
        HeroNode temp = head.next;
        boolean flag = false;//表示是否找到该节点
        while(true){
            if (temp == null){
                break;//已经遍历完链表
            }
            if (temp.no == newHeroNode.no){
                //找到
                flag = true;
                break;
            }
            temp = temp.next;
        }
        //根据flag 判断是否找到想要修改的节点
        if (flag){
            temp.name = newHeroNode.name;
            temp.nickname = newHeroNode.nickname;
        }else {
            //没有找到
            System.out.printf("没有找到编号 %d 的节点，不能修改\n",newHeroNode.no);
        }
    }
```

**运行结果**

```java
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
修改后的链表情况~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=小卢, nickename=玉麒麟~~]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
```

4. **删除链表的节点**

```java
 //删除节点
    /*
    * 1. head不能栋，因此我们需要一个temp辅助节点找到待删除节点的前一个节点
    * 2. 说明我们在比较时，是temp.next.no和需要删除的节点的no 比较
    * */
    public void dal(int no){
        HeroNode temp = head;
        boolean flag = false; //标志是否找到待删除的节点
        while (true){
            if (temp.next == null){
                //已经到了链表的最后
                break;
            }
            if (temp.next.no == no){
                //找到的待删除节点的前一个节点temp
                flag = true;
                break;
            }
            temp = temp.next; //temp后移，遍历
        }
        //判断flag
        if (flag){
            //找到，可以删除
            temp.next = temp.next.next;
        }else{
            System.out.printf("要删除的 %d 几点不存在\n",no);
        }

    }
```

**运行结果**

```java
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
修改后的链表情况~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=小卢, nickename=玉麒麟~~]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
删除后的链表情况~~~~
HeroNode [no=2, name=小卢, nickename=玉麒麟~~]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
```

## 2.3 面试题

1. **求单链表中有效节点的个数**
2. **查找单链表中的倒数第k个节点【新浪面试题】**
3. **单链表的反转【腾讯面试题】**
4. **从尾到头打印单链表【百度，要求方式1：反向遍历。方式2：Stack栈】**
5. **合并两个有序的单链表，合并之后依然有序【课后练习】**

### 2.3.1 第一题

代码的实现：

```java
//方法：获取到单链表的节点的个数（如果是带头节点的链表，不需要统计头节点

    public static int getLength(HeroNode head){
        if (head.next == null){
            return 0;
        }
        int length = 0;n
        //定义一个辅助变量，这里我们没有统计头节点
        HeroNode cur = head.next;
        while(cur != null){
            length++;
            cur = cur.next;
        }
        return length;
    }
```

因为头节点是私有的，所以我们需要自己重新写一个方法来实现：

```java
//这是一个私有属性，我们需要写一个返回方法
    public HeroNode getHead(){
        return head;
    }
```

运行的结果如下：

```java
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
修改后的链表情况~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=小卢, nickename=玉麒麟~~]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
删除后的链表情况~~~~
HeroNode [no=2, name=小卢, nickename=玉麒麟~~]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
有效的节点个数=3
```

### 2.3.2 第二道题

代码：

```java
//查找单链表的倒数第k个节点
    /*
    * 思路：
    * 1. 编写一个方法，接受head节点，同时接收一个index
    * 2. index表示是倒数第index个节点
    * 3. 先把链表从头到尾遍历，得到链表的总的长度getLength
    * 4. 得到size 后，我们从链表的第一个开始遍历（size-index）个，就可以得到
    * 5. 如果找到了，则返回该节点，否则返回null
    * */
    public static HeroNode findLastIndexNode(HeroNode head, int index){
        //判断如果链表为空，返回null
        if (head.next == null){
            return null;
        }
        //第一个遍历得到的链表的长度（节点个数）
        int size = getLength(head);
        //第二次遍历 size - index 位置，就是我们倒数第k个节点
        //先做一个index的校验
        if (index <= 0 || index > size){
            return null;
        }
        //定义给辅助变量，for 循环定位到倒数Index
        HeroNode cur = head.next;
        for (int i = 0;i < size - index; i++){
            cur = cur.next;
        }
        return cur;
    }
```

运行的结果是：

```java
删除后的链表情况~~~~
HeroNode [no=2, name=小卢, nickename=玉麒麟~~]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
有效的节点个数=3
res=HeroNode [no=4, name=林冲, nickename=豹子头]
```

### 2.3.3 第三道题

```java
//将单链反转
    public static void reverseList(HeroNode head){
        //如果当前链表为空，或者只有一个节点，无需反转，直接返回
        if (head.next == null || head.next.next == null){
            return;
        }

        //定义一个辅助的指针（变量），帮助我们遍历原来的链表
        HeroNode cur = head.next;
        HeroNode next = null; //指向当前节点[cur]的下一个节点
        HeroNode reverseHead = new HeroNode(0,"","");

        //遍历原来的链表，每遍历一个节点，就将其取出，并放在新的链表reversrHead的最前端
        while (cur != null){
            next = cur.next; //先暂时保存当前节点的下一个节点，因为后面需要使用
            cur.next = reverseHead.next; //将cur的下一个节点指向新的链表的最前端
            reverseHead.next = cur;  //将cur连接到新的链表上
            cur = next; //让cur后移
        }

        //将head.next 指向reverseHead.next ，实现单链表的反转
        head.next = reverseHead.next;
    }
```

结果：

```java
原来的链表情况~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=4, name=林冲, nickename=豹子头]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
反转单链表~~~
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
HeroNode [no=1, name=松江, nickename=及时雨]
```

**代码分析**

1.cur：在原始表中的当前节点，这个节点要转移到新的reverseHead链表中。

2.next：next是cur的下一个节点，它存在的意义就是让cur知道自己接下来**要搞哪个节点**，因为本身的cur会被转移走。

### 2.3.4 第四题

代码实现：

```
//使用方式 2：可以利用这个数据结构，将各个节点压入栈中，然后利用栈的先进后出的特点，就实现了逆序打印的效果
    public static void reversePrint(HeroNode head){
        if (head.next == null){
            return;//空链表，不能打印
        }
        //创建一个栈，将各个节点压入栈
        Stack<HeroNode> stack = new Stack<HeroNode>();
        HeroNode cur = head.next;
        //将链表的所有节点压入栈
        while(cur != null){
            stack.push(cur);
            cur = cur.next; //cur后移，这样就可以压入下一个节点
        }
        //将栈中的节点进行打印，pop出栈
        while(stack.size() > 0){
            System.out.println(stack.pop());
        }
    }
```

结果显示：

```
原来的链表情况~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=4, name=林冲, nickename=豹子头]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
测试逆序打印单链表,没有改变链表的结构~~~
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
HeroNode [no=1, name=松江, nickename=及时雨]
```

**代码分析**

就是使用**栈**这个高级的方法。其实代码一般般吧。

## 2.4 双向链表

单向链表的缺点分析：

1. 单向链表，查找的方向只能是一个方向，而双向链表可以向前或者向后。
2. 单向链表不能自我删除，需要靠辅助节点，而双向节点链表，则可以自我删除，所以前面我们单链表删除节点，总是找到temp的下一个节点来删除的（认真体会）。

**增删改代码**

```java
package DataStructures;

public class DoubleLinkedListDemo {
    public static void main(String[] args) {
        //进行测试
        System.out.println("双向链表的测试");
        //先创建节点
        HeroNode2 hero1 = new HeroNode2(1, "松江", "及时雨");
        HeroNode2 hero2 = new HeroNode2(2, "卢俊义", "玉麒麟");
        HeroNode2 hero3 = new HeroNode2(3, "吴用", "智多星");
        HeroNode2 hero4 = new HeroNode2(4, "林冲", "豹子头");
        //创建一个双向链表
        DoubleLinkedList doubleLinkedList = new DoubleLinkedList();
        doubleLinkedList.add(hero1);
        doubleLinkedList.add(hero2);
        doubleLinkedList.add(hero3);
        doubleLinkedList.add(hero4);

        doubleLinkedList.list();

        //修改
        HeroNode2 newHeroNode = new HeroNode2(4,"公孙胜","入云龙");
        doubleLinkedList.update(newHeroNode);
        System.out.println("修改后的链表情况~~~");
        doubleLinkedList.list();

        //删除
        doubleLinkedList.dal(3);
        System.out.println("删除后的链表情况~~~");
        doubleLinkedList.list();

    }
}


class DoubleLinkedList{
    //先初始化一个头节点，头节点不动，不存放具体的数据
    private HeroNode2 head = new HeroNode2(0,"","");

    //这是一个私有属性，我们需要写一个返回方法
    public HeroNode2 getHead(){
        return head;
    }

    //显示链表【遍历】
    public void list(){
        //判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode2 temp = head.next;
        while (true){
            //判断是否到链表的最后
            if (temp == null){
                break;
            }
            //输出节点信息
            System.out.println(temp);
            //将temp后移，一定小心
            temp = temp.next;
        }
    }


    //添加一个节点到双向链表的最后
    public void add(HeroNode2 heroNode){
        //因为head节点不能动，因此我们需要一个遍历的temp
        HeroNode2 temp = head;
        //遍历链表，找到最后
        while(true){
            //找到链表的最后
            if (temp.next == null){
                break;
            }
            //如果没有找到最后，将temp后移
            temp = temp.next;
        }
        //当退出while循环时，temp就指向了链表的最后
        //形成一个双向链表
        temp.next = heroNode;
        heroNode.pre = temp;

    }


    //修改一个节点的内容，可以看到双向链表的节点内容和单向链表一样
    public void update(HeroNode2 newHeroNode){
        //判断为空
        if (head.next == null){
            System.out.println("链表为空~~~");
            return;
        }
        //找到需要修改的节点，根据no编号
        //定义一个辅助变量
        HeroNode2 temp = head.next;
        boolean flag = false;//表示是否找到该节点
        while(true){
            if (temp == null){
                break;//已经遍历完链表
            }
            if (temp.no == newHeroNode.no){
                //找到
                flag = true;
                break;
            }
            temp = temp.next;
        }
        //根据flag 判断是否找到想要修改的节点
        if (flag){
            temp.name = newHeroNode.name;
            temp.nickname = newHeroNode.nickname;
        }else {
            //没有找到
            System.out.printf("没有找到编号 %d 的节点，不能修改\n",newHeroNode.no);
        }
    }


    //从一个双向链表中删除一个节点
    /*
    * 1. 对于双向链表，我i们可以直接找到要删除的这个节点
    * 2. 找到后，自我删除即可
    * */
    public void dal(int no){

        //判断当前链表是否为空
        if (head.next == null){
            System.out.println("链表为空，无法删除~~~");
            return;
        }

        HeroNode2 temp = head.next;
        boolean flag = false; //标志是否找到待删除的节点
        while (true){
            if (temp == null){
                //已经到了链表的最后节点的next
                break;
            }
            if (temp.no == no){
                //找到的待删除节点的前一个节点temp
                flag = true;
                break;
            }
            temp = temp.next; //temp后移，遍历
        }
        //判断flag
        if (flag){
            //找到，可以删除
            temp.pre.next = temp.next;
            //这里需要注意：如果是最后一个节点，就不需要执行下面这句话，否者会出现空指针异常，null.pre
            if (temp.next != null){
                temp.next.pre = temp.pre;
            }
        }else{
            System.out.printf("要删除的 %d 几点不存在\n",no);
        }

    }


}


//定义HeroNode, 每个HeroNode对象就是一个节点
class HeroNode2{
    public int no;
    public String name;
    public String nickname;
    public HeroNode2 next; //指向下一个节点，默认为null
    public HeroNode2 pre; //指向前一个节点，默认为null
    //构造器
    public HeroNode2(int no, String name,String nickname){
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }
    //为了显示方法，我们重新toString
    @Override
    public String toString() {
        return "HeroNode [no=" + no + ", name=" + name + ", nickename=" + nickname + "]";
    }
}
```

**运行结果**

```
双向链表的测试
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
修改后的链表情况~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=公孙胜, nickename=入云龙]
删除后的链表情况~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=4, name=公孙胜, nickename=入云龙]
```

**顺序添加的代码**

```
public void addByOrder(HeroNode2 heroNode){
        //因为指针节点不能动，因此我们仍然通过一个辅助指针（变量）来帮助找到添加的位置
        //因为是单链表，我们找的temp 是位于添加位置的前一个节点，否则插入失败
        HeroNode2 temp = head;
        boolean flag = false;//flag 标志添加的编号是否存在，默认为false
        while (true){
            if (temp.next == null){
                //说明temp已经在链表的最后
                break;
            }
            if (temp.next.no > heroNode.no){
                //位置找到，就在temp的后面插入
                break;
            }else if (temp.next.no == heroNode.no){
                //说明希望添加的heroNode的编号已经存在
                flag = true;//说明编号已存在
                break;
            }
            temp = temp.next;//后移，遍历当前链表
        }
        //判断flag的值
        if (flag){
            //不能添加说明编号存在
            System.out.printf("准备插入的英雄的编号 %d 已经存在了，不能加入\n",heroNode.no);
        }else {
            temp.next = heroNode;
            heroNode.pre = temp;
        }

    }
```

**运行的结果**

```java
双向链表的测试
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=林冲, nickename=豹子头]
修改后的链表情况~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=公孙胜, nickename=入云龙]
删除后的链表情况~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=4, name=公孙胜, nickename=入云龙]
顺序添加~~~
HeroNode [no=1, name=松江, nickename=及时雨]
HeroNode [no=2, name=卢俊义, nickename=玉麒麟]
HeroNode [no=3, name=吴用, nickename=智多星]
HeroNode [no=4, name=公孙胜, nickename=入云龙]
```

**注意事项：**

1. 添加需要记住的代码是：

```java
temp.next = heroNode;
heroNode.pre = temp;
```

2. 删除需要记住的代码是：

```java
temp.pre.next = temp.next;
if (temp.next != null){
temp.next.pre = temp.pre;
}
```

## 2.5 单向环形链表

**Josephu问题**

![img](https://img2020.cnblogs.com/blog/2123988/202010/2123988-20201014193740329-643771036.png)

设编号为1，2，… n的n个人围坐一圈，约定编号为k（1<=k<=n）的人从1开始报数，数到 m 的那个人出列，它的下一位又从1开始报数，数到 m 的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。

**算法的实现**

![img](https://img2020.cnblogs.com/blog/2123988/202010/2123988-20201014194850785-757438155.png)

用一个不带头结点的循环链表来处理Josephu 问题：先构成一个有n个结点的单循环链表，然后由k结点起从1开始计数，计到m时，对应结点从链表中删除，然后再从被删除结点的下一个结点又从1开始计数，直到最后一个结点从链表中删除算法结束。

![image-20200726113301153](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI2MTEzMzAxMTUzLnBuZw?x-oss-process=image/format,png)



**代码的实现**

```
package DataStructures;

public class Josephu {
    public static void main(String[] args) {
        //测试一下
        CircleSingleLinkedList circleSingleLinkedList = new CircleSingleLinkedList();
        circleSingleLinkedList.addBoy(5);
        circleSingleLinkedList.showBoy();

    }
}

//创建一个环形的单向链表
class CircleSingleLinkedList {
    //创建一个first节点，当前没有编号
    private Boy first = new Boy(-1);
    //添加小孩节点，构建一个环形的链表
    public void addBoy(int nums){
        //nums做一个数据校验
        if (nums < 1){
            System.out.println("nums的值不正确");
            return;
        }
        Boy curBoy = null; //辅助指针，帮助构建环形链表
        //使用for循环来创建我们的环形链表
        for (int i = 1;i <= nums; i++){
            //根据编号，创建小孩节点
            Boy boy = new Boy(i);
            //如果是第一个小孩
            if (i == 1){
                first = boy;
                first.setNext(first); //构成环形
                curBoy = first;//让curBoy指向第一个小孩
            }else {
                curBoy.setNext(boy);
                boy.setNext(first);
                curBoy = boy;
            }
        }
    }
    //遍历当前环形链表
    public void showBoy(){
        //判断链表是否为空
        if (first == null){
            System.out.println("没有任何小孩~~~");
            return;
        }
        //因为first不能动，因此我们仍然使用一个辅助指针完成遍历
        Boy curBoy = first;
        while(true){
            System.out.printf("小孩的编号 %d \n",curBoy.getNo());
            if (curBoy.getNext() == first){
                //说明遍历已经完毕
                break;
            }
            curBoy = curBoy.getNext();//curboy后移
        }
    }
}



//创建一个Boy类，表示一个节点
class Boy{
    private int no;//编号
    private Boy next;//指向下一个节点，默认为null
    public Boy(int no){
        this.no = no;
    }
    public int getNo() {
        return no;
    }
    public void setNo(int no){
        this.no = no;
    }
    public Boy getNext(){
        return next;
    }
    public void setNext(Boy next){
        this.next = next;
    }
}
```

**运行结果**

```
小孩的编号 1 
小孩的编号 2 
小孩的编号 3 
小孩的编号 4 
小孩的编号 5 
```

![image-20200726113448880](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzI2MTEzNDQ4ODgwLnBuZw?x-oss-process=image/format,png)

**小孩出圈的顺序代码**

```java
//根据用户的输入，计算出几个小孩的出圈顺序
    public void countBoy(int startNo,int countNum,int nums){
        //先对数据进行校验
        if (first == null || startNo < 1 || startNo > nums){
            System.out.println("参数输入有误，请重新输入~~~");
            return;
        }
        //创建要给辅助指针，帮助小孩出圈
        Boy helper = first;
        //需要创建一个辅助指针（变量）helper，事先应该指向环形链表的最后这个节点
        while (true){
            if (helper.getNext() == first){
                //说明helper指向最后小孩节点
                break;
            }
            helper = helper.getNext();
        }
        //小孩报数前，先让first 和 helper 移动 k-1 次
        for (int j = 0; j < startNo - 1; j++){
            first = first.getNext();
            helper = helper.getNext();
        }
        //当小孩报数时，让first 和 helper 移动 m-1 次，然后出圈
        //这是一个循环操作，知道圈中只有一个节点
        while (true){
            if (helper == first){
                //说明圈中只有一个节点
                break;
            }
            //让first 和 helper 移动 countNum - 1 次
            for (int j = 0;j < countNum -1;j++ ){
                first = first.getNext();
                helper = helper.getNext();
            }
            //这个时候first指向的节点，就是要出圈的小孩节点
            System.out.printf("小孩 %d 出圈\n",first.getNo());
            //这时将first指向的小孩节点出圈
            first = first.getNext();
            helper.setNext(first);
        }
        System.out.printf("最后留在圈中的小孩编号 %d \n",first.getNo());
    }
```

**结果的运行**

```
小孩 2 出圈
小孩 4 出圈
小孩 1 出圈
小孩 5 出圈
最后留在圈中的小孩编号3 
```



# 3. 栈



**基础知识**

1. 栈的英文是stack
2. 栈是一个**先入后出**的有序列表
3. 栈是限制线性表中元素的插入和删除**只能在线性表的同一端**进行的一种特殊线性表，允许插入和删除的一端，为变化的一端，称为栈顶（top），另一端为固定的一端，称为栈底（bottom）。
4. 根据栈的定义，最先放入栈中元素的在栈底，最后放入的元素在栈顶，而删除元素刚好相反，最后放入的元素最先删除，最先放入的元素最后删除。

**出栈和入栈的概念示意图**

![image-20200714141818548](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwNzE0MTQxODE4NTQ4LnBuZw?x-oss-process=image/format,png)

**栈的应用场景**

1. 子程序的调用：在跳往子程序之前，先将下一个指令的地址存到堆栈中，直到子程序执行完后再将地址取出，以回到原来的程序中。
2. 处理递归调用：和子程序的调用类似，只是除了存储下一个指令的地址外，也将参数，区域变量等数据存入堆栈中。
3. 表达式的转换[中缀表达式转后缀表达式]和求值（实际解决）
4. 二叉树的遍历
5. 图形的深度优先（depth-first）搜索法。

## 3.1 数组模拟栈的使用

实现栈的思路分析

1. 使用数组来模拟栈
2. 定义一个top来表示栈顶，初始化为-1
3. 入栈的操作，当数据加到栈时，top++；stack[top] = data;
4. 出栈的操作，int value=stack[top];top--;return value;

**代码实现**

```java
package DataStructures;
import java.util.Scanner;

import java.io.StringReader;
import java.util.Scanner;

public class ArrayStackDemo {
    public static void main(String[] args) {
        //测试一下
        ArrayStack stack = new ArrayStack(4);
        String key = "";
        boolean loop = true;//控制是否退出菜单
        Scanner scanner = new Scanner(System.in);
        while (loop) {
            System.out.println("show: 表示显示栈");
            System.out.println("exit: 退出程序");
            System.out.println("push: 表示添加数据到栈（入栈）");
            System.out.println("pop: 表示从栈取出数据（出栈）");
            System.out.println("请输入你的选择");
            key = scanner.next();
            switch (key) {
                case "show":
                    stack.list();
                    break;
                case "push":
                    System.out.println("请输入一个数");
                    int value = scanner.nextInt();
                    stack.push(value);
                    break;
                case "pop":
                    try {
                        int res = stack.pop();
                        System.out.printf("出栈的数据是 %d\n", res);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case "exit":
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("拜了个拜~~~~");

    }
}


//定义一个ArrayStack表示栈
class ArrayStack {
    private int maxSize;//栈的大小
    private int[] stack;//数组，数组模拟栈，数据就放入到数组里面去
    private int top = -1;//top表示栈顶，初始化为-1

    //构造器
    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //栈满
    public boolean isFull() {
        return top == maxSize - 1;
    }

    //栈空
    public boolean isEmpty() {
        return top == -1;
    }

    //入栈
    public void push(int value) {
        //判断栈是否满
        if (isFull()) {
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    //出栈-pop，将栈顶的数据返回
    public int pop() {
        //先判断栈是否空
        if (isEmpty()) {
            //抛出异常
            throw new RuntimeException("栈空，没有数据~~~");
            //这里不需要return
        }
        int value = stack[top];
        top--;
        return value;
    }

    //显示栈的情况【遍历栈】，遍历时需要从栈顶开始显示数据
    public void list() {
        if (isEmpty()) {
            System.out.println("栈空，没有数据~~~");
            return;
        }
        //需要从栈顶开始显示数据
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, stack[i]);
        }
    }
}
```

**结果显示**

```
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
show
栈空，没有数据~~~
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
push
请输入一个数
10
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
push
请输入一个数
20
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
show
stack[1]=20
stack[0]=10
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
pop
出栈的数据是 20
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
show
stack[0]=10
show: 表示显示栈
exit: 退出程序
push: 表示添加数据到栈（入栈）
pop: 表示从栈取出数据（出栈）
请输入你的选择
```

## 3.2 栈实现综合计算器

```java
package DataStructures;

public class Calculator {
    public static void main(String[] args) {
        //根据前面老师思路，完成表达式的运算
        String expression = "3+2*6-2";
        //创建两个栈，数栈，一个符号栈
        ArrayStack2 numStack = new ArrayStack2(10);
        ArrayStack2 operStack = new ArrayStack2(10);
        //定义需要的相关变量
        int index = 0;
        int num1 = 0;
        int num2 = 0;
        int oper = 0;
        int res = 0;
        char ch = ' ';//将每次扫描得到char保存到ch
        //开始while循环的扫描expression
        while (true) {
            //依次得到expression 的每一个字符
            ch = expression.substring(index, index + 1).charAt(0);
            //判断ch是什么，然后做相应的处理
            if (operStack.isOper(ch)) {
                if (!operStack.isEmpty()) {
                    //如果符号栈由操作符，就进行比较，如果当前的操作符的优先级小于或等于栈中的操作符，就需要从数栈中pop出两个数
                    //在从符号栈中pop出一个符号，进行运算，将得到结果，入数栈，然后将当前的操作符入符号栈
                    if (operStack.priority(ch) <= operStack.priority(operStack.peek())) {
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1, num2, oper);
                        //把运算的结果入数栈
                        numStack.push(res);
                        operStack.push(ch);
                    }else {
                        //如果当前的操作符的优先级大于栈中的操作符，就直接入符号栈
                        operStack.push(ch);
                    }
                } else {
                    //如果为空直接入符号栈
                    operStack.push(ch);
                }
            }else {
                //如果是数字直接入数栈
                numStack.push(ch - 48);
            }
            //让index+1，并判断是否扫描到expression最后
            index++;
            if (index >= expression.length()){
                break;
            }
        }
        //当表达式扫描完毕，就顺序的从 数栈和符号栈中pop出相应的数字和符号，并运行
        while (true){
            //如果符号栈为空，则计算到最后的结果，数栈中只有一个数字【结果】
            if (operStack.isEmpty()){
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1, num2, oper);
            numStack.push(res);
        }
        System.out.printf("表达式: %s = %d",expression,numStack.pop());

    }
}

class ArrayStack2 {
    private int maxSize;//栈的大小
    private int[] stack;//数组，数组模拟栈，数据就放入到数组里面去
    private int top = -1;//top表示栈顶，初始化为-1

    //构造器
    public ArrayStack2(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //增加一个方法，可以返回当前栈顶的值，但是不是真正的pop
    public int peek() {
        return stack[top];
    }

    //栈满
    public boolean isFull() {
        return top == maxSize - 1;
    }

    //栈空
    public boolean isEmpty() {
        return top == -1;
    }

    //入栈
    public void push(int value) {
        //判断栈是否满
        if (isFull()) {
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    //出栈-pop，将栈顶的数据返回
    public int pop() {
        //先判断栈是否空
        if (isEmpty()) {
            //抛出异常
            throw new RuntimeException("栈空，没有数据~~~");
            //这里不需要return
        }
        int value = stack[top];
        top--;
        return value;
    }

    //显示栈的情况【遍历栈】，遍历时需要从栈顶开始显示数据
    public void list() {
        if (isEmpty()) {
            System.out.println("栈空，没有数据~~~");
            return;
        }
        //需要从栈顶开始显示数据
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, stack[i]);
        }
    }

    //返回运算符的优先级，优先级是由程序员决定的，优先级用数字表示
    //数字越大，优先级就越高
    public int priority(int oper) {
        if (oper == '*' || oper == '/') {
            return 1;
        } else if (oper == '+' || oper == '-') {
            return 0;
        } else {
            return -1; //假定目前的表达式只有加减乘除
        }
    }

    //判断是不是一个运算符
    public boolean isOper(char val) {
        return val == '+' || val == '-' || val == '*' || val == '/';
    }

    //计算方法
    public int cal(int num1, int num2, int oper) {
        int res = 0;//res用于存放计算的结果
        switch (oper) {
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num2 - num1;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num2 / num1;
                break;
            default:
                break;
        }
        return res;
    }
}
```

上面的代码计算只针对个位数字有效，

```
表达式: 3+2*6-2 = 13
```

多位数的处理

```java
//如果是数字直接入数栈
//numStack.push(ch - 48);
                /*
                * 1.当处理多位数字的时候，不能发现一个数就立刻入栈，因为它可能是多位数
                * 2.在处理数，需要向expression的表达式的index 后再看一位，如果是数就进行扫描，如果是符号才入栈
                * 3.因此我们需要定义一个字符串变量，用于拼接
                * */

                //处理多位数
                keepNum += ch;

                //如果ch已经是expression的最后一位，就直接入栈
                if (index == expression.length() -1){
                    numStack.push(Integer.parseInt(keepNum));
                }else {
                    if (operStack.isOper(expression.substring(index+1,index+2).charAt(0))){
                        //如果最后一位是运算符，则入栈keepNum = '1'或者‘123’
                        numStack.push(Integer.parseInt(keepNum));
                        //重要的！！！！！！！，keepNum清空
                        keepNum = "";
                    }
                }
```

## 3.3 前缀，中缀，后缀表达式

### 3.3.1 前缀表达式

前缀表达式又称波兰式，**前缀表达式的运算符位于操作数之前**

**前缀表达式的计算机求值**

从左至右扫描表达式，遇到数字，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（栈顶元素和次顶元素），并将结果入栈；重复上述过程直到表达式最左端，最后运算得出的值即为表达式的结果。例如**(3+4)*5-6**对应的前缀表达式就是- * + 3 4 5 6，针对前缀表达式求值步骤如下：

1. 从左往右扫描，将4 5 6 3压入堆栈
2. 遇到+运算符，因此弹出3和4（3作为栈顶元素，4作为次栈顶元素），计算出3+4的值，得到7，再将7入栈。
3. 接下来是*运算符，因此弹出7和5，计算出7 * 5=35，将35入栈
4. 最后是- 运算符，计算出35-6的值，即29，由此得出最终的结果

### 3.3.2 中缀表达式

1. 中缀表达式就是常见的运算表达式
2. 中缀表达式的求值是我们人最熟悉的，但是对于计算机来说却不好操作（前面的案例已经说明了这个问题）因此在计算的时候，往往会将中缀表达式换成其他表达式来操作（一般换成后缀表达式）

### 3.3.3 后缀表达式

1. 后缀表达式又称为逆波兰表达式，与前缀表达式相似，只是运算符位于操作数之后
2. 举例说明：（3+4）* 5 - 6对应的后缀表达式是3 4 + 5 * 6 -
3. 再比如：

| 正常的表达式    | 逆波兰表达式   |
| --------------- | -------------- |
| a + b           | a b +          |
| a + (b - c)     | a b c - +      |
| a + (b - c) * d | a b c -  d * + |
| a + d * (b - c) | a d b c - * +  |
| a = 1 + 3       | a 1 3 + =      |

**后缀表达式的计算机求值**

从左到右扫描表达式，遇到数字，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数字，用运算符对它们做相应的计算（次顶元素和栈顶元素），并将结果入栈；重复上述过程直到表达式最右端，最后运算得出的值为表达式的结果

例如：(3+4) * 5-6对应的前缀表达式为3 4 + 5 * 6 -，针对后缀表达式求值步骤如下：

1. 从左至右扫描。将3和4压入堆栈
2. 遇到 + 运算符，因此弹出4和3（4为栈顶元素，3为次栈顶元素），计算出3+4的值，得到7.再将7入栈。
3. 将5入栈
4. 接下来是 * 运算符，因此弹出5和7，计算出结果为35，将35入栈
5. 将6入栈
6. 最后是运算符，计算出35-6的值，即29为最后的结果

### 3.3.4 逆波兰计算器

要求完成一个逆波兰计算器，要求完成如下任务

1. 输入逆波兰表达式，，使用栈，计算出结果
2. 支持小括号和多位数整数，因为这里我们主要讲的是数据结构，因此计算器进行简化，只支持对整数的计算。
3. 思路分析
4. 代码完成

**代码的实现**

```java
package DataStructures;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class PolandNotation {
    public static void main(String[] args) {
        //先定义给逆波兰表达式
        //(3+4)*5-6 => 3 4 + 5 * 6 -
        //说明为了方便，逆波兰表达式的数字和符号使用空格隔开
        String suffixExpression = "3 4 + 5 * 6 -";
        /*
        * 思路：
        * 1. 先将“3 4 + 5 * 6 -” => 放入到ArrayList中
        * 2. 将ArrayList传递给一个方法，遍历ArrayList 配合栈完成计算
        * */
        List<String> list = getListString(suffixExpression);
        System.out.println("list=" + list);

        int res = calculate(list);
        System.out.println("计算的结果是：" + res);

    }
    //将一个逆波兰表达式，一次将数据和运算符放入到ArrayList中
    public static List<String> getListString(String suffixExpression){
        //将suffixExpression分割
        String[] split = suffixExpression.split(" ");
        List<String> list = new ArrayList<>();
        for (String ele: split){
            list.add(ele);
        }
        return list;
    }
    //完成对逆波兰表达式的运算
    public static int calculate(List<String> ls){
        //创建给栈，只需要一个栈即可
        Stack<String> stack = new Stack<String>();
        //遍历ls
        for (String item:ls){
            //这里使用正则表达式取出数
            if (item.matches("\\d+")){
                //匹配的是多位数
                //入栈
                stack.push(item);
            } else {
                //pop出两个数并运算，再入栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if (item.equals("+")){
                    res = num1 + num2;
                } else if (item.equals("-")){
                    res = num1 - num2;
                } else if (item.equals("*")){
                    res = num1 * num2;
                }else if (item.equals("/")){
                    res = num1 / num2;
                }else {
                    throw new RuntimeException("运算符有误~~~");
                }
                //把res入栈
                stack.push(""+res);//把整数转换成字符串
            }
        }
        //最后留在stack中的数据就是运算结果
        return Integer.parseInt(stack.pop());//把字符串转换成一个整数
    }
}
```

**结果**

```
list=[3, 4, +, 5, *, 6, -]
计算的结果是：29
```

### 3.3.5 中缀转后缀表达式

**具体步骤如下**

1. 初始化两个栈：运算符栈s1和存储中间结果的栈s2；
2. 从左至右扫描中缀表达式
3. 遇到操作数时，将其压入s2
4. 遇到运算符时，比较其与s1栈顶运算符的优先级：
   1. 如果s1为空，或栈顶运算符为左括号 “(” ，则直接将此运算符入栈。
   2. 否则，若优先级比栈顶运算符高，也将运算符压入s1；
   3. 否则，将s1栈顶的运算符弹出并压入到s2中，再次转到(4-1)与s1中新的栈顶运算符相比较。
5. 遇到括号
   1. 如果是左括号"("，则直接压入s1
   2. 如果是右括号")"，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
6. 重复步骤2至5，直到表达式的最右边
7. 将s1中剩余的运算符依次弹出并压入s2
8. 依次弹出s2中的元素并输出，结果的逆序即为中缀表达式对应的后缀表达式

![image-20200806163150886](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwODA2MTYzMTUwODg2LnBuZw?x-oss-process=image/format,png)

```java
package DataStructures;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class PolandNotation {
    public static void main(String[] args) {

        //完成将一个中缀表达式转成后缀表达式的功能

        String expression = "1+((2+3)*4)-5";
        List<String> infixExpressionList = toInfixExpressionList(expression);
        System.out.println("中缀表达式对应的List:" + infixExpressionList);
        List<String> suffixExpressionList = parseSuffixExpressionList(infixExpressionList);
        System.out.println("后缀表达式对应的List:" + suffixExpressionList);

        System.out.printf("expression=%d",calculate(suffixExpressionList));


      /*
        //先定义给逆波兰表达式
        //(3+4)*5-6 => 3 4 + 5 * 6 -
        //说明为了方便，逆波兰表达式的数字和符号使用空格隔开
        String suffixExpression = "3 4 + 5 * 6 -";

//         思路：
//         1. 先将“3 4 + 5 * 6 -” => 放入到ArrayList中
//         2. 将ArrayList传递给一个方法，遍历ArrayList 配合栈完成计算

        List<String> list = getListString(suffixExpression);
        System.out.println("list=" + list);

        int res = calculate(list);
        System.out.println("计算的结果是：" + res);
*/

    }

    //方法：将得到的中缀表达式对应的List => 后缀表达式对应的List
    public static List<String> parseSuffixExpressionList(List<String> ls) {
        //定义两个栈
        Stack<String> s1 = new Stack<String>(); //符号栈
        //说明：因为s2这个栈，再整个转换的过程中，没有pop操作，而且后面我们还需要逆序操作
        //因此比较麻烦，这里我们就不需要去用Stack<String> 直接使用list<String> s2
        List<String> s2 = new ArrayList<String>(); //存储中间结果的List s2

        //遍历ls
        for (String item : ls) {
            //如果是一个数，加入s2
            if (item.matches("\\d+")) {
                s2.add(item);
            } else if (item.equals("(")) {
                s1.push(item);
            } else if (item.equals(")")) {
                //如果是右括号“）”，则依次弹出s1栈顶的运算符，并压入s2，知道遇到左括号为止，此时将这一对括号丢弃
                while (!s1.peek().equals("(")) {
                    s2.add(s1.pop());
                }
                s1.pop();
            } else {
                //当item的优先级小于等于s1栈顶运算符，将s1栈顶的运算符弹出并加入到s2中，再次转到4.1与s1中新栈顶运算符相比较
                while (s1.size() != 0 && Operation.getValue(s1.peek()) >= Operation.getValue(item)) {
                    s2.add(s1.pop());
                }
                //还需要将item压入栈
                s1.push(item);
            }
        }

        //将s1中剩余的运算符依次弹出并加入s2
        while (s1.size() != 0) {
            s2.add(s1.pop());
        }
        return s2; //注意因为是存放到list，因此按顺序输出就是对应的后缀表达式对应的List。

    }

    //方法：将中缀表达式转换成对应的List
    public static List<String> toInfixExpressionList(String s) {
        //定义一个List,存放中缀表达式对应的内容
        List<String> ls = new ArrayList<String>();
        int i = 0; //这是一个指针，用于遍历中缀表达式字符串
        String str; //对多位数的拼接
        char c; //遍历到一个字符，就放入到c
        do {
            //如果c是一个非数字，需要接入到ls
            if ((c = s.charAt(i)) < 48 || (c = s.charAt(i)) > 57) {
                ls.add("" + c);
                i++;
            } else {
                //如果是一个数，就要考虑多位数
                str = ""; //先将str置成空
                while (i < s.length() && (c = s.charAt(i)) >= 48 && (c = s.charAt(i)) <= 57) {
                    str += c;//拼接
                    i++;
                }
                ls.add(str);
            }
        } while (i < s.length());
        return ls;
    }


    //将一个逆波兰表达式，一次将数据和运算符放入到ArrayList中
    public static List<String> getListString(String suffixExpression) {
        //将suffixExpression分割
        String[] split = suffixExpression.split(" ");
        List<String> list = new ArrayList<>();
        for (String ele : split) {
            list.add(ele);
        }
        return list;
    }

    //完成对逆波兰表达式的运算
    public static int calculate(List<String> ls) {
        //创建给栈，只需要一个栈即可
        Stack<String> stack = new Stack<String>();
        //遍历ls
        for (String item : ls) {
            //这里使用正则表达式取出数
            if (item.matches("\\d+")) {
                //匹配的是多位数
                //入栈
                stack.push(item);
            } else {
                //pop出两个数并运算，再入栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if (item.equals("+")) {
                    res = num1 + num2;
                } else if (item.equals("-")) {
                    res = num1 - num2;
                } else if (item.equals("*")) {
                    res = num1 * num2;
                } else if (item.equals("/")) {
                    res = num1 / num2;
                } else {
                    throw new RuntimeException("运算符有误~~~");
                }
                //把res入栈
                stack.push("" + res);//把整数转换成字符串
            }
        }
        //最后留在stack中的数据就是运算结果
        return Integer.parseInt(stack.pop());//把字符串转换成一个整数
    }
}

//编写一个类 Operation 可以看到返回一个运算符 对应的优先级
class Operation {
    private static int ADD = 1;
    private static int SUB = 1;
    private static int MUL = 2;
    private static int DIV = 2;

    //写一个方法，返回对应的优先级数字
    public static int getValue(String operation) {
        int result = 0;
        switch (operation) {
            case "+":
                result = ADD;
                break;
            case "-":
                result = SUB;
                break;
            case "*":
                result = MUL;
                break;
            case "/":
                result = DIV;
                break;
            default:
                System.out.println("不存在该运算符");
                break;
        }
        return result;
    }
}
```

**运行结果**

```
中缀表达式对应的List:[1, +, (, (, 2, +, 3, ), *, 4, ), -, 5]
不存在该运算符
不存在该运算符
后缀表达式对应的List:[1, 2, 3, +, 4, *, +, 5, -]
expression=16
```

### 3.3.6 逆波兰计算器完整版

完整版的逆波兰计算器，功能如下

1. 支持+ - * / （）
2. 支持多位数，支持小数
3. 兼容处理，过滤任何空白字符，包括**空格，制表符，换页符**。

# 4. 递归

递归简单的说：递归就是自己调用自己，每次调用时传入不同的变量，**递归有助于编程者解决复杂的问题，**同时可以让代码变得简洁。

## 4.1 递归的两个小案例

![image-20200806164546527](https://imgconvert.csdnimg.cn/aHR0cDovL2hleWdvLm9zcy1jbi1zaGFuZ2hhaS5hbGl5dW5jcy5jb20vaW1hZ2VzL2ltYWdlLTIwMjAwODA2MTY0NTQ2NTI3LnBuZw?x-oss-process=image/format,png)

### 4.1.1 打印问题

**代码**

```java
package DataStructures;

public class RecursionTest {
    public static void main(String[] args) {
        //通过打印回顾递归调用机制
        test(4);
    }
    public static void test(int n){
        if (n > 2){
            test(n -1);
        }
        System.out.println("n = "+n);
    }
}
```

**运行的结果是**

```
n = 2
n = 3
n = 4
```

但是如果加上else的话情况出现变化：

```java
public class RecursionTest {
    public static void main(String[] args) {
        //通过打印回顾递归调用机制
        test(4);
    }

    public static void test(int n) {
        if (n > 2) {
            test(n - 1);
        } else {
            System.out.println("n = " + n);
        }
    }
}
```

**结果**

```
n = 2
```

**在栈中只要开了新的栈空间都是要执行完毕的，但是对于if的语句却无法跳转过去**

### 4.1.2 阶乘问题

```java
public class RecursionTest {
    public static void main(String[] args) {
        //通过打印回顾递归调用机制
//        test(4);
        int res = factorial(3);
        System.out.println(res);
    }

    public static void test(int n) {
        if (n > 2) {
            test(n - 1);
        } else {
            System.out.println("n = " + n);
        }
    }

    public static int factorial(int n){
        if (n == 1){
            return 1;
        } else {
            return factorial(n-1)*n;
        }
    }
}
```

**结果**

```
6
```

## 4.2 递归的基础知识

### 4.2.1 递归适用于哪些问题



1. 各种数学问题，8皇后问题，汉诺塔，阶乘问题，迷宫问题，球和篮子的问题（Google编程大赛）
2. 各种算法也会用到递归，比如快排，并归排序，二分查找，分治算法等
3. 将用栈解决的问题 ---> 递归代码比较简洁

### 4.2.2 递归需要遵守的规则

1. 执行一个方法时，就创建一个新的受保护的独立空间（栈空间）
2. 方法的局部变量是独立的，不相互影响。
3. 如果方法中使用的是引用变量（比如数组），就会共享该引用类型的数据。
4. 递归必须向退出递归的条件逼近，否则就无限递归，然后就死鬼了；
5. 当一个方法执行完毕，或者遇到return，就会返回，遵守谁调用，就将结果返回给谁，同时方法执行完毕或返回时，该方法也就执行完毕。

## 4.3 迷宫回溯问题

代码的实现

```java
public class MiGong {
    public static void main(String[] args) {
        //先创建一个二维数组，模拟迷宫
        //地图
        int[][] map = new int[8][7];
        //使用1表示墙
        //上下全部置为1
        for (int i = 0; i < 7; i++) {
            map[0][i] = 1;
            map[7][i] = 1;
        }
        //左右全部置为1
        for (int i = 0; i < 8; i++) {
            map[i][0] = 1;
            map[i][6] = 1;
        }
        //设置挡板
        map[3][1] = 1;
        map[3][2] = 1;
        System.out.println("地图的情况~~~");
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println();
        }

        setWay(map,1,1);
        System.out.println("小球走过后的地图情况~~~");
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println();
        }
    }
    //使用递归回溯来给小球找路
    /*
    * 1. map表示地图
    * 2. i,j表示从地图的哪个位置开始出发（1，1）
    * 3.如果小球找到map[6][5]位置，则说明通路找到。
    * 4. 约定：当map[i][j]为0表示该点没有走过，当为1表示墙，2表示通路可以走，3表示已经走过，但是走不通
    * 5. 在走迷宫的时候，需要确定一个策略（方法）下->右->上->左，如果该点走不通再回溯。
    * */

    public static boolean setWay(int[][] map, int i,int j){
        if (map[6][5] == 2){
            return true;
        } else {
            if (map[i][j] == 0){
                //如果当前这个点还没有走过则按照设定的规则走
                map[i][j] = 2;//假定这个点是可以走通的
                if (setWay(map, i+1, j)){
                    //向下走
                    return true;
                } else if (setWay(map, i, j+1)){
                    //向右走
                    return true;
                } else if (setWay(map, i-1, j)){
                    //向上走
                    return true;
                } else if (setWay(map, i, j-1)){
                    //向左走
                    return true;
                } else {
                    //说明该点不通，是死路
                    map[i][j] = 3;
                    return false;
                }
            } else {
                //如果map[i][j] != 0,则有可能是1，2，3
                return false;
            }
        }
    }
}
```

**运行的结果**

```
地图的情况~~~
1 1 1 1 1 1 1 
1 0 0 0 0 0 1 
1 0 0 0 0 0 1 
1 1 1 0 0 0 1 
1 0 0 0 0 0 1 
1 0 0 0 0 0 1 
1 0 0 0 0 0 1 
1 1 1 1 1 1 1 
小球走过后的地图情况~~~
1 1 1 1 1 1 1 
1 2 0 0 0 0 1 
1 2 2 2 0 0 1 
1 1 1 2 0 0 1 
1 0 0 2 0 0 1 
1 0 0 2 0 0 1 
1 0 0 2 2 2 1 
1 1 1 1 1 1 1 
```

## 4.4 八皇后问题

**八皇后问题**（[英文](https://baike.baidu.com/item/英文/3079091)：**Eight queens**），是由国际[西洋棋](https://baike.baidu.com/item/西洋棋/1284326)棋手马克斯·贝瑟尔于1848年提出的问题，是[回溯算法](https://baike.baidu.com/item/回溯算法/9258495)的典型案例。

问题表述为：在8×8格的[国际象棋](https://baike.baidu.com/item/国际象棋/80888)上摆放8个[皇后](https://baike.baidu.com/item/皇后/15860305)，使其不能互相攻击，即任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种摆法。[高斯](https://baike.baidu.com/item/高斯/24098)认为有76种方案。1854年在[柏林](https://baike.baidu.com/item/柏林/75855)的象棋杂志上不同的作者发表了40种不同的解，后来有人用[图论](https://baike.baidu.com/item/图论/1433806)的方法解出92种结果。如果经过±90度、±180度旋转，和对角线对称变换的摆法看成一类，共有42类。[计算机](https://baike.baidu.com/item/计算机/140338)发明后，有多种计算机语言可以编程解决此问题。

<img src="https://bkimg.cdn.bcebos.com/pic/d1a20cf431adcbef8c2f33e5adaf2edda3cc9f3e?x-bce-process=image/resize,m_lfit,w_268,limit_1/format,f_jpg" alt="img" style="zoom:67%;" />

**八皇后问题（回溯算法）**

1. 第一个皇后先放第一行第一列
2. 第二个皇后放在第二行的第一列，然后判断是否OK，如果不OK，继续放在第二列，第三列，依次把所有列都放完，找到一个合适的
3. 继续放第三个皇后，还是第一列，第二列，。。。直到放到第八个皇后，放到一个不冲突的位置上，算是找到一个正确的解
4. 当得到一个正确解时，在栈退回上一个栈的时候，就会开始回溯，即将第一个皇后，放到第一列的所有正确解，全部得到。
5. 然后回头继续第一个皇后放第二列，后面继续循环执行1，2，3，4的步骤。

**说明：**理论上应该创建一个二维数组来表示棋盘，但是实际上可以通过算法，用一个一维数组即可解决问题。arr[8]={0,4,7,5,2,6,1,3}，对应arr下标表示第几行，即第几个皇后， arr[i]=val，val表示第i+1个皇后，放在第i+1行的第val + 1 列。

**代码实现**

```java
public class Queen8 {

    //定义一个max表示共有多少个皇后
    int max = 8;
    //定义数组array，保存皇后放置位置的结果
    int[] array = new int[max];
    static int count = 0;
    public static void main(String[] args) {
        //测试一下
         Queen8 queen8 = new Queen8();
        queen8.check(0);
        System.out.printf("一共有%d种解法",count);


    }

    //编写一个方法，放置第n个皇后
    //check是每一次递归时，进入到check中都有一个循环，所以会产生递归。
    private void check(int n){
        if (n == max){
            //n=8，其实8个皇后都已经放好了
            print();
            return;
        }
        //依次放入皇后，判断是否冲突
        for (int i =0;i<max;i++){
            //先把当前这个皇后n，放到该行的第一列
            array[n]=i;
            //判断放置当前这个皇后n到i列时，是否冲突
            if (judge(n)){
                //不冲突，接着放n+1个皇后，从这也开始递归了
                check(n+1);
            }
            //如果冲突，就会继续执行array[n]=i,因为i++，所以会在本行后移一个位置，
        }
    }




    //查看当前我们放置的第n个皇后，就去检测该皇后和前面已经拜访的皇后是否冲突
    private boolean judge(int n){
        for (int i = 0;i < n;i++){
            //前面一个判断是是否在同一列，后者判断是否是在对角线，其实abs的作用是判断后者是否是在前者的左前方，将二维转换成一维的思想。
            if (array[i] == array[n] || Math.abs(n-i) == Math.abs(array[n]- array[i])){
                return false;
            }
        }
        return true;
    }


    //写一个方法，可以将皇后摆放的位置输出
    private void print(){
        count++;
        for (int i =0;i<array.length;i++){
            System.out.print(array[i]+" ");
        }
        System.out.println();
    }
}
```

**结果显示**

```
0 4 7 5 2 6 1 3 
0 5 7 2 6 3 1 4 
0 6 3 5 7 1 4 2 
0 6 4 7 1 3 5 2 
1 3 5 7 2 0 6 4 
1 4 6 0 2 7 5 3 
1 4 6 3 0 7 5 2 
1 5 0 6 3 7 2 4 
1 5 7 2 0 3 6 4 
1 6 2 5 7 4 0 3 
1 6 4 7 0 3 5 2 
1 7 5 0 2 4 6 3 
2 0 6 4 7 1 3 5 
2 4 1 7 0 6 3 5 
2 4 1 7 5 3 6 0 
2 4 6 0 3 1 7 5 
2 4 7 3 0 6 1 5 
2 5 1 4 7 0 6 3 
2 5 1 6 0 3 7 4 
2 5 1 6 4 0 7 3 
2 5 3 0 7 4 6 1 
2 5 3 1 7 4 6 0 
2 5 7 0 3 6 4 1 
2 5 7 0 4 6 1 3 
2 5 7 1 3 0 6 4 
2 6 1 7 4 0 3 5 
2 6 1 7 5 3 0 4 
2 7 3 6 0 5 1 4 
3 0 4 7 1 6 2 5 
3 0 4 7 5 2 6 1 
3 1 4 7 5 0 2 6 
3 1 6 2 5 7 0 4 
3 1 6 2 5 7 4 0 
3 1 6 4 0 7 5 2 
3 1 7 4 6 0 2 5 
3 1 7 5 0 2 4 6 
3 5 0 4 1 7 2 6 
3 5 7 1 6 0 2 4 
3 5 7 2 0 6 4 1 
3 6 0 7 4 1 5 2 
3 6 2 7 1 4 0 5 
3 6 4 1 5 0 2 7 
3 6 4 2 0 5 7 1 
3 7 0 2 5 1 6 4 
3 7 0 4 6 1 5 2 
3 7 4 2 0 6 1 5 
4 0 3 5 7 1 6 2 
4 0 7 3 1 6 2 5 
4 0 7 5 2 6 1 3 
4 1 3 5 7 2 0 6 
4 1 3 6 2 7 5 0 
4 1 5 0 6 3 7 2 
4 1 7 0 3 6 2 5 
4 2 0 5 7 1 3 6 
4 2 0 6 1 7 5 3 
4 2 7 3 6 0 5 1 
4 6 0 2 7 5 3 1 
4 6 0 3 1 7 5 2 
4 6 1 3 7 0 2 5 
4 6 1 5 2 0 3 7 
4 6 1 5 2 0 7 3 
4 6 3 0 2 7 5 1 
4 7 3 0 2 5 1 6 
4 7 3 0 6 1 5 2 
5 0 4 1 7 2 6 3 
5 1 6 0 2 4 7 3 
5 1 6 0 3 7 4 2 
5 2 0 6 4 7 1 3 
5 2 0 7 3 1 6 4 
5 2 0 7 4 1 3 6 
5 2 4 6 0 3 1 7 
5 2 4 7 0 3 1 6 
5 2 6 1 3 7 0 4 
5 2 6 1 7 4 0 3 
5 2 6 3 0 7 1 4 
5 3 0 4 7 1 6 2 
5 3 1 7 4 6 0 2 
5 3 6 0 2 4 1 7 
5 3 6 0 7 1 4 2 
5 7 1 3 0 6 4 2 
6 0 2 7 5 3 1 4 
6 1 3 0 7 4 2 5 
6 1 5 2 0 3 7 4 
6 2 0 5 7 4 1 3 
6 2 7 1 4 0 5 3 
6 3 1 4 7 0 2 5 
6 3 1 7 5 0 2 4 
6 4 2 0 5 7 1 3 
7 1 3 0 6 4 2 5 
7 1 4 2 0 6 3 5 
7 2 0 5 1 4 6 3 
7 3 0 2 5 1 6 4 
一共有92种解法
```

# 5. 排序算法

## 5.1排序算法介绍

排序算法（Sort Algorithm），排序是将一组数据，依指定的顺序进行排序的过程

排序的分类：

1. 内部排序：指将需要处理的所有数据都加载到内部存储器中进行排序。
2. 外部排序：数据量过大，无法全部加载到内存中，需要借助外部存储进行排序。

**常见的排序算法分类**

![image-20210325084358307](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325084358307.png)

**度量一个程序（算法）执行时间的两种方法**

1. 事后统计方法

这种方法可行，但是有两个问题：一是要想对设计的算法的运行能力进行评价，需要实际运行该程序；二是所得时间的统计量依赖于计算机的硬件，软件等环境因素，**这种方式，要在同一台计算机的相同状态下运行，才能比较哪个算法速度更快**

2. 事前估计的方法

通过分析某个算法的时间复杂度来判断哪个算法更优。

### 5.1.1 算法的时间复杂度

**时间频度**

时间频度：一个算法花费的时间与算法中语句的执行次数成正比，哪个算法在语句执行的次数多，它花费的时间就多，**一个算法中的语句执行次数称为语句频度或时间频度。**计为T(n)。

![image-20210325084440288](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325084440288.png)



**结论**

1. 2n+20和2n随着n变大，执行曲线无限接近，20可以忽略
2. 3n+10和3n随着n变大，执行曲线无限接近，10可以忽略

![image-20210325084551920](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325084551920.png)

**结论**

1. 2n^2+3n+10和2n^2随着n的增大，执行曲线无限接近，可以忽略3n+10
2. n^2+5n+20和n^2随着n的增大，执行曲线无限接近，可以忽略5n+20

![image-20210325084622475](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325084622475.png)

**结论**

- 随着 n 值变大， 5n^2+7n 和 3n^2 + 2n ， 执行曲线重合, 说明 这种情况下, 5 和 3 可以忽略。
- 而 n^3+5n 和 6n^3+4n ， 执行曲线分离， 说明多少次方式关键

### 5.1.2 时间复杂度概念

1. 一般情况下，算法中的基本操作语句的重复执行次数是问题规模n的某个函数，用T(n)表示，若某个辅助函数f(n)，使得当n趋近于无穷大时，T(n)/f(n)的极限值为不等于零的常数，则称f(n)是T(n)的同数量级函数，记作T(n)=O(f(n))，称O(f(n))为算法的渐进时间复杂度，简称时间复杂度。
2. T(n)不同，但时间复杂度可能相同。如：T(n)=n^2+7n+6与T(n)+3n^2+2n+2，它们的T(n)不同，但时间复杂度相同都是O(n^2)。
3. 计算时间复杂度的方法
   1. 用常数1代替运行时间中的所有加法常数
   2. 修改后的运行次数函数中，只保留最高阶项
   3. 去除最高阶项的系数

![image-20210325090021138](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325090021138.png)

**常数阶O(1)**

```
int i = 1;
int j = 2;
++i;
j++;
int m = i + j;
```

无论代码执行了多少行，只要没有循环体等复杂结构，那么这个代码的时间复杂度就是O(1)，上述代码在执行的时候，它消耗的时间不随着某个变量的增长而增长，那么无论这类代码有多长。即使几十万都可以用O(1)来表示它的时间复杂度。

**对数阶**

![image-20210325090949315](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325090949315.png)

**线性阶O(n)**

```
for (i = 1;i <= n; ++i){
j = i;
j++;
}
```

这段代码，for循环里面的代码块会执行n遍，因此它消耗的时间是随着n的变化而变化的，因此这类代码都可以用O(n)来表示它的时间复杂度。

**线性对数阶**

![image-20210325092301879](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325092301879.png)

**平方阶O(n^2)**

```
for (x = 1;i <= n;x++){
for (i = 1;i <= n;i++){
j = i;
j++;
}
}
```

平方阶O(n^2)就更容易理解了，如果把O(n)的代码再嵌套循环一遍，它的时间复杂度就是O(n^2)，这段代码其实就是嵌套了2层n循环，如果将其中的一层循环由n改为m，那么它的时间复杂度就变成了O(m*n)。

### 5.1.3 平均时间复杂度和最坏时间复杂度

1. 平均时间复杂度是指所有可能的输入实例以等概率出现的情况下，该算法的运行时间
2. 最坏情况下的时间复杂度称最坏时间复杂度，一般讨论的时间复杂度均是最坏情况下的时间复杂度，这样做的原因是：最坏情况下的时间复杂度是算法在任何输入实例上运行时间的界限，这就保证了算法的运行时间不会比最坏情况更长了。
3. 平均时间复杂度和最坏时间复杂度是否一致，和算法有关如图所示；

![image-20210325093406839](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325093406839.png)

### 5.1.4 算法空间复杂度

1. 类似于时间复杂度的讨论，一个算法的空间复杂度(Space Complexity)定义为该算法所消耗的存储空间，也是问题规模n的函数。
2. 空间复杂度是对一个算法在运行过程中临时占用存储空间大小的量度。有的算法需要占用的临时工作单元数与解决问题的规模n有关，它随着n的增大而增大，当n比较大时，将占用较多的存储单元，例如快速排序和归并排序算法就属于这种情况。
3. 在做算法分析的时候，**主要讨论的是时间复杂度**。从用户使用体验上来看，更看重的程序执行的速度，一些缓存产品（redis,memcache）和算法（基数排序）的本质就是用空间换时间。

## 5.2 冒泡排序

### 5.2.1 基本介绍

冒泡排序（Bubble Sorting）的基本思想是：通过对待排序序列从前向后（从小标较小的元素开始），依次比较相邻元素的值，若发现逆序则交换，使值较大的元素逐渐从前向移向后部，就象水底下的气泡一样逐渐向上冒。

**因为排序的过程中，各个元素不断接近自己的位置，如果一趟比较下来没有进行过交换，就说明序列有序，因此需要在排序的过程中设置一个标志flag判断元素是否进行过交换**。

**思路**

![image-20210325101055268](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325101055268.png)

### 5.2.2 应用实例

我们举例一个具体的案例来说明冒泡法，将五个无序的数：3 9 -1 10 -2，使用冒泡排序法将其排成一个从小到大的有序数列

**基础代码**

```java
public class BubbleSort {
    public static void main(String[] args) {
        int arr[] = {3, 9, -1, 10, -2};

        //第一趟排序，就是将最大的数排在最后
        int temp = 0;//临时变量
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                //如果前面的数比后面的数大就交换
                if (arr[j] > arr[j + 1]) {
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }

            System.out.println("第"+(i+1)+"趟排序后的数组~~~");
            System.out.println(Arrays.toString(arr));
        }
/*
        //第二趟排序，就是将第二大的数排在倒数第二位
        for (int j = 0; j < arr.length - 2; j++) {
            //如果前面的数比后面的数大就交换
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第二趟排序后的数组~~~");
        System.out.println(Arrays.toString(arr));

        //第三趟排序，就是将第二大的数排在倒数第三位
        for (int j = 0; j < arr.length - 3; j++) {
            //如果前面的数比后面的数大就交换
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第三趟排序后的数组~~~");
        System.out.println(Arrays.toString(arr));

        //第四趟排序，就是将第二大的数排在倒数第三位
        for (int j = 0; j < arr.length - 4; j++) {
            //如果前面的数比后面的数大就交换
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第四趟排序后的数组~~~");
        System.out.println(Arrays.toString(arr));

 */

    }
}
```

**结果**

```
第1趟排序后的数组~~~
[3, -1, 9, -2, 10]
第2趟排序后的数组~~~
[-1, 3, -2, 9, 10]
第3趟排序后的数组~~~
[-1, -2, 3, 9, 10]
第4趟排序后的数组~~~
[-2, -1, 3, 9, 10]
```

优化：如果我们发现在某趟排序中，没有发生一次交换，可以提前结束冒泡排序，这个就是优化。

```java
public class BubbleSort {
    public static void main(String[] args) {
//        int arr[] = {3, 9, -1, 10, 20};
//        System.out.println("排序前~~~");
//        System.out.println(Arrays.toString(arr));

        //测试一下冒泡排序的速度O(n^2)，给80000个数据，测试
        //创建要给80000个随机的数组
        int[] arr = new int[80000];
        for (int i = 0;i < 80000;i++){
            arr[i] = (int)(Math.random() * 800000); //生成一个【0，800000）数
        }

        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String date1Str = simpleDateFormat.format(date1);
        System.out.println("排序前的时间："+date1Str);
        //测试冒泡排序
        bubbleSort(arr);

        Date date2 = new Date();
        String date2Str = simpleDateFormat.format(date2);
        System.out.println("排序后的时间："+date2Str);
//        System.out.println("测试后~~~");
//        System.out.println(Arrays.toString(arr));
/*
        //第二趟排序，就是将第二大的数排在倒数第二位
        for (int j = 0; j < arr.length - 2; j++) {
            //如果前面的数比后面的数大就交换
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第二趟排序后的数组~~~");
        System.out.println(Arrays.toString(arr));

        //第三趟排序，就是将第二大的数排在倒数第三位
        for (int j = 0; j < arr.length - 3; j++) {
            //如果前面的数比后面的数大就交换
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第三趟排序后的数组~~~");
        System.out.println(Arrays.toString(arr));

        //第四趟排序，就是将第二大的数排在倒数第三位
        for (int j = 0; j < arr.length - 4; j++) {
            //如果前面的数比后面的数大就交换
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第四趟排序后的数组~~~");
        System.out.println(Arrays.toString(arr));

 */

    }

    public static void bubbleSort(int[] arr) {
        //第一趟排序，就是将最大的数排在最后
        int temp = 0;//临时变量
        boolean flag = false;

        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - 1 - i; j++) {
                //如果前面的数比后面的数大就交换
                if (arr[j] > arr[j + 1]) {
                    flag = true;
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }

//            System.out.println("第" + (i + 1) + "趟排序后的数组~~~");
//            System.out.println(Arrays.toString(arr));

            if (!flag) {
                //在一趟排序中，一次交换没有发生过
                break;
            } else {
                flag = false;//重置flag!!!进行下次判断
            }
        }

    }
}
```

**结果**

```
排序前的时间：2021-03-25 12:56:23
排序后的时间：2021-03-25 12:56:36
```



## 5.3 选择排序

选择排序也属于内部排序法，是从欲排序的数据中，按指定的规则选出某一元素，再次依规定交换位置后达到排序的目的。

![image-20210325130810350](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325130810350.png)



![image-20210325131357981](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325131357981.png)

**代码实现**

```java
public class SelectSort {
    public static void main(String[] args) {
        int[] arr = {101, 34, 119, 1,-1,90,123};
        System.out.println("排序前~~~");
        System.out.println(Arrays.toString(arr));
        System.out.println("排序后~~~");
        selectSort(arr);
        System.out.println(Arrays.toString(arr));
    }

    //选择排序
    public static void selectSort(int[] arr) {


        //在推导的过程中，我们找到了规律，因此我们可以使用for来解决


        for (int i = 0; i < arr.length - 1; i++) {
            int minIndex = i;
            int min = arr[i];

            for (int j = i + 1; j < arr.length; j++) {
                if (min > arr[j]) {
                    //说明假定的最小值，并不是最小值
                    min = arr[j]; //重置min
                    minIndex = j; //重置minIndex
                }
            }
            //将最小值，放在arr[0]，即交换
            if (minIndex != i) {
                arr[minIndex] = arr[i];
                arr[i] = min;
            }
//            System.out.println("第"+ (i+1) + "一轮后");
//            System.out.println(Arrays.toString(arr));
        }
/*
        //第一轮
        int minIndex = 0;
        int min = arr[0];

        for (int j = 0 + 1; j < arr.length; j++) {
            if (min > arr[j]) {
                //说明假定的最小值，并不是最小值
                min = arr[j]; //重置min
                minIndex = j; //重置minIndex
            }
        }

        //将最小值，放在arr[0]，即交换
        if (minIndex != 0) {
            arr[minIndex] = arr[0];
            arr[0] = min;
        }
        System.out.println("第一轮后");
        System.out.println(Arrays.toString(arr));


        //第二轮
        minIndex = 1;
        min = arr[1];
        for (int j = 1 + 1; j < arr.length; j++) {
            if (min > arr[j]) {
                //说明假定的最小值，并不是最小值
                min = arr[j]; //重置min
                minIndex = j; //重置minIndex
            }
        }

        //将最小值，放在arr[0]，即交换
        if (minIndex != 1) {
            arr[minIndex] = arr[1];
            arr[1] = min;
        }
        System.out.println("第二轮后");
        System.out.println(Arrays.toString(arr));


        //第三轮
        minIndex = 2;
        min = arr[2];
        for (int j = 2 + 1; j < arr.length; j++) {
            if (min > arr[j]) {
                //说明假定的最小值，并不是最小值
                min = arr[j]; //重置min
                minIndex = j; //重置minIndex
            }
        }

        //将最小值，放在arr[0]，即交换
        if (minIndex != 2) {
            arr[minIndex] = arr[2];
            arr[2] = min;
        }
        System.out.println("第二轮后");
        System.out.println(Arrays.toString(arr));

 */

    }


}
```

**结果**

```
排序前~~~
[101, 34, 119, 1, -1, 90, 123]
排序后~~~
[-1, 1, 34, 90, 101, 119, 123]
```



**运行时间代码测试**

```
//        int[] arr = {101, 34, 119, 1,-1,90,123};
        int[] arr = new int[80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 800000); //生成一个【0，800000）数
        }

        System.out.println("排序前~~~");
//        System.out.println(Arrays.toString(arr));
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String date1Str = simpleDateFormat.format(date1);
        System.out.println("排序前的时间："+date1Str);

        System.out.println("排序后~~~");
        selectSort(arr);
        Date date2 = new Date();
        String date2Str = simpleDateFormat.format(date2);
        System.out.println("排序后的时间："+date2Str);
```

**结果**

```
排序前~~~
排序前的时间：2021-03-25 14:56:24
排序后~~~
排序后的时间：2021-03-25 14:56:28
```

## 5.4 插入排序

插入式排序属于内部排序法，是对于欲排序的元素以插入的方式寻求该元素的适当位置，以达到排序的目的。

插入排序（Inserting Sorting）的基本思想是：把n个待排序的元素看成为一个有序表和一个无序表，开始时有序表中只含有一个元素，无需表中包含有n-1个元素，排序过程中每次从无序表中取出第一个元素，把它的排序码依次与有序表元素的排序码进行比较，将它插入到有序表中的适当位置，使之成为新的有序表。

![image-20210325151652653](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210325151652653.png)

**代码实现**

```java
public class InsertSort {
    public static void main(String[] args) {
//        int[] arr = {101, 34, 119, 1};

        int[] arr = new int[80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 800000); //生成一个【0，800000）数
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String date1Str = simpleDateFormat.format(date1);
        System.out.println("排序前的时间：" + date1Str);
        //测试插入排序
        insertSort(arr);

        Date date2 = new Date();
        String date2Str = simpleDateFormat.format(date2);
        System.out.println("排序后的时间：" + date2Str);


    }

    //插入排序
    public static void insertSort(int[] arr) {

        for (int i = 1; i < arr.length; i++) {
            int insertVal = arr[i];
            int insertIndex = i - 1; //即arr[1]得前面这个数得下标

            //给insertVal 找到插入得位置
            //说明
            //1. insertIndex >= 0  保证在给insertVal 找到插入位置，不越界
            //2. insertVal < arr[insertIndex] 待插入得数，还没有找到插入的位置
            //3. 就需要将arr[insertIndex] 后移
            while (insertIndex >= 0 && insertVal < arr[insertIndex]) {
                arr[insertIndex + 1] = arr[insertIndex];
                insertIndex--;
            }
            //当退出while循环时，说明插入的位置找到，insertIndex +1;
            //这里我们判断是否需要赋值
            if (insertIndex + 1 != i) {
                arr[insertIndex + 1] = insertVal;
            }
//            System.out.println("第" + i + "轮插入");
//            System.out.println(Arrays.toString(arr));
        }


        /*
        //使用逐步推导的方式来讲解
        //第一轮
        //定义待插入的数
        int insertVal = arr[1];
        int insertIndex = 1 -1; //即arr[1]得前面这个数得下标

        //给insertVal 找到插入得位置
        //说明
        //1. insertIndex >= 0  保证在给insertVal 找到插入位置，不越界
        //2. insertVal < arr[insertIndex] 待插入得数，还没有找到插入的位置
        //3. 就需要将arr[insertIndex] 后移
        while (insertIndex >= 0 && insertVal < arr[insertIndex]){
            arr[insertIndex + 1] = arr[insertIndex];
            insertIndex--;
        }
        //当退出while循环时，说明插入的位置找到，insertIndex +1;
        arr[insertIndex +1] = insertVal;

        System.out.println("第1轮插入");
        System.out.println(Arrays.toString(arr));


        //第二轮
        //定义待插入的数
         insertVal = arr[2];
         insertIndex = 2 -1; //即arr[1]得前面这个数得下标

        //给insertVal 找到插入得位置
        //说明
        //1. insertIndex >= 0  保证在给insertVal 找到插入位置，不越界
        //2. insertVal < arr[insertIndex] 待插入得数，还没有找到插入的位置
        //3. 就需要将arr[insertIndex] 后移
        while (insertIndex >= 0 && insertVal < arr[insertIndex]){
            arr[insertIndex + 1] = arr[insertIndex];
            insertIndex--;
        }
        //当退出while循环时，说明插入的位置找到，insertIndex +1;
        arr[insertIndex +1] = insertVal;

        System.out.println("第2轮插入");
        System.out.println(Arrays.toString(arr));

        //第三轮
        //定义待插入的数
        insertVal = arr[3];
        insertIndex = 3 - 1; //即arr[1]得前面这个数得下标

        //给insertVal 找到插入得位置
        //说明
        //1. insertIndex >= 0  保证在给insertVal 找到插入位置，不越界
        //2. insertVal < arr[insertIndex] 待插入得数，还没有找到插入的位置
        //3. 就需要将arr[insertIndex] 后移
        while (insertIndex >= 0 && insertVal < arr[insertIndex]){
            arr[insertIndex + 1] = arr[insertIndex];
            insertIndex--;
        }
        //当退出while循环时，说明插入的位置找到，insertIndex +1;
        arr[insertIndex +1] = insertVal;

        System.out.println("第3轮插入");
        System.out.println(Arrays.toString(arr));

         */


    }
}
```

**运行结果**

```
排序前的时间：2021-03-29 11:08:21
排序后的时间：2021-03-29 11:08:23
```

## 5.5 希尔排序

### 5.5.1 希尔排序算法的介绍

针对插入排序中存在的问题有：

**当需要插入的数是较小的数时，后移的次数明显增多，对效率有影响**

![image-20210329111648973](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210329111648973.png)

**希尔排序算法的介绍**

希尔排序是1959年提出的一种排序算法，希尔排序也是插入排序，它是简单的排序经过改进之后的一个**更高效**的版本，也称为**缩小增量排序**。

希尔排序的基本思想：

希尔排序是把记录按小标的一定增量分组，对每组使用直接插入排序算法排序，随着增量的减少，每组包含的关键词越来越多，当增量减少至1时，整个文件被分成一组，算法终止。

![image-20210329112211305](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210329112211305.png)

**经过上面的宏观调控，整个数组的有序化程度成果惊人，此时仅仅需要对以上数列简单微调，无需大量移动就可以完成整个数组的排序。**

### 5.5.2 算法的实现

**交换法实现**

```java
public class ShellSort {
    public static void main(String[] args) {
        int[] arr = {8, 9, 1, 7, 2, 3, 5, 4, 6, 0};
        shellSort(arr);
    }

    //使用逐步推导的方式来编写希尔排序
    public static void shellSort(int[] arr) {
        int temp = 0;
        int count = 0;

        //根据前面的逐步分析，使用循环处理
        //希尔排序时，对有序序列在插入时采用交换法

        for (int gap = arr.length / 2; gap > 0; gap /= 2) {
            for (int i = gap; i < arr.length; i++) {
                //遍历各组中所有的元素（共gap组）
                for (int j = i - gap; j >= 0; j -= gap) {
                    //如果当前元素大于加上步长后的那个元素，说明交换
                    if (arr[j] > arr[j + gap]) {
                        temp = arr[j];
                        arr[j] = arr[j + gap];
                        arr[j + gap] = temp;
                    }
                }
            }
            System.out.println("希尔排序" + (++count) + "轮=" + Arrays.toString(arr));
        }
/*
        int temp = 0;
        //希尔排序的第一轮排序
        //因为是第一轮排序，是将十个数据分成五组
        for (int i = 5; i < arr.length; i++) {
            //遍历各组中所有的元素（共5组，每组有2个元素），步长为5
            for (int j = i - 5; j >= 0; j -= 5) {
                //如果当前元素大于加上步长后的那个元素，说明交换
                if (arr[j] > arr[j + 5]) {
                    temp = arr[j];
                    arr[j] = arr[j + 5];
                    arr[j + 5] = temp;
                }
            }
        }

        System.out.println("希尔排序的1轮后=" + Arrays.toString(arr));


        //因为是第二轮排序，是将十个数据分成五组
        for (int i = 2; i < arr.length; i++) {
            //遍历各组中所有的元素（共2组，每组有5个元素），步长为 2
            for (int j = i - 2; j >= 0; j -= 2) {
                //如果当前元素大于加上步长后的那个元素，说明交换
                if (arr[j] > arr[j + 2]) {
                    temp = arr[j];
                    arr[j] = arr[j + 2];
                    arr[j + 2] = temp;
                }
            }
        }

        System.out.println("希尔排序的2轮后=" + Arrays.toString(arr));

        //因为是第三轮排序，是将十个数据分成五组
        for (int i = 1; i < arr.length; i++) {
            //遍历各组中所有的元素（共2组，每组有5个元素），步长为 2
            for (int j = i - 1; j >= 0; j -= 1) {
                //如果当前元素大于加上步长后的那个元素，说明交换
                if (arr[j] > arr[j + 1]) {
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }

        System.out.println("希尔排序的3轮后=" + Arrays.toString(arr));

 */

    }
}
```

**运行的结果显示**

```
希尔排序1轮=[3, 5, 1, 6, 0, 8, 9, 4, 7, 2]
希尔排序2轮=[0, 2, 1, 4, 3, 5, 7, 6, 9, 8]
希尔排序3轮=[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**效率的显示**

```
排序前的时间：2021-03-29 14:00:10
排序后的时间：2021-03-29 14:00:17
```



**移位式的算法实现**

```java
public static void shellSort2(int[] arr) {
        //增量gap，并逐步的缩小增量
        for (int gap = arr.length / 2; gap > 0; gap /= 2) {
            //从第gap个元素，逐步对其所在的组进行直接插入排序
            for (int i = gap; i < arr.length; i++) {
                int j = i;
                int temp = arr[j];
                if (arr[j] < arr[j - gap]) {
                    while (j - gap >= 0 && temp < arr[j - gap]) {
                        //移动
                        arr[j] = arr[j - gap];
                        j -= gap;
                    }
                    //当退出while之后，就给temp找到插入的位置
                    arr[j] = temp;
                }
            }
        }
    }
```



**算法效率的计算**

```
排序前的时间：2021-03-29 14:31:00
排序后的时间：2021-03-29 14:31:00
```

**这也太牛逼了吧~~~~~**~



## 5.6 快速排序算法

### 5.6.1 快速排序算法的介绍

快速排序算法（Quicksort）是对冒泡排序的一种改进，基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另一部分的所有数据要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序国恒可以递归，以此达到整个数据都变成有序序列。

![image-20210329144131037](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210329144131037.png)

### 5.6.3 代码实现



**代码的实现**

```
public class QuickSort {
    public static void main(String[] args) {
//        int[] arr = {-9, 78, 0, 23, -567, 70};
//        quickSort(arr,0,arr.length - 1);
//        System.out.println(Arrays.toString(arr));
        int[] arr = new int[80000];
        for (int i = 0;i < 80000;i++){
            arr[i] = (int)(Math.random() * 800000); //生成一个【0，800000）数
        }

        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String date1Str = simpleDateFormat.format(date1);
        System.out.println("排序前的时间："+date1Str);

        quickSort(arr,0,arr.length - 1);

        Date date2 = new Date();
        String date2Str = simpleDateFormat.format(date2);
        System.out.println("排序后的时间："+date2Str);
    }
/*
    public static void quickSort(int[] arr, int left, int right) {
        int l = left;
        int r = right;
        int temp = 0;//作为交换时使用
        //pivot 中轴值
        int pivot = arr[(left + right) / 2];
        //while循环的目的是让比pivot值小放到左边
        //比pivot大的值放右边
        while (l < r) {
            //在pivot的左边一直找，直到大于等于pivot的值，才退出
            while (arr[l] < pivot) {
                l += 1;
            }
            //在pivot的右边一直找，找到小于等于pivot值，才退出
            while (arr[r] > pivot) {
                r -= 1;
            }
            //如果l >= r说明pivot的左右两边的值，已经按照左边全部是小于等于pivot的值，右边全部是大于等于pivot值
            if (l >= r) {
                break;
            }
            //交换
            temp = arr[l];
            arr[l] = arr[r];
            arr[r] = temp;

            //如果交换完毕，发现这个arr[l] == pivot值相等，r--，前移
            if (arr[l] == pivot) {
                l -= 1;
            }
            //如果交换完毕，发现这个arr[l] == pivot值相等，l++，后移
            if (arr[r] == pivot) {
                r += 1;
            }
        }

        //如果l == r，则必须l++,r++否则出现栈溢出

        if (l == r) {
            l += 1;
            r -= 1;
        }
        //向左递归
        if (left < r) {
            quickSort(arr,left,r);
        }
        //向右递归
        if (right > l) {
            quickSort(arr,l,right);
        }
    }

 */



    private static void quickSort(int[] arr, int left, int right) {
        if (left < right) {
            int partitionIndex = partition(arr, left, right);
            quickSort(arr, left, partitionIndex - 1);
            quickSort(arr, partitionIndex + 1, right);
        }
    }

    private static int partition(int[] arr, int left, int right) {
        int pivot = arr[left];
        //终止while循环以后left和right一定相等的
        while (left < right) {
            while (left < right && arr[right] >= pivot) {
                --right;
            }
            arr[left] = arr[right];
            while (left < right && arr[left] <= pivot) {
                ++left;
            }
            arr[right] = arr[left];
        }
        arr[left] = pivot;
        //right可以改为left
        return left;
    }

}
```

**上面是有两种实现代码**

## 5.7 归并排序

### 5.7.1 归并排序算法基本介绍



归并排序（MERGE-SORT）是利用归并的思想实现的排序方法，该算法采用经典的分治（devide-and-conquer）策略（分治法将问题分(devide)成一些小的问题，然后再递归求解，而治(conquer)的阶段则将分的阶段得到各答案修补在一起，即分而治之。

![image-20210330090336661](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210330090336661.png)

**说明：**可以看到这种结构很像一棵完全二叉树，本文的归并排序我们采用递归去实现，（也可以使用迭代的方式去实现）。分阶段可以理解为就是递归拆分子序列的过程。

![image-20210330091200075](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210330091200075.png)

### 5.7.2 代码的实现

```java
public class MergeSort {
    public static void main(String[] args) {
        int[] arr = {8, 4, 5, 7, 1, 3, 6, 2};
        int[] temp = new int[arr.length];
        mergedSort(arr, 0, arr.length - 1, temp);
        System.out.println(Arrays.toString(arr));
    }

    //分加合并的算法
    public static void mergedSort(int[] arr, int left, int right, int[] temp) {
        if (left < right) {
            int mid = (left + right) / 2;
            //向左递归进行分解
            mergedSort(arr, left, mid, temp);
            //向右递归进行分解
            mergedSort(arr, mid + 1, right, temp);
            //合并
            merge(arr, left, mid, right, temp);
        }
    }


    //合并的方法
    public static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        System.out.println("~~~~");
        int i = left; //初始化i，左边有序序列的初始索引
        int j = mid + 1; //初始化j，右边有序序列的初始索引
        int t = 0; //指向temp数组的当前索引

        //（一）
        //先把左右两边（有序）的数组按照规则填充到temp数组
        //直到左右两边的有序序列，有一边处理完毕
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[t] = arr[i];
                t += 1;
                i += 1;
            } else {
                temp[t] = arr[j];
                t += 1;
                j += 1;
            }
        }
        //（二）
        //把有剩余数据的一边的数据依次全部填充到temp

        while (i <= mid) {
            //左边的有序序列还有剩余的元素，就全部填充到temp
            temp[t] = arr[i];
            t += 1;
            i += 1;
        }
        while (j <= right) {
            //右边的有序序列还有剩余的元素，就全部填充到temp
            temp[t] = arr[j];
            t += 1;
            j += 1;
        }

        //（三)
        //将temp数组的元素拷贝到arr
        //注意，并不是每次都拷贝所有
        t = 0;
        int tempLeft = left;
        while (tempLeft <= right) {
            arr[tempLeft] = temp[t];
            t += 1;
            tempLeft += 1;
        }
    }
}
```

**运行的结果**

```
~~~~
~~~~
~~~~
~~~~
~~~~
~~~~
~~~~
[1, 2, 3, 4, 5, 6, 7, 8]
```

合并的次数就是n-1

## 5.8 基数排序

### 5.8.1 基数排序的基本知识

1. 基数排序（radix sort）属于分配式排序（distribution sort）又称桶子法或bin sort，顾名思义，它是通过键值的各个位的键，将要排序的元素分配至某些桶中，达到排序的作用。
2. 基数排序是属于稳定性的排序，基数排序法是效率高的稳定性排序法。
3. 基数排序是桶排序的扩展
4. 基数排序是1887年赫尔曼发明的，它是这样实现的：将整数按位数切割成不同的数字，然后按每个位数分别比较。

 **基数排序的基本思想**

1. 将所有待比较数值统一为同样数位长度，数位较短的数前面补0，然后从低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后，数列就变成了一个有序序列。
2. 看一个图文解释，理解基数排序的步骤。

![image-20210330110131298](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210330110131298.png)

**第二轮之后是十位**

**第三轮是百位**

**轮数是按照最高位数来判断的**

### 5.8.2 代码实现

```java
public class RadixSort {
    public static void main(String[] args) {
        int[] arr = {53, 3, 542, 748, 14, 214};
        radixSort(arr);
    }

    //基数排序的方法
    public static void radixSort(int[] arr) {

        //根据前面的推导过程，我们可以得到最终的基数排序代码
        //得到数组中最大的数的位数
        int max = arr[0];
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        //得到最大的数是几位数
        int maxLength = (max + "").length();


        //定义一个二维数组，表示10个桶就是一个一维数组
        //说明
        //1. 二维数组包含十个一维数组
        //2. 为了防止在放入数的时候，数据溢出，则每一个一位数组（桶），大小定义为arr.length
        //3. 这是一种典型的空间换时间的算法
        int[][] bucket = new int[10][arr.length];

        //为了记录每个桶中，实际存放了多少个数据，定义一个一维数组来记录各个桶的每次放入的数据个数
        int[] bucketElementCounts = new int[10];


        //使用循环处理代码
        for (int i = 0, n = 1; i < maxLength; i++, n *= 10) {
            for (int j = 0; j < arr.length; j++) {

                int digitOfElement = arr[j] /n % 10;
                //放入到对用的桶中
                bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
                bucketElementCounts[digitOfElement]++;
            }
            //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来的数组）
            int index = 0;
            //遍历每一个桶，并将桶中的数据放入到原数组
            for (int k = 0; k < bucketElementCounts.length; k++) {
                //如果桶中有数据才放入到原数组
                if (bucketElementCounts[k] != 0) {
                    for (int l = 0; l < bucketElementCounts[k]; l++) {
                        arr[index] = bucket[k][l];
                        index++;
                    }
                }
                //第一轮处理结束后，需要将各个bucketElemtCounts[k] = 0;
                bucketElementCounts[k] = 0;
            }

            System.out.println("第"+(i+1)+"轮，对个位数的排序处理 arr = " + Arrays.toString(arr));
        }

        /*
        //第一轮
        for (int j = 0;j < arr.length;j++){
            //取出每个元素个位数的值
            int digitOfElement = arr[j] % 10;
            //放入到对用的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement]++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来的数组）
        int index = 0;
        //遍历每一个桶，并将桶中的数据放入到原数组
        for (int k = 0;k < bucketElementCounts.length; k++){
            //如果桶中有数据才放入到原数组
            if (bucketElementCounts[k] != 0){
                for (int l =0;l < bucketElementCounts[k];l++){
                    arr[index] = bucket[k][l];
                    index++;
                }
            }
            //第一轮处理结束后，需要将各个bucketElemtCounts[k] = 0;
            bucketElementCounts[k] = 0;
        }

        System.out.println("第1轮，对个位数的排序处理 arr = "+ Arrays.toString(arr));

//===============================================================================
        //第二轮
        for (int j = 0;j < arr.length;j++){
            //取出每个元素个位数的值
            int digitOfElement = arr[j] /10 % 10;
            //放入到对用的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement]++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来的数组）
         index = 0;
        //遍历每一个桶，并将桶中的数据放入到原数组
        for (int k = 0;k < bucketElementCounts.length; k++){
            //如果桶中有数据才放入到原数组
            if (bucketElementCounts[k] != 0){
                for (int l =0;l < bucketElementCounts[k];l++){
                    arr[index] = bucket[k][l];
                    index++;
                }
            }

            bucketElementCounts[k] = 0;
        }

        System.out.println("第2轮，对十位数的排序处理 arr = "+ Arrays.toString(arr));

//=====================================================================================

        //第二轮
        for (int j = 0;j < arr.length;j++){
            //取出每个元素个位数的值
            int digitOfElement = arr[j] /100 % 10;
            //放入到对用的桶中
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
            bucketElementCounts[digitOfElement]++;
        }
        //按照这个桶的顺序（一维数组的下标依次取出数据，放入原来的数组）
        index = 0;
        //遍历每一个桶，并将桶中的数据放入到原数组
        for (int k = 0;k < bucketElementCounts.length; k++){
            //如果桶中有数据才放入到原数组
            if (bucketElementCounts[k] != 0){
                for (int l =0;l < bucketElementCounts[k];l++){
                    arr[index] = bucket[k][l];
                    index++;
                }
            }

            bucketElementCounts[k] = 0;
        }

        System.out.println("第3轮，对百位数的排序处理 arr = "+ Arrays.toString(arr));

         */
    }

}
```

**结果**

```
第1轮，对个位数的排序处理 arr = [542, 53, 3, 14, 214, 748]
第2轮，对个位数的排序处理 arr = [3, 14, 214, 542, 748, 53]
第3轮，对个位数的排序处理 arr = [3, 14, 53, 214, 542, 748]
```

### 5.8.3 基数排序的注意事项

1. 基数排序是对传统桶排序的扩展，速度很快
2. 基数排序是经典的空间换时间的方式，占用的内存很大，当对海量数据排序的时候，容易造成 OutOfMemoryError
3. 基数排序是稳定的。【注：假定在待排序的记录序列中，存在多个具有相同的关键字的记录，若经过排序，这些记录的相对次序保持不变，即在原序列中，r[i]=r[j]，且r[i]在r[j]之前，而在排序后的序列中，r[i]依旧在r[j]之前，则称这种排序算法是稳定的，否则是不稳定的。
4. **有负数的数组，我们不用基数排序来进行排序，如果想要支持负数，请查看：https://code.i-harness.com/zh-CN/q/e98fa9**

## 5.9 排序算法比较

![image-20210330145705820](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210330145705820.png)



# 6. 查找算法

在java中我们常用的查找算法是：

1. 顺序（线性）查找
2. 二分查找/折半查找
3. 插值查找
4. 斐波那契查找

## 6.1 线性查找

**代码实现**

```java
public class SeqSearch {
    public static void main(String[] args) {
        int[] arr = {1,9,11,-1,34,80};
        int index = seqSearch(arr,11);
        if (index == -1){
            System.out.println("没有找到");
        } else {
            System.out.println("找到了，下标为 "+index);
        }

    }
    public static int seqSearch(int[] arr,int value){
        //线性查找是逐一对比，发现有相同的值，就返回下标
        for (int i = 0;i < arr.length; i++){
            if (arr[i] == value){
                return i;
            }
        }
        return -1;
    }
}
```

**结果**

```
找到了，下标= 2
```

## 6.2  二分查找

**只针对有序数列查找**

**思路分析**

1. 首先确定该数组的中间的下标：mid=(left+right)/2

2. 然后让需要查找的数findval和arr[mid]进行比较

   2.1 findval > arr[mid]，说明你需要查找的数在mid的右边，因此需要递归的向右查找

   2.2 findval < arr[mid]，说明你需要查找的数在mid的左边，因此需要递归的向左查找

   2.3 find == arr[mid]，说明找到，直接返回

3. 什么时候结束递归呢

   3.1 找到就结束递归

   3.2 递归完整个数组，仍然没有找到findval，也需要结束递归当left > right就需要退出。

### 6.2.1 二分查找的基本版

**对于要查找的数据没有出现重复的现象**

```java
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {1,8,10,89,1000,1234};
        int resIndex = binarySearch(arr,0,arr.length-1,1234);
        System.out.println("resIndex="+resIndex);

    }

    //二分查找算法
    public static int binarySearch(int[] arr,int left,int right,int findVal){

        //当left > right时，说明递归整个数组，但是没有找到
        if (left > right){
            return -1;
        }
        int mid = (left+right)/2;
        int midVal = arr[mid];

        if (findVal > midVal){
            //向右递归
            return binarySearch(arr,mid+1,right, findVal);
        } else if (findVal < midVal){
            return binarySearch(arr,left,mid-1,findVal);
        } else {
            return mid;
        }
    }
}
```

**结果**

```
resIndex=5
```

### 6.2.2 二分查找的高级版

```java
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {1, 8, 10, 89, 1000, 1000, 1000, 1234};
        List<Integer> resIndex = binarySearch2(arr, 0, arr.length - 1, 1000);
        System.out.println("resIndex=" + resIndex);

    }

    //二分查找算法
    public static int binarySearch(int[] arr, int left, int right, int findVal) {

        //当left > right时，说明递归整个数组，但是没有找到
        if (left > right) {
            return -1;
        }
        int mid = (left + right) / 2;
        int midVal = arr[mid];

        if (findVal > midVal) {
            //向右递归
            return binarySearch(arr, mid + 1, right, findVal);
        } else if (findVal < midVal) {
            return binarySearch(arr, left, mid - 1, findVal);
        } else {
            return mid;
        }
    }

    //将表中所有需要查找的数都找出来
    //1. 在找到mid索引值，不要马上返回
    //2. 向mid 索引值的左边扫描，将所有满足1000，的元素的下标加入到集合ArrayList
    //3. 向mid 索引值的右边扫描，将所有满足1000，的元素的下标加入到集合ArrayList
    //4. 将ArrayList返回
    public static ArrayList<Integer> binarySearch2(int[] arr, int left, int right, int findVal) {
        //当left > right时，说明递归整个数组，但是没有找到
        if (left > right) {
            return new ArrayList<Integer>();
        }
        int mid = (left + right) / 2;
        int midVal = arr[mid];

        if (findVal > midVal) {
            //向右递归
            return binarySearch2(arr, mid + 1, right, findVal);
        } else if (findVal < midVal) {
            return binarySearch2(arr, left, mid - 1, findVal);
        } else {
            ArrayList<Integer> resIndexlist = new ArrayList<Integer>();
            //向mid 索引值的左边扫描，将所有满足1000，的元素的下标，加入到集合ArrayList
            int temp = mid - 1;
            while (true) {
                if (temp < 0 || arr[temp] != findVal) {
                    break;
                }
                //否则，将temp放入到resIndexList
                resIndexlist.add(temp);
                temp -= 1;
            }
            resIndexlist.add(mid);

            //向mid 索引值的右边扫描，将所有满足1000，的元素的下标，加入到集合ArrayList
            temp = mid + 1;
            while (true) {
                if (temp > arr.length - 1 || arr[temp] != findVal) {
                    break;
                }
                //否则，将temp放入到resIndexList
                resIndexlist.add(temp);
                temp += 1;
            }
            return resIndexlist;
        }

    }
}
```

**结果**

```
resIndex=[4, 5, 6]
```

## 6.3 插值查找

### 6.3.1 插值查找的基本原理

插值查找算法类似于二分查找，不同的是插值查找每次从自适应mid开始查找

关于mid的定义如下：

![image-20210331082304226](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331082304226.png)

**插值查找算法的举例说明**

数组arr = [1,2,3,.......,100]

假如我们要查找的是1

使用二分查找的话需要多此递归，才能找到1

使用插值查找算法

int mid = left + (left - right)*(findVal - arr[left])/(arr[right] - arr[left])

int mid = 0 + (99 - 0)*(1 - 1)/(100 - 1) = 0

同理我们如果找的是100则

int mid = 0 + (99 - 0)*(100 - 1)/(100 - 1) = 99

### 6.3.2 插值查找的代码实现

```java
public class InsertValueSearch {
    public static void main(String[] args) {
        int[] arr = new int[100];
        for (int i = 0; i < 100; i++) {
            arr[i] = i + 1;
        }
        int index = insertVlaueSearch(arr, 0, arr.length - 1, 10);
        System.out.println("index = " + index);
    }

    //编写插值算法的
    //说明：插值查找算法，也是要求数组是有序的
    public static int insertVlaueSearch(int[] arr, int left, int right, int findVal) {
        System.out.println("~~~~~~~~");
        if (left > right || findVal < arr[0] || findVal > arr[arr.length - 1]) {
            return -1;
        }
        int mid = left + (right - left) * (findVal - arr[left]) / (arr[right] - arr[left]);
        int midVal = arr[mid];
        if (findVal > midVal) {
            //说明数组应该向左查询
            return insertVlaueSearch(arr, mid + 1, right, findVal);
        } else if (findVal < midVal) {
            return insertVlaueSearch(arr, left, mid - 1, findVal);
        } else {
            return mid;
        }
    }
}
```

**结果**

```
~~~~~~~~
index = 9
```

**插值查找的注意事项**

1. 对于数据量较大，关键字分布比较均匀的查找表来说，采用插值查找，速度比较快
2. 关键字分布不均匀的情况下，该方法不一定比折半查找的效果好。

## 6.4 斐波那契（黄金分割）查找算法

### 6.4.1 斐波那契查找的基本介绍

1. 黄金分割点是指把一条线段分割成两部分，使其中一部分与全长之比等于另一部分与这部分之比。取其前三位数字的近似值是0.618。由于按此比例设计的造型十分美丽，因此称为黄金分割，也称中外比。这是一个神奇的数字，会带来意想不到的效果。
2. 斐波那契数列{1，1，2，3，5，8，13，21，34，55}发现斐波那契数列的两个相邻数的比例，无限接近黄金分割值0.618。

![image-20210331091714277](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331091714277.png)

### 6.4.2 代码实现

```
public class FibonacciSearch {
    public static int maxSize = 20;

    public static void main(String[] args) {
        int[] arr = {1, 8, 10, 89, 1000, 1234};
        System.out.println("index=" + fibSearch(arr, 1234));
    }

    //因为我们后面mid = low + F(k-1)-1，需要使用到斐波那契数列，因此我们直接创建一个斐波那契数列
    //非递归方法得到一个斐波那契数列
    public static int[] fib() {
        int[] f = new int[maxSize];
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i < maxSize; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        return f;
    }

    //编写斐波那契查找算法
    //使用非递归的方式编写算法
    public static int fibSearch(int[] a, int key) {
        int low = 0;
        int high = a.length - 1;
        int k = 0; //表示斐波那契分割数值的下标
        int mid = 0; //存放mid值
        int f[] = fib(); //获取到斐波那契数列

        //获取到斐波那契分割数值的下标
        while (high > f[k] - 1) {
            k++;
        }

        //因为f[k]值可能大于a的长度，因此我们需要使用Arrays类，构造一个新的数组，并指向a[]
        //不足的部分使用0进行填充
        int[] temp = Arrays.copyOf(a, f[k]);
        //上面这一部分是将值进行0填补，但是我们希望的是将填补的数值转换为该数组的最高值
        for (int i = high + 1; i < temp.length; i++) {
            temp[i] = a[high];
        }
        //使用while循环处理，找到我们的数hey
        while (low <= high) {
            mid = low + f[k - 1] - 1;
            //只要满足这个条件，就可以开始找
            if (key < temp[mid]) {
                //此时我们应该继续向前找（左边）
                high = mid - 1;
                k--;
                /*
                 * 说明一下为什么是k--
                 * 1. 全部元素 = 前面的元素 + 后面的元素
                 * 2. f[k[ = f[k-1] + f[k-2]
                 * 3. 因为前面有f[k-1]个元素，所以可以继续拆分f[k-1] = f[k-2] + f[k-3]
                 * 即在f[k-1]前面继续查找k--
                 * */
            } else if (key > temp[mid]) {
                low = mid + 1;
                k -= 2;
                //因为后面我们有f[k-2] 所以可以继续拆分为f[k-1] = f[k-3] + f[k-4]
            } else {
                //找到了
                //需要确定我们需要返回的是哪个值
                if (mid <= high) {
                    return mid;
                } else {
                    return high;
                }
            }
        }
        return -1;
    }

}
```

```
index=5
```

### 6.4.3 难点理解

**为什么后面需要k-2**

mid的取值要求的是黄金分割点，意思就是说要保证mid之前的数字比例和mid之后的数字比例是0.618，我们是为了方便才设定了一个斐波那契数列，就是说这个数列已经将如何取值数组值得小标进行了规范，我们也可以自己去计算，但是直接从数组取值会更加的方便。

我们现在查找的数字排序如下：

![image-20210331141610229](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331141610229.png)

按照第一步执行的我们找到了mid值为4

![image-20210331142109251](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331142109251.png)

但是此时我们需要往前走，这个时候会发现此时数据被分成两部分，

![image-20210331142413032](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331142413032.png)

此时我们拿出斐波那契数列按照之前的八个数据，我们得到的数列是：1，1，2，3，5，8

此时第一部分对应的是 5 ，第二部分对应的是 3 。这样我们在进入第二部分判定的时候，需要找出中值就是2。因为2/3才能保证是黄金分割点。其实就是往前面移动一位。

# 7. 哈希表

## 7.1 哈希表的基本介绍

散列表（Hash Table，也叫哈希表），是根据关键码值（Key value）而直接进行访问的数据结构，也就是说它通过把关键码映射到表中的一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

![image-20210331162926288](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331162926288.png)

![image-20210331164103398](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331164103398.png)

## 7.2 代码实现

Google公司的一个上机题：

有一个公司，当有新的员工来报道的时候，要求将该员工的信息加入（id，性别，年龄，住址），当输入该员工的id时，要求查找到该员工的所有信息。

要求：不是用数据库，速度越快越好，添加时候保证id是从低到高的顺序插入。

**思路分析**

![image-20210331165321879](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210331165321879.png)

**代码如下：**

```java
public class HashTabDemo {
    public static void main(String[] args) {
        //创建哈希表
        HashTab hashTab = new HashTab(7);

        //写一个简单的菜单
        String key = "";
        Scanner scanner = new Scanner(System.in);
        while (true){
            System.out.println("add:  添加雇员");
            System.out.println("list； 显示雇员");
            System.out.println("find:  查找雇员");
            System.out.println("exit:  退出系统");

            key = scanner.next();
            switch (key){
                case "add":
                    System.out.println("输入id");
                    int id = scanner.nextInt();
                    System.out.println("输入名字");
                    String name = scanner.next();
                    //创建雇员
                    Emp emp = new Emp(id,name);
                    hashTab.add(emp);
                    break;
                case "list":
                    hashTab.list();
                    break;
                case "exit":
                    scanner.close();
                    System.exit(0);
                    break;
                case "find":
                    System.out.println("请输入要查找的id");
                    id = scanner.nextInt();
                    hashTab.findEmpById(id);
                    break;
                default:
                    break;
            }
        }
    }
}

//创建HashTab管理多条链表
class HashTab {
    private EmpLinkedList[] empLinkedListArray;
    private int size; //表示有多少条链表

    //构造器
    public HashTab(int size) {
        this.size = size;
        //初始化empLiknedListArray
        empLinkedListArray = new EmpLinkedList[size];
        //!!!!!!特别需要注意的是：：：这个时候要分别初始化每个链表
        for (int i = 0; i < size; i++){
            empLinkedListArray[i] = new EmpLinkedList();
        }
    }

    //添加雇员
    public void add(Emp emp) {
        //根据员工的id，得到该员工应当添加到哪条链表
        int empLinkedListNO = hashFun(emp.id);
        //将emp添加到对应的链表中
        empLinkedListArray[empLinkedListNO].add(emp);
    }

    //遍历所有的数据，遍历hashtab
    public void list() {
        for (int i = 0; i < size; i++) {
            empLinkedListArray[i].list(i);
        }
    }

    //根据输入的id，查找到雇员
    public void findEmpById(int id){
        //使用散列函数确定到哪条链表查找
        int empLinkedListNO = hashFun(id);
        Emp emp = empLinkedListArray[empLinkedListNO].findEmpById(id);
        if (emp != null){
            //找到
            System.out.printf("在第%d条链表中找到雇员id = %d\n",(empLinkedListNO+1),id);
        } else {
            System.out.println("在哈希表中，没有找到该雇员");
        }
    }

    //编写散列函数，使用一个简单的取模方法
    public int hashFun(int id) {
        return id % size;
    }
}


//表示一个雇员
class Emp {
    public int id;
    public String name;
    public Emp next;

    public Emp(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }
}


//创建EmpLinkedList，表示链表
class EmpLinkedList {
    //头指针，执行第一个Emp，因此我们这个链表的head是直接指向第一个Emp
    private Emp head; //默认为null
    //添加雇员到链表
    //说明：假定，当添加雇员时，id是自增长，即id的分配总是从小到大，因此我们将该雇员直接加入到本链表的最后即可
    public void add(Emp emp) {
        //如果是添加第一个雇员
        if (head == null) {
            head = emp;
            return;
        }
        //如果不是第一个雇员，刚使用一个辅助指针，帮助定位到最后
        Emp curEmp = head;
        while (true) {
            if (curEmp.next == null) {
                //说明到链表最后
                break;
            }
            curEmp = curEmp.next;
        }
        //退出时直接将emp加入到链表
        curEmp.next = emp;
    }

    //遍历链表的雇员信息
    public void list(int no) {
        if (head == null) {
            //说明链表为空
            System.out.println("第"+(no+1)+"链表为空");
            return;
        }
        System.out.print("第"+(no+1)+"链表的信息为");
        Emp curEmp = head; //辅助指针
        while (true) {
            System.out.printf("==> id=%d name=%s\t", curEmp.id, curEmp.name);
            if (curEmp.next == null) {
                //说明cueEmp已经是最后节点
                break;
            }
            curEmp = curEmp.next;
        }
        System.out.println();
    }
    //根据id查找雇员
    //如果找到就返回Emp，如果没有找到，就返回null
    public Emp findEmpById(int id){
        //判断链表是否为空
        if (head == null){
            System.out.println("链表为空");
            return null;
        }
        //辅助指针
        Emp curEmp = head;
        while (true){
            if (curEmp.id == id){
                break;//说明这个时候已经找到
            }
            //退出
            if (curEmp.next == null){
                //说明没有找到
                curEmp = null;
                break;
            }
            curEmp = curEmp.next;
        }
        return curEmp;
    }
}
```

# 8. 树结构的基本知识

## 8.1 为什么需要树这种数据结构

1. **数组存储方式的分析：**

   优点：通过小标方式访问元素，速度快，对于有序数组，还可以使用二分查找提高检索速度。

   缺点：如果要检索具体某个值，或者插入值（按照一定顺序）会整体移动，效率低

2. **链式存储方式的分析**

   优点：在一定程度上对数组存储方式有优化（比如：插入一个数值节点，只需要将插入节点，连接到链表中即可，删除效率也很高）。

   缺点：在进行检索的时候，效率仍然很低，比如（检索某个值，需要从头节点开始遍历）

3. **树存储方式的分析**

   能提高数据存储，读取的效率，比如利用二叉排序树(Binary Sort Tree)，既可以保证数据的检索速度，同时也可以保证数据的插入，删除，修改的速度。

**分析二叉排序树**

![image-20210401125952349](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210401125952349.png)

## 8.2 二叉树的概念和常用术语

![image-20210401130700479](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210401130700479.png)

**二叉树的概念**

1. 树有很多种，每个节点最多只能有两个子节点的一种形式叫做二叉树。
2. 二叉树的子节点分为左节点和右节点

![image-20210401131815539](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210401131815539.png)

3. 如果该二叉树的所有叶子节点都在最后一层，并且节点总数是2^-1，n为层数，则我们称为满二叉树。
4. 如果该二叉树的所有叶子节点都在最后一层或者倒数第二层，而且最后一层的叶子节点在左边连续，倒数第二层的叶子节点在右边连续，我们称为完全二叉树。

![image-20210401143621262](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210401143621262.png)



## 8.3 二叉树的遍历说明

前序遍历：**先输出父节点，**再遍历左子树和右子树

中序遍历：先遍历左子树，**再输出父节点，**最后遍历右子树

后序遍历：先遍历左子树，再遍历右子树，**最后输出父节点**

**小结：**

看输出父节点的顺序，就确定是前序，中序还是后序。

**二叉树分析**

1. 创建一个二叉树

2. 前序遍历

   2.1 先输出当前节点（初始的时候是root节点）

   2.2 如果左节点不为空则递归继续前序遍历

   2.3 如果右节点不为空则递归继续前序遍历

3. 中序遍历

   3.1 如果当前节点的左子节点不为空，则递归中序遍历

   3.2 输出当前节点

   3.3 如果当前节点的右子节点不为空，则递归中序遍历

4. 后序遍历

   4.1 如果当前节点的左子节点不为空，则递归后序遍历

   4.2 如果当前节点的右子节点不为空，则递归后序遍历

   4.3 输出当前节点

## 8.4 二叉树代码实现

### 8.4.1 前序中序后序的代码实现

```java
public class BinaryTreeDemo {
    public static void main(String[] args) {
        //先创建一个二叉树
        BinaryTree binaryTree = new BinaryTree();
        //需要创建的节点
        HeroNode root = new HeroNode(1,"松江");
        HeroNode node2 = new HeroNode(2,"松江");
        HeroNode node3 = new HeroNode(3,"松江");
        HeroNode node4 = new HeroNode(4,"松江");
        //说明，我们先手动创建该二叉树，后面学习递归的方式创建二叉树
        root.setLeft(node2);
        root.setRight(node3);
        node3.setRight(node4);
        binaryTree.setRoot(root);

        //测试
        System.out.println("前序遍历");
        binaryTree.preOrder();

        System.out.println("中序遍历");
        binaryTree.infixOrde();

        System.out.println("后序遍历");
        binaryTree.postOrder();

    }
}
//定义一个二叉树
class BinaryTree{
    private HeroNode root;
    public void setRoot(HeroNode root){
        this.root = root;
    }
    //前序遍历
    public void preOrder(){
        if (this.root != null){
            this.root.preOrder();
        } else {
            System.out.println("二叉树为空无法遍历");
        }
    }
    //中序遍历
    public void infixOrde(){
        if (this.root != null){
            this.root.infixOrder();
        } else {
            System.out.println("二叉树为空，无法遍历");
        }
    }
    //后序遍历
    public void postOrder(){
        if (this.root != null){
            this.root.postOrder();
        } else {
            System.out.println("二叉树为空，无法遍历");
        }
    }
}

//先创建HeroNode节点
class HeroNode{
    private int no;
    private String name;
    private HeroNode left;  //默认为null
    private HeroNode right; //默认为null
    public HeroNode(int no,String name){
        this.no = no;
        this.name = name;
    }
    public void setNo(int no){
        this.no = no;
    }
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public HeroNode getLeft(){
        return left;
    }
    public void setLeft(HeroNode left){
        this.left = left;
    }
    public HeroNode getRight(){
        return right;
    }
    public void setRight(HeroNode right){
        this.right = right;
    }
    public String toString(){
        return "HeroNode [no="+no+",name="+name+"]";
    }

    //编写前序遍历的方法
    public void preOrder(){
        System.out.println(this); //先输出父节点
        //递归向左子树前序遍历
        if (this.left != null){
            this.left.preOrder();
        }
        //递归向右子树遍历
        if (this.right != null){
            this.right.preOrder();
        }
    }
    //中序遍历
    public void infixOrder(){
        //递归向左子树中序遍历
        if (this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归右子树中序遍历
        if (this.right != null){
            this.right.infixOrder();
        }
    }
    //后序遍历
    public void postOrder(){
        if (this.left != null){
            this.left.postOrder();
        }
        if (this.right != null){
            this.right.postOrder();
        }
        System.out.println(this);
    }
}
```

**结果实现**

```
前序遍历
HeroNode [no=1,name=松江]
HeroNode [no=2,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=4,name=松江]
中序遍历
HeroNode [no=2,name=松江]
HeroNode [no=1,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=4,name=松江]
后序遍历
HeroNode [no=2,name=松江]
HeroNode [no=4,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=1,name=松江]
```

### 8.4.2 查找指定的节点

**要求**

1. 请编写前序查找，中序查找和后序查找的方法
2. 并分别使用三种查找方式，查找HeroNO = 5的节点
3. 并分析各种查找方式，分别比较了多少次

**思路分析**

前序查找思路

1. 先判断当前节点的no是否等于要查找的
2. 如果相等返回当前节点
3. 如果不相等，则判断当前节点的左子节点是否为空，如果不为空则递归前序查找
4. 如果左递归前序查找，找到节点，则返回，否则继续查找，当前的节点的右子节点是否为空，如果不为空的话则继续由递归前序查找

中序查找思路

1. 判断当前节点的左子节点是否为空 ，如果不为空则递归中序查找
2. 如果找到则返回，如果没有找到的话，就和当前节点比较，如果是则返回当前节点，否则的话继续进行由递归中序查找
3. 如果右递归中序查找，找到的话就返回，否则的话返回null

后序查找思路

1. 判断当前节点的左子节点是否为空，如果不为空，则递归后序查找
2. 如果找到，就返回，如果没有找到就判断当前节点的右子节点是否为空，如果不为空，则右递归进行后序查找，如果找到就返回。
3. 就和当前节点进行比较，如果是则返回，否则的话直接返回null。

```java
public class BinaryTreeDemo {
    public static void main(String[] args) {
        //先创建一个二叉树
        BinaryTree binaryTree = new BinaryTree();
        //需要创建的节点
        HeroNode root = new HeroNode(1,"松江");
        HeroNode node2 = new HeroNode(2,"松江");
        HeroNode node3 = new HeroNode(3,"松江");
        HeroNode node4 = new HeroNode(4,"松江");
        HeroNode node5 = new HeroNode(5,"松江");
        //说明，我们先手动创建该二叉树，后面学习递归的方式创建二叉树
        root.setLeft(node2);
        root.setRight(node3);
        node3.setRight(node4);
        node3.setLeft(node5);
        binaryTree.setRoot(root);

        //测试
        System.out.println("前序遍历");
        binaryTree.preOrder();

        System.out.println("中序遍历");
        binaryTree.infixOrde();

        System.out.println("后序遍历");
        binaryTree.postOrder();

        //搜索遍历查找
//        System.out.println("前序遍历方式~~~~~");
//        HeroNode resNode = binaryTree.preOrderSearch(5);
//        if (resNode != null){
//            System.out.printf("已经找到了，no=%d,name=%s",resNode.getNo(),resNode.getName());
//        } else {
//            System.out.println("没有查找到相关信息");
//        }

//        System.out.println("中序遍历方式~~~~~");
//        HeroNode resNode = binaryTree.infixOrderSearch(5);
//        if (resNode != null){
//            System.out.printf("已经找到了，no=%d,name=%s",resNode.getNo(),resNode.getName());
//        } else {
//            System.out.println("没有查找到相关信息");
//        }

        System.out.println("后序遍历方式~~~~~");
        HeroNode resNode = binaryTree.postOrderSearch(5);
        if (resNode != null){
            System.out.printf("已经找到了，no=%d,name=%s",resNode.getNo(),resNode.getName());
        } else {
            System.out.println("没有查找到相关信息");
        }
    }
}
//定义一个二叉树
class BinaryTree{
    private HeroNode root;
    public void setRoot(HeroNode root){
        this.root = root;
    }
    //前序遍历
    public void preOrder(){
        if (this.root != null){
            this.root.preOrder();
        } else {
            System.out.println("二叉树为空无法遍历");
        }
    }
    //中序遍历
    public void infixOrde(){
        if (this.root != null){
            this.root.infixOrder();
        } else {
            System.out.println("二叉树为空，无法遍历");
        }
    }
    //后序遍历
    public void postOrder(){
        if (this.root != null){
            this.root.postOrder();
        } else {
            System.out.println("二叉树为空，无法遍历");
        }
    }


    //搜索遍历
    public HeroNode preOrderSearch(int no){
        if (root != null){
            return root.preOrderSearch(no);
        } else {
            return null;
        }
    }
    public HeroNode infixOrderSearch(int no){
        if (root != null){
            return root.infixOrderSearch(no);
        } else {
            return null;
        }
    }
    public HeroNode postOrderSearch(int no){
        if (root != null){
            return root.postOrderSearch(no);
        } else {
            return null;
        }
    }
}

//先创建HeroNode节点
class HeroNode{
    private int no;
    private String name;
    private HeroNode left;  //默认为null
    private HeroNode right; //默认为null
    public HeroNode(int no,String name){
        this.no = no;
        this.name = name;
    }
    public int getNo(){
        return no;
    }
    public void setNo(int no){
        this.no = no;
    }
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public HeroNode getLeft(){
        return left;
    }
    public void setLeft(HeroNode left){
        this.left = left;
    }
    public HeroNode getRight(){
        return right;
    }
    public void setRight(HeroNode right){
        this.right = right;
    }
    public String toString(){
        return "HeroNode [no="+no+",name="+name+"]";
    }

    //编写前序遍历的方法
    public void preOrder(){
        System.out.println(this); //先输出父节点
        //递归向左子树前序遍历
        if (this.left != null){
            this.left.preOrder();
        }
        //递归向右子树遍历
        if (this.right != null){
            this.right.preOrder();
        }
    }
    //中序遍历
    public void infixOrder(){
        //递归向左子树中序遍历
        if (this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归右子树中序遍历
        if (this.right != null){
            this.right.infixOrder();
        }
    }
    //后序遍历
    public void postOrder(){
        if (this.left != null){
            this.left.postOrder();
        }
        if (this.right != null){
            this.right.postOrder();
        }
        System.out.println(this);
    }
    //前序遍历查找
    public HeroNode preOrderSearch(int no){
        System.out.println("进入前序遍历：：：：");
        //比较当前节点是不是
        if (this.no == no){
            return this;
        }
        //1. 判断当前节点的左子节点是否为空，如果不为空就递归前序查找
        //2. 如果左递归前序查找，找到节点，则返回
        HeroNode resNode = null;
        if (this.left != null){
            resNode = this.left.preOrderSearch(no);
        }
        if (resNode != null){
            //说明左子树找到了
            return resNode;
        }
        //1. 左递归前序查找，找到节点，则返回，否则继续判断
        //2. 当前系欸但的右子节点是否为空，如果不为空，则继续向右递归前序查找
        if (this.right != null){
            resNode = this.right.preOrderSearch(no);
        }
        return resNode;//不管有没有都要返回了，如果没有的话返回的是null。
    }

    //中序遍历查找
    public HeroNode infixOrderSearch(int no){

        HeroNode resNode = null;
        if (this.left != null){
            resNode = this.left.infixOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        //如果找到就直接返回，如果没有找到，就和当前节点比较，如果是就返回当前节点
        System.out.println("进入中序遍历：：：：");
        if (this.no == no){
            return this;
        }
        if (this.right != null){
            resNode = this.right.infixOrderSearch(no);
        }
        return resNode;
    }

     //后遍历查找
    public HeroNode postOrderSearch(int no){
        HeroNode resNode = null;
        if (this.left != null){
            resNode = this.left.postOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        if (this.right != null){
            resNode = this.right.postOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        System.out.println("进入后序遍历：：：：");
        if (this.no == no){
            return this;
        }
        return resNode;
    }
}
```

**结果**

```
前序遍历
HeroNode [no=1,name=松江]
HeroNode [no=2,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=5,name=松江]
HeroNode [no=4,name=松江]
中序遍历
HeroNode [no=2,name=松江]
HeroNode [no=1,name=松江]
HeroNode [no=5,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=4,name=松江]
后序遍历
HeroNode [no=2,name=松江]
HeroNode [no=5,name=松江]
HeroNode [no=4,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=1,name=松江]
后序遍历方式~~~~~
进入后序遍历：：：：
进入后序遍历：：：：
已经找到了，no=5,name=松江
```

### 8.4.3 二叉树删除节点

**要求：**

1. 如果删除的节点是叶子节点，则删除该节点
2. 如果删除的节点是非叶子节点，则删除该子树
3. 测试，删除掉5号叶子节点和3号子树

**思路：**

1. 因为我们的二叉树是单向的，所以我们是判断当前节点的子节点是否是需要删除的节点，而不能去判断当前这个节点是不是需要删除节点
2. 如果当前节点的左子节点不为空，并且左子节点就是要删除节点，就将this.left = null；并且返回（结束递归删除）。
3. 如果当前节点的右子节点不为空，并且右子节点就是要删除节点，就将this.right = null；并且就返回（结束递归删除）。
4. 如果第2步和第3步没有删除节点，那么我们就需要向左子树进行递归删除，
5. 如果第4步也没有删除节点，则应当向右子树进行递归删除。
6. 考虑如果树是空树root，如果只有一个root节点，则等价将二叉树置空。

**在HeroNode中编写删除的代码**

```java
    //递归删除节点
    //1. 如果删除的是叶子节点，则删除该节点
    //2. 如果删除的节点是非叶子节点，则删除该子树
    public void delNode(int no){
        //如果当前节点的左子节点不为空，并且左子节点就是要删除的点
        if (this.left != null && this.left.no == no){
            this.left = null;
            return;
        }
        //如果当前节点的右子节点不为空，并且右子节点就是要删除的点
        if (this.right != null && this.right.no == no){
            this.right = null;
            return;
        }
        //如果上面都没有要删除的节点
        if (this.left != null){
            this.left.delNode(no);
        }
        if (this.right != null){
            this.right.delNode(no);
        }
    }
```

**我觉得this很关键**

**在binaryTree里面编写删除方法**

```java
    //删除节点
    public void delNode(int no){
        if (root != null){
            if (root.getNo() == no){
                root = null;
            } else {
                root.delNode(no);
            }
        } else {
            System.out.println("这个二叉树是个空树");
        }
    }
```

**运行结果**

```
删除前，前序遍历~~~
HeroNode [no=1,name=松江]
HeroNode [no=2,name=松江]
HeroNode [no=3,name=松江]
HeroNode [no=5,name=松江]
HeroNode [no=4,name=松江]
删除后，前序遍历~~~
HeroNode [no=1,name=松江]
HeroNode [no=2,name=松江]
```

### 8.4.4 顺序存储二叉树

**基本说明**

从数据存储来看，数据存储方式和树的存储方式可以相互转换，树也可以转换成数组，

要求：

1. 二叉树的节点，可以以数组的方式来存放 arr:[1,2,3,4,5,6,7]
2. 要求在遍历数组arr的时候，仍然可以以前序遍历，中序遍历和后序遍历的方式完成节点的遍历

![image-20210403101535883](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210403101535883.png)

**顺序存储二叉树的特点**

1. 顺序二叉树通常只考虑完全二叉树
2. 第n个元素的左子节点为 2*n+1
3. 第n个元素的右子节点为2*n+2
4. 第n个元素的父节点为(n-1)/2
5. n：表示二叉树的第几个元素（按0开始编号）

**要求**：给一个数组[1,2,3,4,5,6,7]，要求以二叉树前遍历的方式进行遍历，前遍历的结果应当为1，2，4，5，3，6，7

## 8.5 线索化二叉树

**问题分析**

![image-20210403151152295](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210403151152295.png)

1. 当我们对上面的二叉树进行中序遍历时，数列为{8，3，10，1，6，14}
2. 但是6，8，10，14这几个节点的左右指针，并没有完全利用上
3. 如果我们希望充分的利用各个节点的左右指针，让各个节点可以指向自己的前后节点怎么办？
4. **可以使用线索二叉树来实现**

**线索二叉树的基本介绍**

1. n个节点的二叉链表中含有n+1个空指针域。利用二叉链表中的空指针域，存放指向该节点在某种遍历次序下的前驱和后继节点的指针（这种附加的指针称为“线索”
2. 这种加上线索的二叉链表称为线索链表，相应的二叉树称为线索二叉树（Thread Binary Tree）。根据线索性质的不同，线索二叉树可以分为前序线索二叉树，中序线索二叉树和后序线索二叉树三种
3. 一个节点的前一个节点，称为前驱节点
4. 一个节点的后一个节点，称为后继节点

![image-20210403154712100](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210403154712100.png)

**说明：当线索化二叉树后，Node节点的属性lef和right，有如下情况：**

1. left指向的是左子树，也可能是指向的前驱节点，比如①节点left指向的左子树，而⑩节点的left指向的就是前驱节点
2. right指向的是右子树，也可能是指向后继节点，比如①节点right指向的就是右子树，而⑩节点的right指向的是后继节点。

### 8.5.1 线索二叉树的代码

**代码实现**

```java
//定义ThreadedBinaryTree 实现了线索化功能的二叉树
class ThreadedBinaryTree {
    public static void main(String[] args) {

        // 测试一把中序线索二叉树的功能
        HeroNode root = new HeroNode(1, "tom");
        HeroNode node2 = new HeroNode(3, "jack");
        HeroNode node3 = new HeroNode(6, "smith");
        HeroNode node4 = new HeroNode(8, "mary");
        HeroNode node5 = new HeroNode(10, "king");
        HeroNode node6 = new HeroNode(14, "dim");

        // 二叉树，后面我们要递归创建, 现在简单处理使用手动创建
        root.setLeft(node2);
        root.setRight(node3);
        node2.setLeft(node4);
        node2.setRight(node5);
        node3.setLeft(node6);

        // 测试中序线索化
        ThreadedBinaryTree threadedBinaryTree = new ThreadedBinaryTree();
        threadedBinaryTree.setRoot(root);
        threadedBinaryTree.threadedNodes();

        // 测试: 以10号节点测试
        HeroNode leftNode = node5.getLeft();
        HeroNode rightNode = node5.getRight();
        System.out.println("10号结点的前驱结点是 =" + leftNode); // 3
        System.out.println("10号结点的后继结点是=" + rightNode); // 1

    }


    private HeroNode root;

    // 为了实现线索化，需要创建一个指向当前结点的前驱结点的指针
    // 在递归进行线索化时，pre 总是保留前一个结点
    private HeroNode pre = null;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    // 重载一把threadedNodes方法
    public void threadedNodes() {
        this.threadedNodes(root);
    }

    // 编写对二叉树进行中序线索化的方法
    // node 就是当前需要线索化的结点
    public void threadedNodes(HeroNode node) {

        // 如果node==null, 不能线索化
        if (node == null) {
            return;
        }

        // (一)先线索化左子树
        threadedNodes(node.getLeft());

        // (二)线索化当前结点[有难度]
        // 处理当前结点的前驱结点
        // 以8结点来理解
        // 8结点的.left = null , 8结点的.leftType = 1
        if (node.getLeft() == null) {
            // 让当前结点的左指针指向前驱结点
            node.setLeft(pre);
            // 修改当前结点的左指针的类型,指向前驱结点
            node.setLeftType(1);
        }

        // 处理后继结点
        if (pre != null && pre.getRight() == null) {
            // 让前驱结点的右指针指向当前结点
            pre.setRight(node);
            // 修改前驱结点的右指针类型
            pre.setRightType(1);
        }
        // !!! 每处理一个结点后，让当前结点是下一个结点的前驱结点
        pre = node;

        // (三)在线索化右子树
        threadedNodes(node.getRight());

    }
}


//先创建HeroNode 结点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left; // 默认null
    private HeroNode right; // 默认null
    // 说明
    // 1. 如果leftType == 0 表示指向的是左子树, 如果 1 则表示指向前驱结点
    // 2. 如果rightType == 0 表示指向是右子树, 如果 1表示指向后继结点
    private int leftType;
    private int rightType;

    public int getLeftType() {
        return leftType;
    }

    public void setLeftType(int leftType) {
        this.leftType = leftType;
    }

    public int getRightType() {
        return rightType;
    }

    public void setRightType(int rightType) {
        this.rightType = rightType;
    }

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode [no=" + no + ", name=" + name + "]";
    }

}
```

**结果**

```
10号结点的前驱结点是 =HeroNode [no=3, name=jack]
10号结点的后继结点是=HeroNode [no=1, name=tom]
```

### 8.5.2 遍历线索二叉树

**说明：**对前面的中序线索化的二叉树，进行遍历

**分析：**因为线索化后，各个节点指向有变化，因此原来的遍历方式是不可以使用的，这个时候需要使用新的方式遍历线索化二叉树，各个节点可以通过线索型方式遍历，因此无需使用递归方式，这样也提高了遍历的效率，遍历的次序应当和中序保持一致。

```java
  //遍历线索化二叉树的方法
    public void threadedList(){
        //定义一个变量，存储当前遍历的节点，从root开始
        HeroNode node = root;
        while(node != null){
            //循环的找到leftType == 1的节点，第一个找到的就是节点8
            //后面随着遍历而发生变化，因为当leftType == 1时，说明该节点是按照线索化
            //处理后的有效节点
            while (node.getLeftType() == 0){
                node = node.getLeft();
            }
            //打印当前这个节点
            System.out.println(node);
            //如果当前节点的右指针指向的是后继节点，就一直输出
            while (node.getRightType() == 1){
                //获取当前节点的后继节点
                node = node.getRight();
                System.out.println(node);
            }
            //如果不是后继节点的话，直接替换这个遍历的节点
            node = node.getRight();
        }
    }
```

结果

```
使用线索化的方法遍历线索化二叉树~~~~~
HeroNode [no=8, name=mary]
HeroNode [no=3, name=jack]
HeroNode [no=10, name=king]
HeroNode [no=1, name=tom]
HeroNode [no=14, name=dim]
HeroNode [no=6, name=smith]
```

# 9. 树结构实际应用

## 9.1 堆排序

### 9.1.1 堆排序的基本原理

1. 堆排序是利用堆这种数据结构而设计的一种排序算法，堆排序是一种**选择排序**，它的最坏，最好，平均时间复杂度均为O(nlogn)，它也是不稳定排序
2. 堆是具有一下性质的完全二叉树：每个节点的值都大于或等于其左右子节点的值，称为大顶堆。**注意**：没有要求节点的左右子节点的值的大小关系。
3. 每个节点的值都小于或者等于其左右子节点的值，称为小顶堆

**大顶堆**

![image-20210404111241068](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404111241068.png)

我们对堆中的节点按层进行编号，映射到数组中就是下面这个样子：其特点是，arr[i] >= arr[2 * i + 1] && arr[i] >= arr[2 * i + 2]

![image-20210404111356926](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404111356926.png)

**小顶堆**

![image-20210404111546393](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404111546393.png)

小顶堆的特点是：arr[i] <= arr[2 * i + 1] && arr[i] <= arr[2 * i + 1]。**一般降序采用的是小顶堆，升序使用的是大顶堆**

**堆排序的基本思想**

1. 将待排序序列构造成一个大顶堆
2. 此时整个序列的最大值就是堆顶的根节点
3. 将其与末尾元素进行交换，此时末尾就为最大值
4. 然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如次反复操作，便能得到一个有序序列

可以看到在构建大顶堆的过程中，元素的个数在逐渐减少，最后得到的就是有序序列。

### 9.1.2 堆排序的过程

**步骤一：构造初始堆，将给定无序序列构造成一个大顶堆（一般升序采用大顶堆，降序采用小顶堆）**

1）假设给定无序序列结构如下：

![image-20210404142446293](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404142446293.png)

2）此时我们从最后一个非叶子节点开始（叶节点自然不需要调整，最后一个非叶子节点的计算是：arr.length/2-1=1，也就是下面的6节点），从左至右，从下至上进行调整。

![image-20210404143205856](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404143205856.png)

3）找到第二个非叶子节点4，由于【4，9，8】中9元素最大，4和9交换

![image-20210404143401946](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404143401946.png)

4）上一步的交换导致字根[4,5,6]结构混乱，继续调整，其中4和6进行交换

![image-20210404143538032](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404143538032.png)

此时我们就将一个无序序列构建成了一个大顶堆

**步骤二：将堆顶元素和末尾元素进行交换，使末尾元素最大。然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素，如此进行交换，重建和交换**

1）将堆顶元素9和末尾元素4进行交换

![image-20210404143943049](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404143943049.png)

2）重新调整结构，使其继续满足堆定义

![image-20210404144120631](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404144120631.png)

3）在将堆顶元素8与末尾元素5进行交换，得到第二大元素

![image-20210404144216025](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404144216025.png)

4）后续过程，继续调整，交换，如此反复进行，最终使得整个序列有序

![image-20210404144355264](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210404144355264.png)

**再简单总结下堆排序的基本思路：**

1. 将一个无序序列构建成一个堆，根据升序降序需要选择大顶堆或者小顶堆
2. 将堆顶元素与末尾元素进行交换，使最大元素“沉”到数组末端
3. 重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换，直到整个序列有序。

### 9.1.3 代码实现

```java
public class HeapSort {
    public static void main(String[] args) {
        //要求将数组进行升序排序
        int[] arr = {4, 6, 8, 5, 9};
        heapSort(arr);
    }

    //编写一个堆排序的方法
    public static void heapSort(int arr[]) {
        int temp = 0;
        System.out.println("堆排序！！！！");

        //分步完成
//        adjustHeap(arr, 1, arr.length);
//        System.out.println("第一次" + Arrays.toString(arr));

        //完成我们的最终代码
        for (int i = arr.length / 2 - 1; i >= 0; i--) {
            adjustHeap(arr, i, arr.length);
        }

        //1. 将堆顶元素与末尾元素进行交换，将最大的元素沉到数组末端
        //2. 重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整交换，直到整个序列有序
        for (int j = arr.length-1;j > 0; j--){
            //交换
            temp = arr[j];
            arr[j] = arr[0];
            arr[0] = temp;
            adjustHeap(arr,0,j);
        }
        System.out.println("数组=" + Arrays.toString(arr));
    }

    //将一个数组（二叉树）调整成一个大顶堆
    //i 表示非叶子节点再数组中的索引
    //length表示堆多少个元素继续调整,而且数值再逐渐的减少
    public static void adjustHeap(int[] arr, int i, int length) {
        int temp = arr[i];  //先取出当前元素的值，保存在临时变量
        //开始调整
        //说明：其中k = i * 2 + 1; k 是 i 节点的左子节点
        for (int k = i * 2 + 1; k < length; k = k * 2 + 1) {
            if (k + 1 < length && arr[k] < arr[k + 1]) {
                //说明左子系欸但小于右子节点的值
                k++; //将k指向右子节点
            }
            if (arr[k] > temp) {
                //如果子节点大于父节点
                arr[i] = arr[k];  //把较大的值赋值给当前节点
                i = k;  //！！！！i指向k，继续循环比较
            } else {
                break;
            }
        }
        //for循环结束后，我们已经将i 为父节点的树的最大值，放在了最顶部（局部）
        arr[i] = temp;
    }
}
```

**结果的实现**

```
堆排序！！！！
数组=[4, 5, 6, 8, 9]
```

**这个速度很快，八百万的数据只需要三秒，复杂度是O(nlogn)**

## 9.2 赫夫曼树

### 9.2.1 赫夫曼树的原理

**基本介绍**

1. 给定n个权值作为n个叶子节点，构造一棵二叉树，若该树的带权路径长度(wpl)达到最小，称这样的二叉树为最优二叉树，也称为是哈夫曼树
2. 哈夫曼树是带权路径长度最短的树，权值较大的节点离根较近

**赫夫曼树几个重要的概念**

1. 路径和路径的长度：在一棵树中，从一个节点往下可以达到子节点或者孙节点之间的通路，称为路径。通路中分支的数目称为路径长度，若规定根节点的层数为1，则从根节点到第L层节点的路径为L-1
2. **节点的权以及带权路径长度**：若将树中节点赋值给一个有着某种含义的数值，则这个数值称为该节点的权。**节点的带权路径长度：**从根节点到该节点之间的路径航都与该节点的权的乘积。
3. **树的带权路径长度：**树的带权路径长度规定为所有叶子节点的带权路径长度之和，记为WPL(weighted path length)，权值越大的节点离根节点越近的二叉树才是最优二叉树。
4. WPL最小的就是赫夫曼树。

![image-20210405095159497](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405095159497.png)

### 9.2.2 赫夫曼树创建的思路

**给一个数组{13，7，8，3，29，6，1}，要求转换称一个赫夫曼树**

**构建赫夫曼树的步骤**

1. 从小到大进行排序，将每一个数据，每一个数据都是一个节点，每个节点可以看成是一棵简单的二叉树，
2. 取出根节点权值最小的两颗二叉树
3. 组成一颗新的二叉树，该新的二叉树的根节点的权值是前面两棵二叉树根节点权值的和
4. 在将这棵新的二叉树，以根节点的权值大小再次排序，不断重复上面1~3的步骤，直到数列中，所有的数据都被处理，就得到一颗二叉树。

![image-20210405100913933](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405100913933.png)

### 9.2.3 代码实现

```java
public class HuffmanTree {
    public static void main(String[] args) {
        int[] arr = {13, 7, 8, 3, 29, 6, 1};
        Node root= creatHuffmanTree(arr);
        root.preOrder();
    }

    //编写一个前序遍历的方法
    public static void preOrder(Node root){
        if (root != null){
            root.preOrder();
        } else {
            System.out.println("这尼玛是空树，编写你妹呢");
        }
    }

    //创建赫夫曼树的方法
    public static Node creatHuffmanTree(int[] arr) {
        //第一步为了操作方便
        //1. 遍历arr数组
        //2. 将arr的每个元素构成一个Node
        //3. 将Node放入到Array List中
        List<Node> nodes = new ArrayList<Node>();
        for (int value : arr) {
            nodes.add(new Node(value));
        }

        //我们处理的过程是一个循环的过程
        while (nodes.size() > 1) {
            //从小到大排序
            Collections.sort(nodes);
            System.out.println("ndoes = " + nodes);

            //取出根节点权值最小的两棵二叉树
            //(1) 取出权值最小的节点（二叉树）
            Node leftNode = nodes.get(0);
            //(2) 取出权值第二小的节点（二叉树）
            Node rightNode = nodes.get(1);
            //(3) 构建一颗新的二叉树
            Node parent = new Node(leftNode.value + rightNode.value);
            parent.left = leftNode;
            parent.right = rightNode;
            //(4)从ArrayList删除处理过的二叉树
            nodes.remove(leftNode);
            nodes.remove(rightNode);
            nodes.add(parent);
        }
        //返回的是赫夫曼的root节点
        return nodes.get(0);
    }
}

//创建节点类，为了让Node对象支持排序Collection集合排序
//让Node实现Comparable接口
class Node implements Comparable<Node> {
    int value;  //节点权值
    Node left;  //指向左节点
    Node right;  //指向右节点

    //前序遍历
    public void preOrder(){
        System.out.println(this);
        if (this.left != null){
            this.left.preOrder();
        }
        if (this.right != null){
            this.right.preOrder();
        }
    }
    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node[value=" + value + "]";
    }

    @Override
    public int compareTo(Node o) {
        //表示从小到大排序
        return this.value - o.value;
    }
}
```

**结果**

```
ndoes = [Node[value=1], Node[value=3], Node[value=6], Node[value=7], Node[value=8], Node[value=13], Node[value=29]]
ndoes = [Node[value=4], Node[value=6], Node[value=7], Node[value=8], Node[value=13], Node[value=29]]
ndoes = [Node[value=7], Node[value=8], Node[value=10], Node[value=13], Node[value=29]]
ndoes = [Node[value=10], Node[value=13], Node[value=15], Node[value=29]]
ndoes = [Node[value=15], Node[value=23], Node[value=29]]
ndoes = [Node[value=29], Node[value=38]]
Node[value=67]
Node[value=29]
Node[value=38]
Node[value=15]
Node[value=7]
Node[value=8]
Node[value=23]
Node[value=10]
Node[value=4]
Node[value=1]
Node[value=3]
Node[value=6]
Node[value=13]
```

## 9.3 赫夫曼编码

### 9.3.1 赫夫曼编码的原理介绍

**基本介绍**

1. 赫夫曼编码也翻译成哈夫曼编码，是一种编码方式，属于一种程序算法
2. 赫夫曼编码是赫夫曼树在电讯通信中经典的应用
3. 赫夫曼编码广泛应用于数据文件压缩，其压缩率通常在20%~90%之间
4. 赫夫曼是可变字长编码的一种，是1952年提出的一种编码方式，称之为最佳编码

**定长编码**

![image-20210405112141945](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405112141945.png)

**变长编码**

![image-20210405112231133](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405112231133.png)

**霍夫曼编码**

1. 需要传输的字符串是： i like like like java do you like a java
2. 统计各个字符串出现的次数： d:1 y:1 u:1 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9
3. 按照上面字符出现的次数 [1,1,2,2,4,4,4,5,5,9] 构建一棵赫夫曼树，次数作为权值。

![image-20210405142336915](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405142336915.png)

4. 根据赫夫曼树，给各个字符规定编码（前缀编码），向左的路径为0向右的路径为1，编码完成之后：

o:1000  u:10010 d:100110 y:100111 i:101 a:110 k:1110 e:1111 j:0000 v:0001 l:001  :01

5. 按照上面的编码介绍我们可以得到，需要传输的字串编码为：

![image-20210405143008214](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405143008214.png)

**说明**

1. 原来的长度是359，现在的长度是133
2. 此编码满足前缀编码，即字符的编码都不能是其他编码的前缀。不会造成匹配的多义性

**特别注意**

注意这个赫夫曼树根据排序方法不同，也可能不太一样，这样对应的赫夫曼编码也不完全一样，但是WPL是一样的，都是最小的，比如：如果我们让每次生成的新的二叉树总是排在权值相同的二叉树的最后一个，则生成的二叉树为：

![image-20210405143920045](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405143920045.png)

总的长度133是不会发生变化的。

### 9.3.2 数据压缩（创建赫夫曼树）

![image-20210405144253046](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210405144253046.png)

**思路**

1. Node {data(存放数据)，weight(权值)，left和right}
2. 得到'i like like like java do you like a java' 对应的byte[] 数组
3. 编写一个方法，将准备构建赫夫曼树的Node节点放到List，形似如[Node[data=97,weight=5], Node[]data=32,weight=9]........]，要体现出的是：d:1 y:1 u:1 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9
4. 可以通过List创建对应的赫夫曼树

**代码实现**

```java
public class HuffmanCode {
    public static void main(String[] args) {
        String content = "i like like like java do you like a java";
        byte[] contentBytes = content.getBytes();
        System.out.println(contentBytes.length); //40

        List<Node> nodes = getNodes(contentBytes);
        System.out.println("nodes="+nodes);

        //测试一把，创建的二叉树
        System.out.println("赫夫曼树");
        Node huffmanTree = creatHuffmanTree(nodes);
        System.out.println("前序遍历");
        huffmanTree.preOrder();

    }

    private static List<Node> getNodes(byte[] bytes){
        //创建一个ArrayList
        ArrayList<Node> nodes = new ArrayList<Node>();

        //遍历byte，统计每一个byte出现的次数 => map[key,value]
        Map<Byte,Integer> counts = new HashMap<>();
        for (byte b : bytes){
            Integer count = counts.get(b);
            if (count == null){
                //map还没有这个字符数据，第一次
                counts.put(b,1);
            } else {
                counts.put(b,count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入到nodes集合
        //遍历map
        for (Map.Entry<Byte,Integer> entry: counts.entrySet()){
            nodes.add(new Node(entry.getKey(),entry.getValue()));
        }
        return nodes;
    }

    //可以通过List 创建对应的赫夫曼树
    private static Node creatHuffmanTree(List<Node> nodes){
        while (nodes.size() > 1){
            //排序从小到大
            Collections.sort(nodes);
            //取出第一棵最小的二叉树
            Node leftNode = nodes.get(0);
            Node rightNode = nodes.get(1);
            //取出一个新的二叉树，它的根节点没有data，只有权值
            Node parent = new Node(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;

            //将已经处理的两颗二叉树从Nodes删除
            nodes.remove(leftNode);
            nodes.remove(rightNode);
            nodes.add(parent);
        }
        //nodes最后的节点，就是赫夫曼树的根节点
        return nodes.get(0);
    }

    //前序遍历的方法
    private static void preOrder(Node root){
        if (root != null){
            root.preOrder();
        } else {
            System.out.println("这他妈是空的，写尼玛呢");
        }
    }
}


//创建Node，其中加入的有数据和权值
class Node implements Comparable<Node> {
    Byte data;  //存放数据（字符）本身，比如'a'  => 97
    int weight;  //权值，表示字符出现的次数
    Node left;
    Node right;

    public Node(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    public String toString() {
        return "Node [data =" + data + "weight = " + weight + "]";
    }

    @Override
    public int compareTo(Node o) {
        return this.weight - o.weight;
    }

    //前序遍历
    public void preOrder(){
        System.out.println(this);
        if (this.left != null){
            this.left.preOrder();
        }
        if (this.right != null){
            this.right.preOrder();
        }
    }
}
```

**运行结果**

```
40
nodes=[Node [data =32weight = 9], Node [data =97weight = 5], Node [data =100weight = 1], Node [data =101weight = 4], Node [data =117weight = 1], Node [data =118weight = 2], Node [data =105weight = 5], Node [data =121weight = 1], Node [data =106weight = 2], Node [data =107weight = 4], Node [data =108weight = 4], Node [data =111weight = 2]]
赫夫曼树
前序遍历
Node [data =nullweight = 40]
Node [data =nullweight = 17]
Node [data =nullweight = 8]
Node [data =108weight = 4]
Node [data =nullweight = 4]
Node [data =106weight = 2]
Node [data =111weight = 2]
Node [data =32weight = 9]
Node [data =nullweight = 23]
Node [data =nullweight = 10]
Node [data =97weight = 5]
Node [data =105weight = 5]
Node [data =nullweight = 13]
Node [data =nullweight = 5]
Node [data =nullweight = 2]
Node [data =100weight = 1]
Node [data =117weight = 1]
Node [data =nullweight = 3]
Node [data =121weight = 1]
Node [data =118weight = 2]
Node [data =nullweight = 8]
Node [data =101weight = 4]
Node [data =107weight = 4]
```

### 9.3.3 生成赫夫曼编码

**此时我们已经完成了赫夫曼树，下面继续完成任务**

![image-20210406100855830](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210406100855830.png)

代码实现

```java
//生成赫夫曼树对应的赫夫曼编码
    //1, 将赫夫曼编码表存放在Map<Byte,String>形式
    //形式如：32-->01  97-->100  100--->11000
    static Map<Byte,String> huffmanCodes = new HashMap<Byte,String>();
    //2. 在生成赫夫曼编码表示的时候，需要去拼接路径，定义一个StringBuilder存储某个子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    //为了调用方便，我们重载getCodes
    private static Map<Byte,String> getCode(Node root){
        if (root == null){
            return null;
        }

        //处理root的左子树
        getCode(root.left,"0",stringBuilder);
        getCode(root.right,"1",stringBuilder);
        return huffmanCodes;
    }

    //功能：将传入的node节点的所有叶子节点的赫夫曼编码得到，并放入到huffmanCodes集合
    //node：传入节点
    //code：路径，左子节点是0，右子节点是1
    //StringBuilder 用于拼接路劲
    private static void getCode(Node node, String code, StringBuilder stringBuilder){
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code加入到stringbuilder2
        stringBuilder2.append(code);
        if (node != null){
            //如果node == null不处理，判断当前node是叶子节点还是非叶子节点
            if (node.data == null){
                //非叶子节点，递归处理，
                //向左递归
                getCode(node.left,"0",stringBuilder2);
                getCode(node.right,"1",stringBuilder2);
            } else {
                //说明是一个叶子节点，就表示找到某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }
}
```

测试调用

```java
//测试生成的赫夫曼编码
        Map<Byte,String> huffmanCodes = getCode(huffmanTreeRoot);
        System.out.println("生成的赫夫曼编码表"+huffmanCodes);
```

运行的结果

```
生成的赫夫曼编码表{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011}
```

**使用赫夫曼编码生成赫夫曼编码数据，按照上面的赫夫曼编码，字符串生成对应的编码数据，然后存储在数组中**

```java
    private static byte[] zip(byte[] bytes,Map<Byte,String> huffmanCodes){
        //1. 利用huffmanCodes将bytes转换成赫夫曼对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes数组
        for (byte b : bytes){
            stringBuilder.append(huffmanCodes.get(b));
        }
        //将合并后的赫夫曼字符串拼接成长的字符，101010001011111110.。。。抓换成byte[]
        //统计返回 byte[] huffmanCodeBytes长度
        int len;
        if (stringBuilder.length() % 8 == 0){
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建存储压缩后的byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;  //记录是第几个byte
        for (int i = 0;i < stringBuilder.length(); i+=8){
            String strByte;
            if (i+8 > stringBuilder.length()){
                //此时是不够八位的
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i,i+8);
            }
            //将strByte换成一个byte，放入到huffmanCodeBytes
            huffmanCodeBytes[index] = (byte)Integer.parseInt(strByte,2);
            index++;
        }

        return huffmanCodeBytes;

    }
```

**测试代码**

```
//测试
        byte[] huffmanCodeBytes = zip(contentBytes,huffmanCodes);
        System.out.println("huffmanCodeBytes="+Arrays.toString(huffmanCodeBytes));
```

**运行的结果**

```
huffmanCodeBytes=[-88, -65, -56, -65, -56, -65, -55, 77, -57, 6, -24, -14, -117, -4, -60, -90, 28]
```

### 9.3.4 将方法进行完善

```java
//使用一个方法，将前面的方法封装起来，便于我们的调用
    //bytes 原始的字符串对应的字节数组
    //返回的是经过赫夫曼编码处理后的字节数组（压缩后的数组）
    private static byte[] huffmanZip(byte[] bytes){
        List<Node> nodes = getNodes(bytes);
        //根据nodes 创建的赫夫曼树
        Node huffmanTreeRoot = creatHuffmanTree(nodes);
        //对应的赫夫曼编码（根据赫夫曼树）
        Map<Byte,String> huffmanCodes = getCode(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes,huffmanCodes);
        return huffmanCodeBytes;
    }
```

**运行代码**

```java
String content = "i like like like java do you like a java";
        byte[] contentBytes = content.getBytes();
        System.out.println(contentBytes.length); //40
        byte[] huffmanCodesBytes = huffmanZip(contentBytes);
        System.out.println("压缩后的结果是："+Arrays.toString(huffmanCodesBytes));
```

结果

```
40
压缩后的结果是：[-88, -65, -56, -65, -56, -65, -55, 77, -57, 6, -24, -14, -117, -4, -60, -90, 28]
```

### 9.3.5 赫夫曼解码

**具体要求**

1. 前面我们得到赫夫曼编码和对应的解码byte[]，即[-88, -65, -56, -65, -56, -65, -55, 77, -57, 6, -24, -14, -117, -4, -60, -90, 28]
2. 现在要求使用赫夫曼编码，进行解码，又会重新得到原来的字符串"i like like like java do you like a java"

```java
public class HuffmanCode {
    public static void main(String[] args) {
        String content = "i like like like java do you like a java";
        byte[] contentBytes = content.getBytes();
        System.out.println(contentBytes.length); //40
        byte[] huffmanCodesBytes = huffmanZip(contentBytes);
        System.out.println("压缩后的结果是：" + Arrays.toString(huffmanCodesBytes));

        byte[] sourceBytes = decode(huffmanCodes, huffmanCodesBytes);
        System.out.println("原来的字符串=" + new String((sourceBytes)));

/*
        List<Node> nodes = getNodes(contentBytes);
        System.out.println("nodes="+nodes);

        //测试一把，创建的二叉树
        System.out.println("赫夫曼树");
        Node huffmanTreeRoot = creatHuffmanTree(nodes);
        System.out.println("前序遍历");
        huffmanTreeRoot.preOrder();

        //测试生成的赫夫曼编码
        Map<Byte,String> huffmanCodes = getCode(huffmanTreeRoot);
        System.out.println("生成的赫夫曼编码表"+huffmanCodes);

        //测试
        byte[] huffmanCodeBytes = zip(contentBytes,huffmanCodes);
        System.out.println("huffmanCodeBytes="+Arrays.toString(huffmanCodeBytes));

 */

    }


    //完成数据的解压
    //1. 将huffmanCodeBytes[-88, -65, -56, -65, -56, -65, -55, 77, -57, 6, -24, -14, -117, -4, -60, -90, 28]
    //  重写先转换成 赫夫曼编码对应的二进制字符串"1010100010111......"
    //2. 赫夫曼拜编码对应的二进制的字符串=====>对照赫夫曼编码=========>生成i like like like java do you like a java


    //编写一个方法，完成对压缩数据的解码
    //1. huffmanCodes 赫夫曼编码表map
    //2. huffmanbytes 赫夫曼编码得到的字节数组
    //返回的是原来字符串对应的数组

    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        //1. 先得到huffmanBytes对应的二进制的字符串，形式如：1010100010111....
        StringBuilder stringBuilder = new StringBuilder();
        //将byte数组转换成二进制的字符串
        for (int i = 0; i < huffmanBytes.length - 1; i++) {
            byte b = huffmanBytes[i];
            //判断是不是最后一个字节
            boolean flag = (i == huffmanBytes.length - 1);
            stringBuilder.append(byteToBitString( b));
        }

        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询a->100, 100->a
        Map<String, Byte> map = new HashMap<String, Byte>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }

        //创建要给集合，存放byte
        List<Byte> list = new ArrayList<>();
        //i 可以理解成就是索引，扫描stringBuilder
        for (int i = 0; i < stringBuilder.length();) {
            int count = 1;  //小的计数器
            boolean flag = true;
            Byte b = null;

            while (flag) {
                //递增的取出keyi++
                String key = stringBuilder.substring(i, i + count);  //i不动让count移动，指定匹配到一个字符
                b = map.get(key);
                if (b == null) {
                    //说明没有匹配到
                    count++;
                } else {
                    //匹配到
                    flag = false;
                }
            }
            list.add(b);
            i += count;  //i直接移动到count
        }
        //当for循环之后，我们在list中就存放了所有的字符
        //把list中的数据放入到byte[] 并返回
        byte[] b = new byte[list.size()];
        for (int i = 0; i < b.length; i++) {
            b[i] = list.get(i);
        }
        return b;
    }

    //将一个byte转成一个二进制的字符串，如果看不懂，可以参考我们所讲的原码，反码，补码
    //b是传入的byte
    //flag 标志是否需要补高位，如果是true则需要补高位，如果是false则不需要补高位，如果是最后一个字节无需补高位
    //b 是对应的二进制的字符串，（注意是按补码返回）

    private static String byteToBitString(byte b) {
        // 使用变量保存 b
        int temp = b; // 将 b 转成 int
        temp |= 0x100; // 如果是正数我们需要将高位补零
        // 转换为二进制字符串，正数：高位补 0 即可，然后截取低八位即可；负数直接截取低八位即可
        // 负数在计算机内存储的是补码，补码转原码：先 -1 ，再取反
        String binaryStr = Integer.toBinaryString(temp);
        return binaryStr.substring(binaryStr.length() - 8);
    }



    //使用一个方法，将前面的方法封装起来，便于我们的调用
    //bytes 原始的字符串对应的字节数组
    //返回的是经过赫夫曼编码处理后的字节数组（压缩后的数组）
    private static byte[] huffmanZip(byte[] bytes) {
        List<Node> nodes = getNodes(bytes);
        //根据nodes 创建的赫夫曼树
        Node huffmanTreeRoot = creatHuffmanTree(nodes);
        //对应的赫夫曼编码（根据赫夫曼树）
        Map<Byte, String> huffmanCodes = getCode(huffmanTreeRoot);
        //根据生成的赫夫曼编码，压缩得到压缩后的赫夫曼编码字节数组
        byte[] huffmanCodeBytes = zip(bytes, huffmanCodes);
        return huffmanCodeBytes;
    }

    private static List<Node> getNodes(byte[] bytes) {
        //创建一个ArrayList
        ArrayList<Node> nodes = new ArrayList<Node>();

        //遍历byte，统计每一个byte出现的次数 => map[key,value]
        Map<Byte, Integer> counts = new HashMap<>();
        for (byte b : bytes) {
            Integer count = counts.get(b);
            if (count == null) {
                //map还没有这个字符数据，第一次
                counts.put(b, 1);
            } else {
                counts.put(b, count + 1);
            }
        }

        //把每一个键值对转成一个Node对象，并加入到nodes集合
        //遍历map
        for (Map.Entry<Byte, Integer> entry : counts.entrySet()) {
            nodes.add(new Node(entry.getKey(), entry.getValue()));
        }
        return nodes;
    }

    //可以通过List 创建对应的赫夫曼树
    private static Node creatHuffmanTree(List<Node> nodes) {
        while (nodes.size() > 1) {
            //排序从小到大
            Collections.sort(nodes);
            //取出第一棵最小的二叉树
            Node leftNode = nodes.get(0);
            Node rightNode = nodes.get(1);
            //取出一个新的二叉树，它的根节点没有data，只有权值
            Node parent = new Node(null, leftNode.weight + rightNode.weight);
            parent.left = leftNode;
            parent.right = rightNode;

            //将已经处理的两颗二叉树从Nodes删除
            nodes.remove(leftNode);
            nodes.remove(rightNode);
            nodes.add(parent);
        }
        //nodes最后的节点，就是赫夫曼树的根节点
        return nodes.get(0);
    }

    //前序遍历的方法
    private static void preOrder(Node root) {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("这他妈是空的，写尼玛呢");
        }
    }


    private static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {
        //1. 利用huffmanCodes将bytes转换成赫夫曼对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        //遍历bytes数组
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));
        }
        //将合并后的赫夫曼字符串拼接成长的字符，101010001011111110.。。。抓换成byte[]
        //统计返回 byte[] huffmanCodeBytes长度
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建存储压缩后的byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;  //记录是第几个byte
        for (int i = 0; i < stringBuilder.length(); i += 8) {
            String strByte;
            if (i + 8 > stringBuilder.length()) {
                //此时是不够八位的
                strByte = stringBuilder.substring(i);
            } else {
                strByte = stringBuilder.substring(i, i + 8);
            }
            //将strByte换成一个byte，放入到huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strByte, 2);
            index++;
        }

        return huffmanCodeBytes;

    }


    //生成赫夫曼树对应的赫夫曼编码
    //1, 将赫夫曼编码表存放在Map<Byte,String>形式
    //形式如：32-->01  97-->100  100--->11000
    static Map<Byte, String> huffmanCodes = new HashMap<Byte, String>();
    //2. 在生成赫夫曼编码表示的时候，需要去拼接路径，定义一个StringBuilder存储某个子节点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    //为了调用方便，我们重载getCodes
    private static Map<Byte, String> getCode(Node root) {
        if (root == null) {
            return null;
        }

        //处理root的左子树
        getCode(root.left, "0", stringBuilder);
        getCode(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    //功能：将传入的node节点的所有叶子节点的赫夫曼编码得到，并放入到huffmanCodes集合
    //node：传入节点
    //code：路径，左子节点是0，右子节点是1
    //StringBuilder 用于拼接路劲
    private static void getCode(Node node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code加入到stringbuilder2
        stringBuilder2.append(code);
        if (node != null) {
            //如果node == null不处理，判断当前node是叶子节点还是非叶子节点
            if (node.data == null) {
                //非叶子节点，递归处理，
                //向左递归
                getCode(node.left, "0", stringBuilder2);
                getCode(node.right, "1", stringBuilder2);
            } else {
                //说明是一个叶子节点，就表示找到某个叶子节点的最后
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }
}


//创建Node，其中加入的有数据和权值
class Node implements Comparable<Node> {
    Byte data;  //存放数据（字符）本身，比如'a'  => 97
    int weight;  //权值，表示字符出现的次数
    Node left;
    Node right;

    public Node(Byte data, int weight) {
        this.data = data;
        this.weight = weight;
    }

    public String toString() {
        return "Node [data =" + data + "weight = " + weight + "]";
    }

    @Override
    public int compareTo(Node o) {
        return this.weight - o.weight;
    }

    //前序遍历
    public void preOrder() {
        System.out.println(this);
        if (this.left != null) {
            this.left.preOrder();
        }
        if (this.right != null) {
            this.right.preOrder();
        }
    }
}
```

### 9.3.6 文件压缩

**要求**

学习通过赫夫曼编码对一个字符串的编码和解码，下面完成对文件的压缩和解压，具体要求：给你一个图片文件，要求对其进行无损压缩，看看压缩的效果如何。

**思路**：读取文件=====>得到赫夫曼编码表========>完成压缩

```
//编写一个方法，将文件进行压缩

    /**
     *
     * @param srcFile 你传入的希望压缩的文件的全部路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
    public static void zipFile(String srcFile,String dstFile){

        //创建输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        //创建输入流
        //创建文件的输入流
        FileInputStream is = null;
        try {
            //创建文件的驶入流
            is = new FileInputStream(srcFile);
            //创建一个和源文件大小一样的byte[]
            byte[] b = new byte[is.available()];
            //读取文件
            is.read();
            //直接对源文件压缩
            byte[] huffmanBytes = huffmanZip(b);
            //创建文件的输出流，存放压缩文件
            os = new FileOutputStream(dstFile);
            //创建一个和文件输出流关联的ObjectOutputStream
            oos = new ObjectOutputStream(os);
            //把赫夫曼编码后的字节数组写入压缩文件
            oos.writeObject(huffmanBytes);
            //这里我们以对象流的方式写入赫夫曼编码，是为了以后我们恢复源文件时使用
            //注意一定要把赫夫曼写入压缩文件
            oos.writeObject(huffmanCodes);

        }catch (Exception e){
            System.out.println(e.getMessage());
        }finally {
            try {
                is.close();
                os.close();
                oos.close();
            }catch (Exception e){
                System.out.println(e.getMessage());
            }
        }
    }
```

### 9.3.7 文件解压

```java
//编写一个方法，完成对压缩文件的压缩

    /**
     *
     * @param zipFile 准备解压的文件
     * @param dstFile 将文件解压到哪个路径
     */

    public static void unZipFile(String zipFile,String dstFile){
        //定义文件输入流
        InputStream is = null;
        //定义一个对象输入流
        ObjectInputStream ois = null;
        //定义文件的输出流
        OutputStream os = null;
        try {
            //创建文件输入流
            is = new FileInputStream(zipFile);
            //创建一个和is关联的对象输入流
            ois = new ObjectInputStream(is);
            //读取byte数组 huffmanBytes
            byte[] huffmanBytes = (byte[])ois.readObject();
            //读取赫夫曼编码表
            Map<Byte,String> huffmanCodes = (Map<Byte,String>)ois.readObject();

            //解码
            byte[] bytes = decode(huffmanCodes,huffmanBytes);
            //将bytes 数组写入到目标文件
            os = new FileOutputStream(dstFile);
            //写数据到dstFile文件
            os.write(bytes);
        }catch (Exception e){
            System.out.println(e.getMessage());
        } finally {
            try {
                os.close();
                ois.close();
                is.close();
            } catch (Exception e2){
                System.out.println(e2.getMessage());
            }

        }
    }
```

### 9.3.8 赫夫曼压缩文件注意事项

1. 如果文件本身就是经过压缩处理的，那么使用赫夫曼编码再压缩效率不会有明显变化，比如视频，ppt等文件。
2. 赫夫曼编码是按字节来处理的，因此可以处理所有的文件
3. 如果一个文件中的内容，重复的数据不多，压缩效果也不会很明显

## 9.4 二叉排序树

### 9.4.1 二叉排序树的基本介绍

**先看一个需求**

给一个数列{7,3,10,12,5,1,9}，要求能够高效的完成对数据的查询喝添加

**解决方案**

1. 使用数组：1）数组未排序，优点是直接在数组尾添加，速度快。缺点是：查找速度慢。2）数组排序，优点：可以使用二分查找，查找速度快，缺点：为了保证数组有序，在添加新数据时，找到插入位置后，后面的数据需要整体移动，速度慢。
2. 使用链式存储----链表：不管链表是否有序，查找速度都很慢，添加数据舒服比数组快，不需要数据整体移动。
3. 使用二叉排序树

**二叉排序树介绍**

二叉排序树：BST(Binary Sort Tree)，对于二叉排序树的任何一个非叶子节点，要求左子节点的值比当前节点的值小，右子节点的值比当前节点大。**特别说明**：如果有相同的值，可以将该节点放在左子节点或右子节点。

![image-20210407120452685](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210407120452685.png)

### 9.4.2 二叉排序树的创建和遍历

一个数组创建成对用的二叉排序树，并使用中序遍历二叉排序树。

```java
public class BinarySortTreeDemo {
    public static void main(String[] args) {
        int[] arr = {7,3,10,12,5,1,9};
        BinarySortTree binarySortTree = new BinarySortTree();
        //循环添加节点
        for (int i = 0; i < arr.length; i++){
            binarySortTree.add(new Node(arr[i]));
        }

        //中序遍历二叉排序树
        System.out.println("看小爷的中序排序操作输出~~~~~");
        binarySortTree.infixOrder();
    }
}

//创建二叉排序树
class BinarySortTree{
    private Node root;
    //添加节点的方法
    public void add(Node node){
        if (root == null){
            root = node;  //如果root为空则直接让root指向node
        } else {
            root.add(node);
        }
    }
    //中序遍历
    public void infixOrder(){
        if (root!=null){
            root.infixOrder();
        } else {
            System.out.println("这他妈是空的，玩尼玛呢");
        }
    }
}


//创建Node节点
class Node{
    int value;
    Node left;
    Node right;
    public Node(int value){
        this.value = value;
    }

    @Override
    public String toString(){
        return "Node [value="+value+"]";
    }

    //添加节点的方法，使用递归的方式进行添加，注意需要满足二叉排序树的要求
    public void add(Node node){
        if (node == null){
            return;
        }
        //判断传入的节点值和当前子树的根节点值的关系
        if (node.value < this.value){
            //如果当前节点左子节点为null
            if (this.left == null){
                this.left = node;
            } else {
                //递归向左子树添加
                this.left.add(node);
            }
        } else {
            //添加的节点的值大于当前节点的值
            if (this.right == null){
                this.right = node;
            } else {
                //递归的向右子树添加
                this.right.add(node);
            }
        }
    }

    //中序遍历
    public void infixOrder(){
        if (this.left != null){
            this.left.infixOrder();
        }
        System.out.println(this);
        if (this.right != null){
            this.right.infixOrder();
        }
    }

}
```

**运行结果**

```
看小爷的中序排序操作输出~~~~~
Node [value=1]
Node [value=3]
Node [value=5]
Node [value=7]
Node [value=9]
Node [value=10]
Node [value=12]
```

### 9.4.3 二叉排序树的删除

![image-20210407123929807](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210407123929807.png)

二叉排序树的删除情况比较复杂，下面有三种情况需要考虑

1. 删除叶子节点（比如：2，5，9，12）
2. 删除只有一棵子树的节点（比如：1）
3. 删除有两棵子树的节点（比如：7，3，10）

**第一种情况**

1. 需要先找到要删除的节点targetNode
2. 找到targetNode的父节点parent
3. 确定targetNode是parent的左子节点还是右子节点
4. 根据前面的情况来对应删除

左子节点：parent.left = null;

右子节点：parent.right = null;

**第二种情况**

1. 需要先找到要删除的节点targetNode

2. 找到targetNode的父节点parent

3. 确定targetNode 的子节点是左子节点还是右子节点

4. targetNode 是 parent的左子节点还是右子节点

5. 如果targetNode有左子节点

   5.1 如果targetNode是parent的左子节点

   parent.left = targetNode.left;

   5.2 如果targetNode是parent的右子节点

   parent.right = targetNode.left;

6. 如果targetNode有右子节点

   6.1 如果targetNode是parent的左子节点

   parent.left = targetNode.right;

   6.2 如果targetNode是parent的右子节点

   parent.right = targetNode.right;

**第三种情况**

1. 需要先找到要删除的节点targetNode
2. 找到targetNode的父节点parent
3. 从targetNode的左子树找到最小的节点
4. 用一个临时变量，将最小节点的值保存temp
5. 删除最小的节点
6. targetNode.value = temp;

### 9.4.4 代码实现

**删除叶子节点**

```java
public class BinarySortTreeDemo {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9, 2};
        BinarySortTree binarySortTree = new BinarySortTree();
        //循环添加节点
        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(new Node(arr[i]));
        }

        //中序遍历二叉排序树
        System.out.println("看小爷的中序排序操作输出~~~~~");
        binarySortTree.infixOrder();

        //测试一下删除叶子节点
        binarySortTree.delNode(2);
        System.out.println("删除节点后");
        binarySortTree.infixOrder();
    }
}

//创建二叉排序树
class BinarySortTree {
    private Node root;

    //查找要删除的节点
    public Node search(int value) {
        if (root == null) {
            return null;
        } else {
            return root.search(value);
        }
    }

    //查找父节点
    public Node searchParent(int value) {
        if (root == null) {
            return null;
        } else {
            return root.searchParent(value);
        }
    }

    //删除节点
    public void delNode(int value) {
        if (root == null) {
            return;
        } else {
            //1. 需要先去找要删除的节点targetNode
            Node targetNode = search(value);
            //如果没有找到要删除的节点
            if (targetNode == null) {
                return;
            }
            //如果我们发现这个二叉树只有一个节点
            if (root.left == null && root.right == null) {
                root = null;
                return;
            }

            //找到targetNode的父节点
            Node parent = searchParent(value);
            //如果要删除的节点是叶子节点
            if (targetNode.left == null && targetNode.right == null) {
                //判断targetNode 是父节点的左子节点还是右子节点
                if (parent.left != null && parent.left.value == value) {
                    parent.left = null;
                } else if (parent.right != null && parent.right.value == value) {
                    parent.right = null;
                }
            }
        }
    }

    //添加节点的方法
    public void add(Node node) {
        if (root == null) {
            root = node;  //如果root为空则直接让root指向node
        } else {
            root.add(node);
        }
    }

    //中序遍历
    public void infixOrder() {
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("这他妈是空的，玩尼玛呢");
        }
    }
}


//创建Node节点
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    //查找要删除的节点

    /**
     * @param value 希望删除的节点的值
     * @return 如果找到返回该节点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {
            //找到就是该节点
            return this;
        } else if (value < this.value) {
            //如果查找的值小于该节点，向左子树递归查找
            //如果左子节点为空
            if (this.left == null) {
                return null;
            }
            return this.left.search(value);
        } else {
            if (this.right == null) {
                return null;
            }
            return this.right.search(value);
        }
    }

    //找到要删除节点的父节点

    /**
     * @param value 要找到的节点的值
     * @return 返回的是要删除的节点的父节点，如果没有就返回null
     */
    public Node searchParent(int value) {
        //如果当前节点就是要珊瑚的节点的父节点就返回
        if ((this.left != null && this.left.value == value) || (this.right != null && this.right.value == value)) {
            return this;
        } else {
            //如果查找的值小于当前节点的值，并且当前节点的左子节点不为空
            if (value < this.value && this.left != null) {
                return this.left.searchParent(value);  //向左子树递归查找
            } else if (value >= this.value && this.right != null) {
                return this.right.searchParent(value);  //向右子树递归查找
            } else {
                return null;  //说明没有找到父节点
            }
        }
    }

    @Override
    public String toString() {
        return "Node [value=" + value + "]";
    }

    //添加节点的方法，使用递归的方式进行添加，注意需要满足二叉排序树的要求
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的节点值和当前子树的根节点值的关系
        if (node.value < this.value) {
            //如果当前节点左子节点为null
            if (this.left == null) {
                this.left = node;
            } else {
                //递归向左子树添加
                this.left.add(node);
            }
        } else {
            //添加的节点的值大于当前节点的值
            if (this.right == null) {
                this.right = node;
            } else {
                //递归的向右子树添加
                this.right.add(node);
            }
        }
    }

    //中序遍历
    public void infixOrder() {
        if (this.left != null) {
            this.left.infixOrder();
        }
        System.out.println(this);
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

}
```

**运行结果**

```
看小爷的中序排序操作输出~~~~~
Node [value=1]
Node [value=2]
Node [value=3]
Node [value=5]
Node [value=7]
Node [value=9]
Node [value=10]
Node [value=12]
删除节点后
Node [value=1]
Node [value=3]
Node [value=5]
Node [value=7]
Node [value=9]
Node [value=10]
Node [value=12]

Process finished with exit code 0

```

**删除只有一个子树的节点**

```java
//删除只有一个子树的节点
                //如果要删除的节点有左子节点
                if (targetNode.left != null){
                    //如果targetNode 是parent的左子节点
                    if (parent.left.value == value){
                        parent.left = targetNode.left;
                    } else {
                        //targetNode 是parent的右子节点
                        parent.right = targetNode.left;
                    }
                } else {
                    //如果要删除的节点是右子节点
                    if (parent.left.value == value){
                        parent.left = targetNode.right;
                    } else {
                        //如果targetNode是parent的右子节点
                        parent.right = targetNode.right;
                    }
                }
```

**结果**

```
看小爷的中序排序操作输出~~~~~
Node [value=1]
Node [value=2]
Node [value=3]
Node [value=5]
Node [value=7]
Node [value=9]
Node [value=10]
Node [value=12]
删除节点后
Node [value=2]
Node [value=3]
Node [value=5]
Node [value=7]
Node [value=9]
Node [value=10]
Node [value=12]
```

**删除有两个子节点的代码**

```java
else if (targetNode.left != null && targetNode.right != null){
                //删除有两个子树的节点
                int minVal = delRightTreeMin(targetNode.right);
                targetNode.value = minVal;
```

**运行结果**

```
看小爷的中序排序操作输出~~~~~
Node [value=1]
Node [value=2]
Node [value=3]
Node [value=5]
Node [value=7]
Node [value=9]
Node [value=10]
Node [value=12]
删除节点后
Node [value=1]
Node [value=2]
Node [value=3]
Node [value=5]
Node [value=9]
Node [value=10]
Node [value=12]
```

### 9.4.5 二叉排序树代码中的一个漏洞

**当你删除的还剩下最后两个节点的时候，此时进行删除的时候就进入删除只有一个节点的情况，但是parent会出现空指针异常的情况**

所以我们需要修改以下代码，来防止异常的出现

**史上最完整的代码**

```java
package binarySortTree;

public class BinarySortTreeDemo {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9, 2};
        BinarySortTree binarySortTree = new BinarySortTree();
        //循环添加节点
        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(new Node(arr[i]));
        }

        //中序遍历二叉排序树
        System.out.println("看小爷的中序排序操作输出~~~~~");
        binarySortTree.infixOrder();

        //测试一下删除叶子节点
        binarySortTree.delNode(7);
        System.out.println("删除节点后");
        binarySortTree.infixOrder();
    }
}

//创建二叉排序树
class BinarySortTree {
    private Node root;

    //查找要删除的节点
    public Node search(int value) {
        if (root == null) {
            return null;
        } else {
            return root.search(value);
        }
    }

    //查找父节点
    public Node searchParent(int value) {
        if (root == null) {
            return null;
        } else {
            return root.searchParent(value);
        }
    }
    //编写方法：
    //1. 返回的以node为根节点的二叉排序树的最小节点的值
    //2. 删除node为根节点的二叉排序树的最小节点

    /**
     * @param node 传入的节点（当作二叉排序树的根节点）
     * @return 返回的以Node为根节点的二叉排序树的最小节点的值
     */
    public int delRightTreeMin(Node node) {
        Node target = node;
        //循环的查找左子节点，就会找到最小值
        while (target.left != null) {
            target = target.left;
        }
        //这时target就指向了最小节点
        //删除最小节点
        delNode(target.value);
        return target.value;
    }

    //删除节点
    public void delNode(int value) {
        if (root == null) {
            return;
        } else {
            //1. 需要先去找要删除的节点targetNode
            Node targetNode = search(value);
            //如果没有找到要删除的节点
            if (targetNode == null) {
                return;
            }
            //如果我们发现这个二叉树只有一个节点
            if (root.left == null && root.right == null) {
                root = null;
                return;
            }

            //找到targetNode的父节点
            Node parent = searchParent(value);
            //如果要删除的节点是叶子节点
            if (targetNode.left == null && targetNode.right == null) {
                //判断targetNode 是父节点的左子节点还是右子节点
                if (parent.left != null && parent.left.value == value) {
                    parent.left = null;
                } else if (parent.right != null && parent.right.value == value) {
                    parent.right = null;
                }
            } else if (targetNode.left != null && targetNode.right != null) {
                //删除有两个子树的节点
                int minVal = delRightTreeMin(targetNode.right);
                targetNode.value = minVal;

            } else {
                //删除只有一个子树的节点
                //如果要删除的节点有左子节点
                if (targetNode.left != null) {
                    //如果targetNode 是parent的左子节点
                    if (parent != null) {
                        if (parent.left.value == value) {
                            parent.left = targetNode.left;
                        } else {
                            //targetNode 是parent的右子节点
                            parent.right = targetNode.left;
                        }
                    } else {
                        root = targetNode.left;
                    }
                } else {
                    //如果要删除的节点是右子节点
                    if (parent != null) {
                        if (parent.left.value == value) {
                            parent.left = targetNode.right;
                        } else {
                            //如果targetNode是parent的右子节点
                            parent.right = targetNode.right;
                        }
                    } else {
                        root = targetNode.right;
                    }
                }
            }
        }
    }

    //添加节点的方法
    public void add(Node node) {
        if (root == null) {
            root = node;  //如果root为空则直接让root指向node
        } else {
            root.add(node);
        }
    }

    //中序遍历
    public void infixOrder() {
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("这他妈是空的，玩尼玛呢");
        }
    }
}


//创建Node节点
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    //查找要删除的节点

    /**
     * @param value 希望删除的节点的值
     * @return 如果找到返回该节点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {
            //找到就是该节点
            return this;
        } else if (value < this.value) {
            //如果查找的值小于该节点，向左子树递归查找
            //如果左子节点为空
            if (this.left == null) {
                return null;
            }
            return this.left.search(value);
        } else {
            if (this.right == null) {
                return null;
            }
            return this.right.search(value);
        }
    }

    //找到要删除节点的父节点

    /**
     * @param value 要找到的节点的值
     * @return 返回的是要删除的节点的父节点，如果没有就返回null
     */
    public Node searchParent(int value) {
        //如果当前节点就是要珊瑚的节点的父节点就返回
        if ((this.left != null && this.left.value == value) || (this.right != null && this.right.value == value)) {
            return this;
        } else {
            //如果查找的值小于当前节点的值，并且当前节点的左子节点不为空
            if (value < this.value && this.left != null) {
                return this.left.searchParent(value);  //向左子树递归查找
            } else if (value >= this.value && this.right != null) {
                return this.right.searchParent(value);  //向右子树递归查找
            } else {
                return null;  //说明没有找到父节点
            }
        }
    }

    @Override
    public String toString() {
        return "Node [value=" + value + "]";
    }

    //添加节点的方法，使用递归的方式进行添加，注意需要满足二叉排序树的要求
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的节点值和当前子树的根节点值的关系
        if (node.value < this.value) {
            //如果当前节点左子节点为null
            if (this.left == null) {
                this.left = node;
            } else {
                //递归向左子树添加
                this.left.add(node);
            }
        } else {
            //添加的节点的值大于当前节点的值
            if (this.right == null) {
                this.right = node;
            } else {
                //递归的向右子树添加
                this.right.add(node);
            }
        }
    }

    //中序遍历
    public void infixOrder() {
        if (this.left != null) {
            this.left.infixOrder();
        }
        System.out.println(this);
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

}
```

## 9.5 平衡二叉树

**给一个数列{1，2，3，4，5，6}，要求创建一棵二叉排序树(BST)，可能出现的问题是**

![image-20210407182253446](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210407182253446.png)

**左边BST存在的问题**

1. 左边子树全为空，从形式上看，更像一个单链表
2. 插入速度没有问题
3. 查询速度明显降低（因为需要一次比较），不能发挥BST的优势，因为每次还需要比较左子树，其查询速度比单链表还慢
4. 解决的方案是**平衡二叉树**

### 9.5.1 平衡二叉树的基本原理介绍

**基本介绍**

1. 平衡二叉树也叫平衡二叉搜索树（self-balancing binary search tree）又称为AVL树，可以保证**查询效率较高**
2. 具有以下特点：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树，平衡二叉树的常用实现方法有：红黑树，AVL，替罪羊树，Treap，伸展树等。

### 9.5.2 左旋转分析

![image-20210407183734876](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210407183734876.png)

**代码实现**

```java
package AVL;

public class AVLTreeDemo {
    public static void main(String[] args) {
        int[] arr ={4,3,6,5,7,8};
        //创建一个AVLTree对象
        AVLTree avlTree = new AVLTree();
        //添加节点
        for (int i = 0; i < arr.length; i++){
            avlTree.add(new Node(arr[i]));
        }
        //遍历
        System.out.println("中序遍历");
        avlTree.infixOrder();

        System.out.println("在没有平衡处理之前~~~");
        System.out.println("树的高度="+avlTree.getRoot().height());
        System.out.println("树的左子树高度="+avlTree.getRoot().leftHeight());
        System.out.println("树的右子树高度="+avlTree.getRoot().rightHeight());
    }
}

//创建AVLTree
class AVLTree {
    private Node root;

    public Node getRoot(){
        return root;
    }

    //查找要删除的节点
    public Node search(int value) {
        if (root == null) {
            return null;
        } else {
            return root.search(value);
        }
    }

    //查找父节点
    public Node searchParent(int value) {
        if (root == null) {
            return null;
        } else {
            return root.searchParent(value);
        }
    }
    //编写方法：
    //1. 返回的以node为根节点的二叉排序树的最小节点的值
    //2. 删除node为根节点的二叉排序树的最小节点

    /**
     * @param node 传入的节点（当作二叉排序树的根节点）
     * @return 返回的以Node为根节点的二叉排序树的最小节点的值
     */
    public int delRightTreeMin(Node node) {
        Node target = node;
        //循环的查找左子节点，就会找到最小值
        while (target.left != null) {
            target = target.left;
        }
        //这时target就指向了最小节点
        //删除最小节点
        delNode(target.value);
        return target.value;
    }

    //删除节点
    public void delNode(int value) {
        if (root == null) {
            return;
        } else {
            //1. 需要先去找要删除的节点targetNode
            Node targetNode = search(value);
            //如果没有找到要删除的节点
            if (targetNode == null) {
                return;
            }
            //如果我们发现这个二叉树只有一个节点
            if (root.left == null && root.right == null) {
                root = null;
                return;
            }

            //找到targetNode的父节点
            Node parent = searchParent(value);
            //如果要删除的节点是叶子节点
            if (targetNode.left == null && targetNode.right == null) {
                //判断targetNode 是父节点的左子节点还是右子节点
                if (parent.left != null && parent.left.value == value) {
                    parent.left = null;
                } else if (parent.right != null && parent.right.value == value) {
                    parent.right = null;
                }
            } else if (targetNode.left != null && targetNode.right != null) {
                //删除有两个子树的节点
                int minVal = delRightTreeMin(targetNode.right);
                targetNode.value = minVal;

            } else {
                //删除只有一个子树的节点
                //如果要删除的节点有左子节点
                if (targetNode.left != null) {
                    //如果targetNode 是parent的左子节点
                    if (parent != null) {
                        if (parent.left.value == value) {
                            parent.left = targetNode.left;
                        } else {
                            //targetNode 是parent的右子节点
                            parent.right = targetNode.left;
                        }
                    } else {
                        root = targetNode.left;
                    }
                } else {
                    //如果要删除的节点是右子节点
                    if (parent != null) {
                        if (parent.left.value == value) {
                            parent.left = targetNode.right;
                        } else {
                            //如果targetNode是parent的右子节点
                            parent.right = targetNode.right;
                        }
                    } else {
                        root = targetNode.right;
                    }
                }
            }
        }
    }

    //添加节点的方法
    public void add(Node node) {
        if (root == null) {
            root = node;  //如果root为空则直接让root指向node
        } else {
            root.add(node);
        }
    }

    //中序遍历
    public void infixOrder() {
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("这他妈是空的，玩尼玛呢");
        }
    }
}

//创建Node节点
class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    //返回左子树的高度
    public int leftHeight() {
        if (left == null) {
            return 0;
        }
        return left.height();
    }

    //返回右子树的高度
    public int rightHeight() {
        if (right == null) {
            return 0;
        }
        return right.height();
    }

    //返回当前节点的高度，以该节点为根节点的树的高度
    public int height() {
        return Math.max(left == null ? 0 : left.height(), right == null ? 0 : right.height()) + 1;
    }

    //左旋转方法
    private void leftRotate(){
        //创建新的节点，以当前根节点的值
        Node newNode = new Node(value);
        //把新的节点的左子树设置成当前节点的左子树
        newNode.left = left;
        //把新的节点的右子树设置成当前节点的右子树的左子树
        newNode.right = right.left;
        //把当前节点的值替换成右子节点的值
        value = right.value;
        //把当前节点的右子树设置成当前节点右子树的右子树
        right = right.right;
        //把当前节点的左子树（左子节点）设置成新的节点
        left = newNode;
    }

    //查找要删除的节点

    /**
     * @param value 希望删除的节点的值
     * @return 如果找到返回该节点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {
            //找到就是该节点
            return this;
        } else if (value < this.value) {
            //如果查找的值小于该节点，向左子树递归查找
            //如果左子节点为空
            if (this.left == null) {
                return null;
            }
            return this.left.search(value);
        } else {
            if (this.right == null) {
                return null;
            }
            return this.right.search(value);
        }
    }

    //找到要删除节点的父节点

    /**
     * @param value 要找到的节点的值
     * @return 返回的是要删除的节点的父节点，如果没有就返回null
     */
    public Node searchParent(int value) {
        //如果当前节点就是要珊瑚的节点的父节点就返回
        if ((this.left != null && this.left.value == value) || (this.right != null && this.right.value == value)) {
            return this;
        } else {
            //如果查找的值小于当前节点的值，并且当前节点的左子节点不为空
            if (value < this.value && this.left != null) {
                return this.left.searchParent(value);  //向左子树递归查找
            } else if (value >= this.value && this.right != null) {
                return this.right.searchParent(value);  //向右子树递归查找
            } else {
                return null;  //说明没有找到父节点
            }
        }
    }

    @Override
    public String toString() {
        return "Node [value=" + value + "]";
    }

    //添加节点的方法，使用递归的方式进行添加，注意需要满足二叉排序树的要求
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的节点值和当前子树的根节点值的关系
        if (node.value < this.value) {
            //如果当前节点左子节点为null
            if (this.left == null) {
                this.left = node;
            } else {
                //递归向左子树添加
                this.left.add(node);
            }
        } else {
            //添加的节点的值大于当前节点的值
            if (this.right == null) {
                this.right = node;
            } else {
                //递归的向右子树添加
                this.right.add(node);
            }
        }
        //当添加完一个节点之后，如果：右子树的高度减去左子树的高度大于1，则进行左旋转
        if (rightHeight() - leftHeight() > 1){
            leftRotate();
        }
    }

    //中序遍历
    public void infixOrder() {
        if (this.left != null) {
            this.left.infixOrder();
        }
        System.out.println(this);
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

}
```

**结果**

```
中序遍历
Node [value=3]
Node [value=4]
Node [value=5]
Node [value=6]
Node [value=7]
Node [value=8]
在没有平衡处理之前~~~
树的高度=3
树的左子树高度=2
树的右子树高度=2
```

### 9.5.3 右旋转分析

![image-20210407201038972](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210407201038972.png)

**代码**

```java
//右旋转
    private void rightRotate(){
        Node newNode = new Node(value);
        newNode.right = right;
        newNode.left = left.right;
        value = left.value;
        left = left.left;
        right = newNode;
    }
```

### 9.5.4 双旋转分析

**前面的两个数列，进行单旋转（即一次旋转）就可以将非平衡二叉树转成平衡二叉树，但是在某种情况下，但旋转不能完成平衡二叉树的转换**

比如数组 int[] arr = {10,11,7,6,8,9}，或者数组int[] arr = {2,1,6,5,7,3}

![image-20210407205408834](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210407205408834.png)

**问题分析**

1. 当符号右旋转调整时
2. 如果它的左子树中（右子树的高度大于左子树高度

![image-20210408003313785](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408003313785.png)

3. 然后对其进行左旋转

![image-20210408003639337](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408003639337.png)

4. 然后再对整个进行右旋转

![image-20210408004006877](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408004006877.png)

**代码实现**

```java
//当添加完一个节点之后，如果：右子树的高度减去左子树的高度大于1，则进行左旋转
        if (rightHeight() - leftHeight() > 1){
            if (right != null && right.leftHeight() > right.rightHeight()){
                right.rightRotate();
                leftRotate();
            } else {
                leftRotate();
            }
            return;  //必须需要！！！
        }
        //当添加完一个节点之后，如果：左子树的高度减去右子树的高度大于1，则进行右旋转
        if (leftHeight() - rightHeight() > 1){
            //如果它的左子树的右子树高度大于它的左子树高度
            if (left != null && left.rightHeight() > left.leftHeight()){
                //先对当前节点的左节点====>左旋转
                left.leftRotate();
                //再对当前节点进行右旋转
                rightRotate();
            } else {
                rightRotate();
            }
        }
    }
```

**结果实现**

```
中序遍历
Node [value=6]
Node [value=7]
Node [value=8]
Node [value=9]
Node [value=10]
Node [value=12]
在没有平衡处理之前~~~
树的高度=3
树的左子树高度=2
树的右子树高度=2
```

# 10 多路查找树

## 10.1 基础知识介绍

**二叉树的问题分析**

![image-20210408091745322](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408091745322.png)

- 二叉树需要加载到内存中，如果二叉树的节点少，没有什么问题，但是如果二叉树的节点很多（比如一亿个左右），就会存在以下问题

- 问题一：在构建二叉树时，需要多此进行io操作（海量数据存在数据库或文件中），节点海量，构建二叉树时速度有影响。

- 问题二：节点海量，也会造成树的高度很大，会降低操作速度。

**多叉树**

1. 在二叉树中，每个节点有数据项，最多有两个子节点。如果允许每个节点可以有更多的数据项和更多的子节点，就是多叉树(multiway tree)
2. 后面我们所讲解的2-3树，2-3-4树就是多叉树，多叉树通过重新组织节点，减少树的高度，能对二叉树进行优化

![image-20210408092723801](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408092723801.png)

**B树的基本介绍**

![image-20210408093049126](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408093049126.png)

B树通过重新组织节点，降低树的高度，并且减少io读取次数来提升效率

1. 如图B树通过重新组织节点，降低了树的高度
2. 文件系统及数据库系统的设计者利用了磁盘预读原理，将一个节点的大小设为等于一个页（页的大小通常为4k），这样每个节点只需要一次io就可以完全载入
3. 将树的度M设置为1024，在600亿个元素中最多只需要4次io操作就可以读取到想要的元素，B（b+）树广泛应用于文件存储系统以及数据库系统中。

## 10.2 2-3树案例讲解

**2-3树基本介绍**：2-3树是最简单的B树结构，具有以下特点

1. 2-3树的所有叶子节点都在同一层，（只要是B树都满足这个条件）
2. 有两个子节点的节点叫二节点，二节点要么没有子节点，要么有两个子节点
3. 有三个节点的节点叫三节点，三节点要么没有节点，要么有三个节点
4. 2-3树是由二节点和三节点构成的树。

将数列{16,24,12,32,14,26,34,10,8,28,38,20}构建成2-3树，并保证数据插入的大小顺序

**插入规则**

1. 2-3树的所有叶子节点都是在同一层（只要是B树都要满足这个条件）
2. 有两个子节点的节点叫二节点，二节点要么没有节点，要么由两个节点。
3. 有三个节点的节点叫三节点，三节点要么没有节点，要么有三个节点
4. 当按照规则插入一个数到某个节点时，不能满足上面三个要求，就需要拆，先向上拆，如果上层满，则拆本层，拆后仍然需要满足上面三个条件，
5. 对于三节点的子树的值的大小仍然遵循（BST二叉排序树）的规则。

插入完成后的结果

![image-20210408095729152](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408095729152.png)

## 10.3 B树

**B树的介绍**

B-tree就是B树，B是Balanced，平衡的意思。有人把B-tree翻译成B-树，容易让人产生误解，会认为B-树是一种树，而B树是另外一种树。实际上B-tree就是指的是B树。

**B树的介绍**

前面已经介绍了2-3树以及2-3-4树，它们就是B树，这里我们再做一个说明，我们再学习MySQL时，经常可以听到说某种类型的索引就是基于B树或者B+树的。

1. B树的阶：节点的最多子节点个数。比如2-3树的阶就是3，2-3-4树的阶就是4.
2. B树的搜索，从根节点开始，对节点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查询关键字所属范围的子节点，重复，直到所对应的子指针为空，或已经时叶子节点
3. 关键字集合分布再整棵树中，即叶子节点和非叶子节点都存放数据
4. 搜索有可能再非叶子节点结束
5. 其搜索性能等价于再关键字全集内做一次二叉查找。

![image-20210408102145585](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408102145585.png)

**B+树的介绍**

B+树时B树的变体，也是一种多路搜索树。

1. B+树的搜索与B树也基本相同，区别是B+树只有达到叶子节点才能命中（B树可以再非叶子节点命中），其性能也等价于再关键字全集做一次二分查找。
2. 所有关键字都出现在叶子节点的链表中（即数据只能在叶子节点【也叫稠密索引】），且链表中的关键字（数据）恰好是有序的。
3. 不可能在非叶子节点命中
4. 非叶子节点相当于是叶子节点的索引（稀疏索引）叶子节点相当于是存储（关键字）数据的数据层
5. 更适合文件索引系统
6. B树和B+树各有自己的应用场景，不能说B+树完全比B树好，反之亦然。

![image-20210408103238688](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408103238688.png)

**B *树的介绍**

B * 树是B+树的非根和非叶子节点再增加指向兄的指针

1. B * 树定义了非叶子节点关键字个数至少为(2/3)*M，即最块的最低使用率为2/3，而B+树的块的最低使用率为B+树的1/2。
2. 从第一个特点我们就可以看出，B*树分配新节点的概率比B+树要低，空间利用率更高。

# 11 图

**为什么要有图**

1. 前面学习的线性表和树
2. 线性表局限于一个直接前驱和一个直接后继的关系
3. 树也只能有一个直接前驱也就是父节点
4. 当我们需要表示多对多的关系时，这里就需要用到图

## 11.1 图的基本知识

**图的常用概念**

![image-20210408124420527](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408124420527.png)

![image-20210408124543996](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408124543996.png)

**图的表示方式**

图的表示方式有两种：二维数组表示（邻接矩阵）：链表表示（邻接表）

![image-20210408130408829](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408130408829.png)

邻接表

1. 邻接矩阵需要为每个顶点都分配n个边的空间，其实有很多边都是不存在的，会造成空间的一定损失
2. 邻接表的实现只关心存在的边，不关心不存在的边，因此没有空间浪费，邻接表数组+链表组成。

![image-20210408130650143](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408130650143.png)

**说明**

1. 标号为0的节点的相关联的节点为1 2 3 4 
2. 标号为1的节点的相关联系节点为0 4
3. 标号为2的节点的相关联节点为0 4 5

## 11.2 图的快速入门案例

**要求**

![image-20210408144607869](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210408144607869.png)

**图的创建和分析代码**

```java
public class Graph {

    private ArrayList<String> vertexList;   //存储顶点集合
    private int[][] edges;   //存储图对应的邻结矩阵
    private int numOfEdges;  //表示边的数目

    public static void main(String[] args) {
        //测试图创建的是否OK
        int n = 5;  //节点的个数
        String Vertexs[] = {"A","B","C","D","E"};
        //创建图像
        Graph graph = new Graph(5);
        //循环的添加顶点
        for (String vertex:Vertexs){
            graph.insertVertex(vertex);
        }
        //添加边
        graph.insertEdge(0,1,1);
        graph.insertEdge(0,2,1);
        graph.insertEdge(1,2,1);
        graph.insertEdge(1,3,1);
        graph.insertEdge(1,4,1);

        //显示邻接矩阵
        graph.showGraph();


    }

    //构造器
    public Graph(int n){
        //初始化矩阵vertexList
        edges = new int[n][n];
        vertexList = new ArrayList<String>(n);
        numOfEdges = 0;
    }
    //图中常用的方法
    //返回节点个数
    public int getNumOfVertex(){
        return vertexList.size();
    }
    //显示对应的矩阵
    public void showGraph(){
        for (int[] link:edges){
            System.out.println(Arrays.toString(link));
        }
    }
    //得到边的数目
    public int getNumOfEdges(){
        return numOfEdges;
    }
    //返回节点i(下标)对应的数据
    public String getValueByIndex(int i){
        return vertexList.get(i);
    }
    //返回v1和v2的权值
    public int getWeight(int v1, int v2){
        return edges[v1][v2];
    }
    //插入节点
    public void insertVertex(String vertex){
        vertexList.add(vertex);
    }
    //添加边

    /**
     *
     * @param v1 表示点下标即是第几个顶点
     * @param v2 表示第二个顶点对应的下标
     * @param weight
     */
    public void insertEdge(int v1, int v2, int weight){
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }
}
```

**结果**

```
[0, 1, 1, 0, 0]
[1, 0, 1, 1, 1]
[1, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
[0, 1, 0, 0, 0]
```

## 11.3 图的遍历

**图的遍历的介绍**

所谓图的遍历，即是对节点的访问。一个图有那么多个节点，如何遍历这些节点，需要特定的策略，一般有两种访问策略：（1）**深度优先遍历**（2）**广度优先遍历**

### 11.3.1 深度优先遍历

**深度优先遍历的基本思想**(Deepth First Search)

1. 深度优先遍历，从初始访问节点出发，初始访问节点可能有多个邻接节点，深度优先遍历的策略就是首先访问第一个邻接节点，然后再以这个被访问的邻接节点作为初始节点，访问它的第一个邻接节点，可以这样理解：每次再访问完当前节点后首先访问的是第一个邻接节点。
2. 我们可以看出，这样的访问策略是优先往纵向挖掘深入的，而不是对一个节点的所有邻接节点进行横向访问。
3. 显然，深度优先搜索是一个递归的过程。

**深度优先遍历算法步骤**

1. 访问初始节点v，并标记节点v为已访问。
2. 查找节点v的第一个邻接节点w
3. 若w存在，则继续执行4，如果w不存在，则回到第一步中，将从v的下一个节点继续
4. 若w未被访问，对w进行深度优先遍历递归（即把w当作另一个v，然后进行步骤123）
5. 查找节点v的w邻接节点的下一个邻接节点

**代码**

```java
//深度优先遍历
    //i 第一次就是0
    public void dfs(boolean[] isVisited,int i){
        //首先我们访问该节点，输出
        System.out.print(getValueByIndex(i) + "==>");
        //将节点设置为已经访问
        isVisited[i] = true;
        //查找节点i的第一个邻接节点w
        int w = getFirstNeighbor(i);
        while (w != -1){
            //说明有
            if (!isVisited[w]){
                dfs(isVisited,w);
            }
            //如果w节点已经被访问过
            w = getNextNeighbor(i,w);
        }
    }

    //对dfs进行一个重载，遍历我们所有的节点，并进行dfs
    public void dfs(){
        //遍历所有的点
        for (int i = 0; i < getNumOfVertex(); i++){
            if (!isVisited[i]){
                dfs(isVisited,i);
            }
        }
    }
//得到第一个邻接点的下标w

    /**
     *
     * @param index
     * @return 如果真的存在就返回对应的下标，否则返回的是-1
     */
    public int getFirstNeighbor(int index){
        for (int j = 0; j < vertexList.size(); j++){
            if (edges[index][j] > 0){
                return j;
            }
        }
        return -1;
    }

    //根据前一个节点的下标来获取下一个邻接节点
    public int getNextNeighbor(int v1, int v2){
        for (int j = v2 + 1; j < vertexList.size(); j++){
            if (edges[v1][j] > 0){
                return j;
            }
        }
        return -1;
    }
```

**结果显示**

```
深度遍历
A==>B==>C==>D==>E==>
```

  ### 11.3.2 广度优先遍历

**广度优先遍历基本思想**（Board First Search）

类似于一个分层搜索的过程，广度优先遍历需要使用一个队列以保持访问过的节点的顺序，以便按照这个顺序来访问这些节点的邻接节点。

**广度优先访问遍历算法步骤**

1. 访问初始节点v并标记节点v为已访问

2. 节点v入队列

3. 当队列非空时，继续执行，否则算法结束

4. 出队列，取得对头节点u

5. 查找节点u的第一个邻接节点w

6. 若节点u的邻接节点w不存在，则跳转到步骤3；否则循环执行以下三个步骤

   6.1 若节点w尚未被访问，则访问节点w并标记为已访问

   6.2 节点w入队列

   6.3 查找节点u的继w邻接节点后的下一个邻接节点w，转到步骤6 

**代码实现**

```java
    //对一个节点进行广度优先遍历
    private void bfs(boolean[] isVisited, int i){
        int u;  //表示队列的头节点对应下标
        int w;  //邻接点w
        //队列，记录节点访问的顺序
        LinkedList queue = new LinkedList();
        //访问节点，输出节点信息
        System.out.print(getValueByIndex(i) + "====>");
        //标记为已访问
        isVisited[i] = true;
        //将节点加入队列
        queue.addLast(i);
        while (!queue.isEmpty()){
            //取出队列的头节点的下标
            u = (Integer)queue.removeFirst();
            //得到第一个邻接点的下标w
            w = getFirstNeighbor(u);
            while (w != -1){
                //是否被访问过
                if (!isVisited[w]){
                    System.out.print(getValueByIndex(w) + "====>");
                    //标记已经访问
                    isVisited[w] = true;
                    //入队
                    queue.addLast(w);
                }
                //以u为前去点，找到w后面的下一个邻接点
                w = getNextNeighbor(u,w); //体现出我们的广度优先
            }
        }
    }
    //遍历所有的节点，都进行广度优先搜索
    public void bfs(){
        for (int i = 0;i < getNumOfVertex(); i++){
            if (!isVisited[i]){
                bfs(isVisited,i);
            }
        }
    }
```

**运行结果**

```
广度优先
A====>B====>C====>D====>E====>
```

### 11.3.3 广度优先和深度优先的区别

**应用实例**

![image-20210412094600093](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210412094600093.png)

编写代码

```java
int n = 8;  //节点的个数
//        String Vertexs[] = {"A","B","C","D","E"};
        String Vertexs[] = {"1", "2", "3", "4", "5", "6", "7", "8"};
        //创建图像
        Graph graph = new Graph(8);
        //循环的添加顶点
        for (String vertex : Vertexs) {
            graph.insertVertex(vertex);
        }
        //添加边
//        graph.insertEdge(0, 1, 1);
//        graph.insertEdge(0, 2, 1);
//        graph.insertEdge(1, 2, 1);
//        graph.insertEdge(1, 3, 1);
//        graph.insertEdge(1, 4, 1);
        //更新边的关系
        graph.insertEdge(0,1,1);
        graph.insertEdge(0,2,1);
        graph.insertEdge(1,3,1);
        graph.insertEdge(1,4,1);
        graph.insertEdge(3,7,1);
        graph.insertEdge(4,7,1);
        graph.insertEdge(2,5,1);
        graph.insertEdge(2,6,1);
        graph.insertEdge(5,6,1);
```

运行的结果

```
[0, 1, 1, 0, 0, 0, 0, 0]
[1, 0, 0, 1, 1, 0, 0, 0]
[1, 0, 0, 0, 0, 1, 1, 0]
[0, 1, 0, 0, 0, 0, 0, 1]
[0, 1, 0, 0, 0, 0, 0, 1]
[0, 0, 1, 0, 0, 0, 1, 0]
[0, 0, 1, 0, 0, 1, 0, 0]
[0, 0, 0, 1, 1, 0, 0, 0]
深度遍历
1==>2==>4==>8==>5==>3==>6==>7==>
广度优先
1====>2====>3====>4====>5====>6====>7====>8====>
```

# 12 程序员常用的十种算法

## 12.1 二分查找算法（非递归）

### 12.1.1 算法的基本介绍

1. 前面我们讲过了二分查找算法，是使用递归的方式，下面讲解的是二分查找算法的非递归方式
2. 二分查找算法只适用于从有序的数列中进行查找（比如数字和字母等），将数列排序后再进行查找
3. 二分查找法的运行时间为对数时间O(nlogn)，即查找到需要的目标位置最多只需要logn步

### 12.1.2 二分查找算法（非递归）代码实现

数组{1,3,8,10,11,67,100}，编程实现二分查找，要求使用非递归的递归的方式完成。

```java
public class BinarySearchNoRecur {
    public static void main(String[] args) {
        int[] arr = {1,3,8,10,11,67,100};
        int index = binarySearch(arr,100);
        System.out.println("index = "+index);
    }
    //二分查找的非递归实现
    /**
     *
     * @param arr 待查找的数组，arr是升序排序
     * @param target 需要查找的数
     * @return 返回对应的下标，-1表示没有找到
     */
    public static int binarySearch(int[] arr,int target){
        int left = 0;
        int right = arr.length - 1;
        while (left <= right){
            //满足条件继续查找
            int mid = (left + right)/2;
            if (arr[mid] == target){
                return mid;
            } else if (arr[mid] > target){
                right = mid - 1; //需要向做查找
            } else {
                left = mid + 1;  //需要向右查找
            }
        }
        return -1;
    }
}
```

结果的实现

```
index = 6
```

## 12.2 分治算法

### 12.2.1 基本介绍

1. 分治算法是一种很重要的算法，字面上的解释是：分而治之，就是把一个复杂的问题分成两个或者很多的相同或者相似的子问题，再把子问题分成更小的子问题，直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。这个技巧是很多高效算法的基础，

2. 分治算法可以求解一些经典问题

   - 二分搜索

   - 大整数乘法

   - 合并问题

   - 快速排序

   - 线性时间选择

   - 最接近点对问题

   - 循环赛日程表

   - 汉诺塔

**分治算法的基本步骤**

1. 分解：将原有问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题
2. 解决：若子问题规模较小而容易被解决则直接解，否则递归的解决各个子问题
3. 合并：将各个子问题的解合并为原问题的解。

**分治算法设计模式**

```java
if |P| <= n0
    then return(ADHOC(P));
//将问题分解为较小的子问题P1,P2,P3,....,Pk
for i<--1 to k
    do yi<--Divide-and-Conquer(pi) 递归解决Pi
        T<--MERGE(y1,y2,...,yk)合并子问题
return(T)
```

其中|P|表示问题P的规模：n0为一般阈值，表示当问题P的规模不超过n0时，问题已容易直接解出，不必再继续分解，ADHOC(P)是该分治算法中的基本子算法，用于直接解小规模的问题P。因此，当P的规模不超过n0时，直接用算法ADHOC(P)求解，算法MERGER(y1,y2,...,yk)是该分治算法中的合并子算法，用于直接将P的子问题P1，P2，的相应的解y1,y2合并为P的解。

### 12.2.2 分治算法的经典案例

**汉诺塔**（Tower of Hanoi），又称**河内塔**，是一个源于[印度](https://baike.baidu.com/item/印度/121904)古老传说的[益智玩具](https://baike.baidu.com/item/益智玩具/223159)。[大梵天](https://baike.baidu.com/item/大梵天/711550)创造世界的时候做了三根金刚石柱子，在一根柱子上从下往上按照大小顺序摞着64片黄金圆盘。大梵天命令[婆罗门](https://baike.baidu.com/item/婆罗门/1796550)把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一次只能移动一个圆盘。

**思路分析**

1. 如果只有一个盘，直接A------>C

2. 如果有2个及以上的盘子，我们总是可以看做是两个盘子，1.最下边的盘子，2.上面的盘子

   2.1 先把最上面的盘A--->B

   2.2 把最下面的盘A--->C

   2.3 把B塔所有的盘从B--->C

**代码**

```java
public class Hanoitower {
    public static void main(String[] args) {
        hanoiTower(5,'A','B','C');

    }
    //汉诺塔的移动方法
    //使用分治算法

    public static void hanoiTower(int num, char a, char b, char c) {
        //如果只有一个盘
        if (num == 1) {
            System.out.println("第一个盘从 " + a + "==>" + c);
        } else {
            //如果我们有n >= 2情况，我们总是可以看做是两个盘1. 最下边的一个盘子。2. 上面的所有盘子
            //1. 先把上面的所有盘A==>B，移动过程中会使用到C
            hanoiTower(num - 1, a, c, b);
            //2. 把最下边的盘A==>C
            System.out.println("第" + num + "个盘从 " + a + "==>" + c);
            //3. 把B塔的所有盘从B==>C，移动过程中使用到A塔
            hanoiTower(num - 1, b, a, c);
        }
    }
}

```

**结果**

```
第一个盘从 A==>C
第2个盘从 A==>B
第一个盘从 C==>B
第3个盘从 A==>C
第一个盘从 B==>A
第2个盘从 B==>C
第一个盘从 A==>C
第4个盘从 A==>B
第一个盘从 C==>B
第2个盘从 C==>A
第一个盘从 B==>A
第3个盘从 C==>B
第一个盘从 A==>C
第2个盘从 A==>B
第一个盘从 C==>B
第5个盘从 A==>C
第一个盘从 B==>A
第2个盘从 B==>C
第一个盘从 A==>C
第3个盘从 B==>A
第一个盘从 C==>B
第2个盘从 C==>A
第一个盘从 B==>A
第4个盘从 B==>C
第一个盘从 A==>C
第2个盘从 A==>B
第一个盘从 C==>B
第3个盘从 A==>C
第一个盘从 B==>A
第2个盘从 B==>C
第一个盘从 A==>C
```

## 12.3 动态规划算法

### 12.3.1 动态规划算法的基本介绍

1. 动态规划(Dynamic Programming)算法的核心思想是：将大的问题划分为小问题解决，从而进一步获取最优解的处理算法
2. 动态规划算法与分治算法类似，其基本思想也是将待求解的算法分解成若干个子问题，先求解子问题，然后从这些子问题的解得到原问题的解
3. 与分治算法不同的是，适合于用动态规划求解的问题，经分解得到子问题往往是相互独立的。（即下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行下一步的求解）。
4. 动态规划可以通过填表的方式来逐步推进，得到最优解。

### 12.3.2 案例

背包问题

![image-20210412190953332](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210412190953332.png)

**思路分析**

1. 背包问题主要是指一个给定容量的背包，若干具有一定价值和重量的物品，如何选择物品放入背包使物品的价值最大，其中又分为01背包和完全背包（01背包指的是放入的东西不可以重复，完全背包指的是：每种物品都有无限件可以使用）
2. 这个问题属于01背包，即每个物品最多可以放一个，而无限背包可以转换为01背包。
3. **算法的主要思想**：利用动态规划来解决，每次遍历到第i个物品，根据w[i]和v[i]来确定是否需要将该物品放入到背包，即对于给定的n个物品，设v[i]和w[i]分别为物品的价值和重量，C为背包的容量，再令v[i] [j]表示在前i个物品中能够装入容量为j的背包中的最大价值，

**几个重要的代码讲解**

1. v[i] [0] = v[0] [j] = 0; //表示填入表的第一行和第一列都为零
2. 当w[j] > j时：v[i] [j] = v[i-1] [j]  //当准备加入新增的商品的容量大于当前背包的容量时，就直接使用上一个单元格的装入策略。
3. 当j >= w[i]时：v[i] [j] = max{v[i-1] [j]，v[i] + v[i-1] [i-w[i]]}，**//当准备加入的新增的商品的容量小于等于当前背包的容量，//装入的方式：v[i-1] [j]:就是上一个单元格的装入的最大值，v[i]表示当前商品的价值，v[i-1] [j-w[i]]：装入i-1商品，到剩余空间j-w[i]的最大值。**

![image-20210412201228126](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210412201228126.png)

```java
public class KnapsackProblem {
    public static void main(String[] args) {
        int[] w = {1, 4, 3};  //物品的重量
        int[] val = {1500, 3000, 2000};//物品的价值，这里val[i]就是前面讲的v[i]
        int m = 4; //背包的容量
        int n = val.length; //物品的个数

        //创建二维数组
        //v[i][j] 表示在前i个物品中能够装入容量为j的背包的最大价值
        int[][] v = new int[n + 1][m + 1];
        //为了记录放入商品的情况，我们来制定一个二维数组
        int[][] path = new int[n+1][m+1];

        //首先是初始化第一行和第一列，这里在本程序中，可以不去处理，因为默认的是零
        for (int i = 0; i < v.length; i++) {
            v[i][0] = 0;
        }
        for (int i = 0; i < v[0].length; i++) {
            v[0][i] = 0;
        }

        //根据前面得到的公式来动态规划处理
        for (int i = 1; i < v.length; i++) {
            //不处理第一行 i 是从1开始的
            for (int j = 1; j < v[0].length; j++) {
                if (w[i - 1] > j) {
                    v[i][j] = v[i - 1][j];
                } else {
//                    v[i][j] = Math.max(v[i - 1][j], val[i - 1] + v[i - 1][j - w[i - 1]]);
                    //为了记录商品存放到背包的情况，我们不能直接的使用上面的公式，需要使用if-else来体现出公式
                    if (v[i-1][j] < val[i - 1] + v[i - 1][j - w[i - 1]]){
                        v[i][j] = val[i - 1] + v[i - 1][j - w[i - 1]];
                        path[i][j] = 1;
                    } else {
                        v[i][j] = v[i - 1][j];
                    }
                }
            }
        }

        //输出一下V看看目前的情况
        for (int i = 0; i < v.length; i++) {
            for (int j = 0; j < v[i].length; j++) {
                System.out.print(v[i][j] + " ");
            }
            System.out.println();
        }

        //需要认真思考的部分
        int i = path.length -1; //行的最大下标
        int j = path[0].length - 1;//列的最大下标
        while (i > 0 && j > 0){
            if (path[i][j] == 1) {
                System.out.printf("第%d个商品放入到背包\n", i);
                j -= w[i - 1];
            }
            i--;
        }

    }
}
```



**结果**

```
0 0 0 0 0 
0 1500 1500 1500 1500 
0 1500 1500 1500 3000 
0 1500 1500 2000 3500 
第3个商品放入到背包
第1个商品放入到背包
```

## 12.4 KMP算法

**应用场景—字符串匹配问题**

![image-20210413092653533](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413092653533.png)

### 12.4.1 暴力匹配算法

![image-20210413093130622](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413093130622.png)

**代码**

```java
public class ViolenceMatch {
    public static void main(String[] args) {
        //测试暴力匹配算法
        String str1 = "这个程序你都不会，你可真行呀";
        String str2 = "都不会";

        int index = violenceMatch(str1,str2);
        System.out.println("index = "+index);
    }
    //暴力匹配算法实现
    public static int violenceMatch(String str1, String str2){
        char[] s1 = str1.toCharArray();
        char[] s2 = str2.toCharArray();

        int s1Len = s1.length;
        int s2Len = s2.length;

        int i = 0;  //i的索引指向s1
        int j = 0;  //j的索引指向s2
        while (i < s1Len && j < s2Len){
            if (s1[i] == s2[j]){
                //匹配OK
                i++;
                j++;
            } else {
                //如果匹配失败的话
                i = i - (j - 1);
                j = 0;
            }
        }
        //判断是否匹配成功
        if (j == s2Len){
            return i - j;
        } else {
            return -1;
        }
    }
}
```

**结果**

```
index = 5
```

### 12.4.2 KMP算法

**基本介绍**

1. KMP算法是一个解决模式串在文本串是否出现过，如果出现过，最早出现的位置的经典算法
2. Knuth-Morris-Pratt字符串查找算法，简称KMP算法，常用在一个文本串S内查找一个模式串P的出现位置，
3. KMP方法算法就利用之前判断过信息，通过一个next数组，保存模式串中前后最长公共子序列的长度，每次回溯时，通过next数组找到，前面匹配过的位置，省去了大量的计算时间。

**部分匹配值**

![image-20210413103416126](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413103416126.png)

![image-20210413103434122](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413103434122.png)

部分匹配的实质是，有时候，字符串头部和尾部会有重复，比如“ABCDAB"之中有两个”AB“，那么它的部分匹配值就是2。搜索词移动的时候，第一个”AB“向后移动4位（字符串长度-部分匹配值），这样就可以来到第二个AB的位置。

![image-20210413103942370](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413103942370.png)

**代码**

```java
public class KMPAlgorithm {
    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDE";
        String str2 = "ABCDABD";

        int[] next = kmpNext("ABCDABD");
        System.out.println("next = "+ Arrays.toString(next));

        int index = kmpSearch(str1,str2,next);
        System.out.println("index = "+index);
    }

    //写出我们的kmp搜索算法

    /**
     *
     * @param str1  源字符串
     * @param str2  字串
     * @param next  部分匹配表，是字串对应的部分匹配表
     * @return 如果是-1就是没有匹配到，否则就返回第一个匹配的位置
     */
    public static int kmpSearch(String str1, String str2,int[] next){
        //遍历
        for (int i = 0, j = 0;i < str1.length(); i++){

            //需要处理不相等的情况
            //KMP的核心算法
            while (j > 0 && str1.charAt(i) != str2.charAt(j)){
                j = next[j - 1];
            }
            if (str1.charAt(i) == str2.charAt(j)){
                j++;
            }
            if (j == str2.length()){
                //找到了
                return i - j + 1;
            }
        }
        return -1;
    }


    //获取到一个字符串（字串）的部分匹配值表
    public static int[] kmpNext(String dest){
        //创建一个next数组保存部分匹配值
        int[] next = new int[dest.length()];
        next[0] = 0; //如果字符串是长度为1部分匹配值就是0
        for (int i = 1, j = 0; i < dest.length(); i++){
            //当dest.charAt(i) != dest.charAt(j)，我们需要从next[ j -1 ]获取新的j
            //直到我们发现等式成立才退出
            //这个是KMP的核心算法
            while (j > 0 && dest.charAt(i) != dest.charAt(j)){
                j = next[j - 1];
            }
            //当满足dest.charAt(i) == dest.charAt(j)时，部分匹配值就是加1
            if (dest.charAt(i) == dest.charAt(j)){
                j++;
            }
            next[i] = j;
        }
        return next;
    }
}
```

**运行结果**

```
next = [0, 0, 0, 0, 1, 2, 0]
index = 15
```

## 12.5 贪心算法

### 12.5.1 贪心算法的基本介绍

**介绍**

1. 贪心算法又叫做贪婪算法，是指在对问题进行求解时，在每一步选择中都采取最好或者最优（即最有利）的选择，从而希望能够导致结果是最好或者最优的算法。
2. 贪婪算法所得到的结果**不一定是最优的结果（有时候会是最优解），**但是都是相近相似（接近）最优解的结果。

### 12.5.2 案例

假设存在如下表需要付费的广播台，以及广播台可以覆盖的地区，如何选择最少的广播台，让所有的地区都可以接收到信号

![image-20210413135629323](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413135629323.png)

**解决思路**

目前没有算法可以快速计算得到准备的值，使用贪婪算法，则可以得到非常接近的解，并且效率很高，选择策略上，因为需要覆盖全部的地区的最小集合：

1. 遍历所有的广播电台，找到一个覆盖了最多**未覆盖的地区**的电台（此电台可能含有一些已覆盖的地区，但是没有关系）
2. 将这个电台加入到一个集合中（比如ArrayList），想办法把该电台覆盖的地区在下次比较的时候去掉
3. 重复第一步直到覆盖了全部的地区

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;

public class GreedyAlgorithm {
    public static void main(String[] args) {

        //创建广播电台放到Map
        HashMap<String, HashSet<String>> broadcasts = new HashMap<String,HashSet<String>>();
        //将各个电台放入到broacasts
        HashSet<String> hashSet1 = new HashSet<String>();
        hashSet1.add("北京");
        hashSet1.add("上海");
        hashSet1.add("天津");

        HashSet<String> hashSet2 = new HashSet<String>();
        hashSet2.add("广州");
        hashSet2.add("北京");
        hashSet2.add("深圳");

        HashSet<String> hashSet3 = new HashSet<String>();
        hashSet3.add("成都");
        hashSet3.add("上海");
        hashSet3.add("杭州");

        HashSet<String> hashSet4 = new HashSet<String>();
        hashSet3.add("上海");
        hashSet3.add("天津");

        HashSet<String> hashSet5 = new HashSet<String>();
        hashSet5.add("杭州");
        hashSet5.add("大连");

        //加入到map
        broadcasts.put("k1",hashSet1);
        broadcasts.put("k2",hashSet2);
        broadcasts.put("k3",hashSet3);
        broadcasts.put("k4",hashSet4);
        broadcasts.put("k5",hashSet5);

        //allAreas存放所有的地区
        HashSet<String> allAreas = new HashSet<String>();
        allAreas.add("北京");
        allAreas.add("上海");
        allAreas.add("天津");
        allAreas.add("广州");
        allAreas.add("深圳");
        allAreas.add("成都");
        allAreas.add("杭州");
        allAreas.add("大连");

         //创建ArrayList ，存放选择的电台集合
        ArrayList<String> selects = new ArrayList<String>();

        //定义一个临时的集合，在遍历的过程中，存放遍历过程中的电台覆盖的地区和当前还没有覆盖的地区的交集
        HashSet<String> tempSet = new HashSet<String>();

        //定义一个maxKey 保存一次遍历过程中，能够覆盖最大未覆盖的地区对应的电台的key
        //如果maxKey不为null，则会加入到selects
        String maxKey = null;
        while (allAreas.size() != 0){//如果allAreas不为0，则表示还没有覆盖到所有的地区
            //每进行一次while，需要
            maxKey = null;

            //遍历broadcasts，取出对应的key
            for (String key:broadcasts.keySet()){
                //每进行一次for循环
                tempSet.clear();
                //当这个key能够覆盖的地区
                HashSet<String> areas = broadcasts.get(key);
                tempSet.addAll(areas);
                //求出tempSet和allAreas 集合的交集，交集会赋给tempSet
                tempSet.retainAll(allAreas);
                //如果当前这个集合包含的未覆盖地区的数量，比maxKey指向的集合地区还多
                //就需要重置maxKey
                //tempSet.size() > broadcasts.get(maxKey).size()体现出贪心算法的特点，每次都选择最优的。
                if (tempSet.size() > 0 && (maxKey == null || tempSet.size() > broadcasts.get(maxKey).size())){
                    maxKey = key;
                }
            }
            //maxKey != null; 就应该将maxKey 加入 selects
            if (maxKey != null){
                selects.add(maxKey);
                //将maxKey指向的广播电台覆盖的地区，从allAreas去掉
                allAreas.removeAll(broadcasts.get(maxKey));
            }

        }
        System.out.println("得到的选择结果"+selects);
    }
}
```



```
得到的选择结果[k3, k1, k2, k5]
```

## 12.6 普里姆算法

**应用场景**

![image-20210413194558324](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413194558324.png)

1. 有胜利乡有7个村庄（A,B,C,D,E,F,G），现在需要修路把7个村庄连通
2. 各个村庄的距离用边线表示（权），比如A-B距离5公里。
3. 问：如何修路保证各个村庄能连通，并且总的修建公路总里程最短？

**最小生成树**

修路问题的本质就是最小生成树问题，先介绍一下最小生成树（Minimum Cost Spanning Tree）简称MST

1. 给定一个带权的无向连通图，如何选取一棵生成树，使书上所有**边上权的总和最小**，这就叫做最小生成树
2. N个顶点，一定有N-1条边
3. 包含全部顶点
4. N-1条边都在图中
5. 求最小生成树的算法主要是**普利姆算法和克鲁斯卡尔算法**

### 12.6.1 算法介绍

**基本介绍**

![image-20210413195340667](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413195340667.png)

**图解分析**

![image-20210413201036629](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413201036629.png)

### 12.6.2 代码实现

```java
public class PrimAlgorithm {
    public static void main(String[] args) {
        //测试看看图是否创建OK
        char[] data = new char[]{'A','B','C','D','E','F','G'};
        int verxs = data.length;
        //邻接矩阵的关系使用二维数组表示，10000这个树表示两个点是不连通的
        int[][] weight = new int[][]{
                {10000,5,7,10000,10000,10000,2},
                {5,10000,10000,9,10000,10000,3},
                {7,10000,10000,10000,8,10000,10000},
                {10000,9,10000,10000,10000,4,10000},
                {10000,10000,8,10000,10000,5,4},
                {10000,10000,10000,4,5,10000,6},
                {2,3,10000,10000,4,6,10000}
        };
        //创建MGraph对象
        MGraph graph = new MGraph(verxs);
        //创建一个minTree对象
        MinTree minTree = new MinTree();
        minTree.creatGraph(graph,verxs,data,weight);
        //输出
        minTree.showGraph(graph);
        //测试普利姆算法
        minTree.prim(graph,0);
    }
}

//创建最小生成树==>村庄的图
class MinTree{
    //创建图的邻接矩阵

    /**
     *
     * @param graph 图对象
     * @param verxs 图对应的顶点个数
     * @param data 图的各个顶点的值
     * @param weight 图的邻接矩阵
     */
    public void creatGraph(MGraph graph,int verxs, char[] data, int [][] weight){
        int i, j;
        for (i = 0;i<verxs;i++){
            graph.data[i] = data[i];
            for (j = 0; j < verxs; j++){
                graph.weight[i][j] = weight[i][j];
            }
        }
    }
    //显示图的邻接矩阵
    public void showGraph(MGraph graph){
        for (int[] link:graph.weight){
            System.out.println(Arrays.toString(link));
        }
    }
    //编写prim算法，得到最小生成树

    public void prim(MGraph graph,int v){
        //visited[] 标记节点(顶点)是否被访问过
        int[] visited = new int[graph.verxs];
        //把当前这个节点标记为已访问
        visited[v] = 1;
        //h1 和 h2记录两个顶点的下标
        int h1 = -1;
        int h2 = -1;
        int minWeight = 10000;  //将minWeight 初始化成一个很大的值，后面遍历的时候会被替换
        for (int k = 1; k < graph.verxs; k++){
            //因为有n个顶点所以我们只需要n-1条边就可以了
            //这个是确定每一次生成的子图，和哪个节点的距离最近
            for (int i = 0; i<graph.verxs; i++){
                //i节点表示被访问过的节点
                for (int j = 0; j < graph.verxs; j++){
                    //j节点表示还没有访问过的节点
                    if (visited[i] == 1 && visited[j] == 0 && graph.weight[i][j] < minWeight){
                        //替换minWeight(寻找已经访问过的节点和未访问过的节点间的权值最小边）
                        minWeight = graph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
                }
            }
            //找到一条边最小
            System.out.println("边<"+graph.data[h1]+","+graph.data[h2]+"> 权值："+minWeight);
            //将当前这个节点标记未已经访问
            visited[h2] = 1;
            //minWeight 重新设置为最大值10000
            minWeight = 10000;
        }
    }
}

class MGraph{
    int verxs;  //表示图的节点个数
    char[] data;  //存放节点数据
    int[][] weight;  //存放边，就是我们的邻接矩阵

    public MGraph(int verxs){
        this.verxs = verxs;
        data = new char[verxs];
        weight = new int[verxs][verxs];
    }
}
```

```
[10000, 5, 7, 10000, 10000, 10000, 2]
[5, 10000, 10000, 9, 10000, 10000, 3]
[7, 10000, 10000, 10000, 8, 10000, 10000]
[10000, 9, 10000, 10000, 10000, 4, 10000]
[10000, 10000, 8, 10000, 10000, 5, 4]
[10000, 10000, 10000, 4, 5, 10000, 6]
[2, 3, 10000, 10000, 4, 6, 10000]
边<A,G> 权值：2
边<G,B> 权值：3
边<G,E> 权值：4
边<E,F> 权值：5
边<F,D> 权值：4
边<A,C> 权值：7
```

## 12.7 克鲁斯卡尔算法

**应用场景**

![image-20210414092032042](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210414092032042.png)

1. 某城市新增7个站点，现在需要修路把七个站点连通
2. 各个站点的距离用边线表示（权），比如A-B距离12公里
3. 问：如何修路保证各个站点都能连通，并且总的修建公里总路程最短

### 12.7.1 算法的基本介绍

**介绍**

1. 克鲁斯卡尔算法，是用来求加权连通图的最小生成树的算法
2. 基本思想：按照权值从小到大的顺序选择n-1条边，并保证这n-1条边不构成回路
3. 具体做法：首先构造一个只含n个顶点的森林，然后依权值从小到大从连通网中选择加入到森林中去，并使森林不产生回路，直至森林变成一棵树为止。

**图解分析**

![image-20210414093423761](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210414093423761.png)

![image-20210414093634959](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210414093634959.png)

![image-20210414095300212](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210414095300212.png)

### 12.7.2 算法的实现

```java
public class KruskalCase {

    private int edgNum;  //边的个数
    private char[] vertexs;  //顶点数组
    private int[][] matrix;  //邻接矩阵

    //使用INF 表示两个顶点不能连通
    private static final int INF = Integer.MAX_VALUE;

    public static void main(String[] args) {
        char[] vertexs = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        //克鲁斯卡尔算法的邻接矩阵
        int matrix[][] = {
                {0, 12, INF, INF, INF, 16, 14},
                {12, 0, 10, INF, INF, 7, INF},
                {INF, 10, 0, 3, 5, 6, INF},
                {INF, INF, 3, 0, 4, INF, INF},
                {INF, INF, 5, 4, 0, 2, 8},
                {16, 7, 6, INF, 2, 0, 9},
                {14, INF, INF, INF, 8, 9, 0}
        };
        //创建KruskalCase对象实例
        KruskalCase kruskalCase = new KruskalCase(vertexs, matrix);
        //输出创建
        kruskalCase.print();
        kruskalCase.kruskal();
    }

    //构造器
    public KruskalCase(char[] vertexs, int[][] matrix) {
        //初始化定点数和边的个数
        int vlen = vertexs.length;

        //初始化顶点，复制拷贝的方式
        this.vertexs = new char[vlen];
        for (int i = 0; i < vertexs.length; i++) {
            this.vertexs[i] = vertexs[i];
        }

        //初始化边，使用的是复制拷贝的方式
        this.matrix = new int[vlen][vlen];
        for (int i = 0; i < vlen; i++) {
            for (int j = 0; j < vlen; j++) {
                this.matrix[i][j] = matrix[i][j];
            }
        }
        //统计边
        for (int i = 0; i < vlen; i++) {
            for (int j = i + 1; j < vlen; j++) {
                if (this.matrix[i][j] != INF) {
                    edgNum++;
                }
            }
        }
    }

    public void kruskal(){
        int index = 0;  //表示最后结果数组的索引
        int[] ends = new int[edgNum];  //用于保存“已有最小生成树”中的每个顶点在最小生成树的终点
        //创建结果数组，保存最后的最小生成树
        EData[] rets = new EData[edgNum];

        //获取图中所有边的合集，一共有12条边
        EData[] edges = getEdges();
        System.out.println("图的边的集合"+ Arrays.toString(edges)+"共"+edges.length+"条边");

        //按照边的权值大小进行排序(从小到大)
        sortEdges(edges);

        //遍历edges 数组，将边添加到最小生成树中时，判断准备加入的边是否形成了回路，如果没有，就加入rets，否则不能加入
        for (int i = 0; i <edgNum; i++){
            //获取到第i条边的第一个顶点(起点)
            int p1 = getPosition(edges[i].start);
            //获取到第i条边的第二个顶点
            int p2 = getPosition(edges[i].end);

            //获取p1这个顶点在已有最小生成树中的终点
            int m = getEnd(ends,p1);
            //获取p2这个顶点在已有最小生成树中的终点
            int n = getEnd(ends,p2);
            //是否构成回路
            if (m != n){
                //没有构成回路
                ends[m] = n;  //设置m在"已有最小生成树“中的终点
                rets[index++] = edges[i];  //有一条边加入到rets数组
            }
        }
        //统计并打印：”最小生成树“，输出rets
        System.out.println("最小生成树为");
        for (int i = 0; i < index; i++){
            System.out.println(rets[i]);
        }
    }

    //打印邻接矩阵
    public void print() {
        System.out.println("邻接矩阵为： \n");
        for (int i = 0; i < vertexs.length; i++) {
            for (int j = 0; j < vertexs.length; j++) {
                System.out.printf("%15d", matrix[i][j]);
            }
            System.out.println();
        }
    }

    /**
     * 功能：对边进行排序，冒泡排序
     *
     * @param edges 边的合集
     */
    private void sortEdges(EData[] edges) {
        for (int i = 0; i < edges.length - 1; i++) {
            for (int j = 0; j < edges.length - 1 - i; j++) {
                if (edges[j].weight > edges[j + 1].weight) {
                    //交换
                    EData temp = edges[j];
                    edges[j] = edges[j + 1];
                    edges[j + 1] = temp;
                }
            }
        }
    }

    /**
     * @param ch 顶点的值，比如'A','B'
     * @return 返回ch顶点对应的下标，如果找不到，就返回-1
     */
    private int getPosition(char ch) {
        for (int i = 0; i < vertexs.length; i++) {
            if (vertexs[i] == ch) {
                return i;
            }
        }
        //找不到就返回-1
        return -1;
    }

    /**
     * 功能：获取图中边，放到EData[] 数组里面，后面我们需要遍历该数组，是通过matrix邻接矩阵来获取
     * EData[] 形式[[‘A’,'B',12],['B','F',7],....]
     *
     * @return
     */
    private EData[] getEdges() {
        int index = 0;
        EData[] edges = new EData[edgNum];
        for (int i = 0; i < vertexs.length; i++) {
            for (int j = i + 1; j < vertexs.length; j++) {
                if (matrix[i][j] != INF) {
                    edges[index++] = new EData(vertexs[i], vertexs[j], matrix[i][j]);
                }
            }
        }
        return edges;
    }

    /**
     * 功能：获取下标为i的顶点的终点，用于后面判断两个顶点的终点是否相同
     * @param ends 数组就是记录了各个顶点对应的终点是哪个，ends数组是在遍历过程中，逐步形成
     * @param i 表示传入的顶点对应的下标
     * @return 返回的就是下标为i的这个顶点对应的终点的下标，
     */
    private int getEnd(int[] ends, int i){
        while (ends[i] != 0){
            i = ends[i];
        }
        return i;
    }
}

//创建一个类EData，它们对象实例就表示一个边
class EData {
    char start; //边的一个点
    char end;  //边的另外一个点
    int weight;  //边的权值

    //构造器
    public EData(char start, char end, int weight) {
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    //重写toString，便于输出边信息
    @Override
    public String toString() {
        return "EData [<" + start + ", " + end + "> ===>" + weight + "]";
    }
}
```



```
邻接矩阵为： 

              0             12     2147483647     2147483647     2147483647             16             14
             12              0             10     2147483647     2147483647              7     2147483647
     2147483647             10              0              3              5              6     2147483647
     2147483647     2147483647              3              0              4     2147483647     2147483647
     2147483647     2147483647              5              4              0              2              8
             16              7              6     2147483647              2              0              9
             14     2147483647     2147483647     2147483647              8              9              0
图的边的集合[EData [<A, B> ===>12], EData [<A, F> ===>16], EData [<A, G> ===>14], EData [<B, C> ===>10], EData [<B, F> ===>7], EData [<C, D> ===>3], EData [<C, E> ===>5], EData [<C, F> ===>6], EData [<D, E> ===>4], EData [<E, F> ===>2], EData [<E, G> ===>8], EData [<F, G> ===>9]]共12条边
最小生成树为
EData [<E, F> ===>2]
EData [<C, D> ===>3]
EData [<D, E> ===>4]
EData [<B, F> ===>7]
EData [<E, G> ===>8]
EData [<A, B> ===>12]
```

## 12.8 迪杰斯特拉算法

**案例**

![image-20210413194558324](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210413194558324.png)

1. 战争时期，胜利乡有7个村庄，现在又六个邮差，从G点出发，，需要分别把邮件送到ABCDEF六个村庄
2. 各个村庄的距离用边线表示，
3. 问：如何计算出G村庄到其他各个村庄的最短距离。
4. 如果从其他点出发到各个点的最短距离又是多少呢？

### 12.8.1 迪杰斯特拉算法的介绍

**介绍**

迪杰斯特拉算法是典型最短路劲算法，用于计算一个节点到其他节点的最短路径，它的主要特点是以起点为中心向外层扩展（广度优先搜索算法），直到扩展到终点为止。

**算法过程**

![image-20210414165541324](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210414165541324.png)

### 12.8.2 算法

```java
public class DijkstraAlgorithm {
    public static void main(String[] args) {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;  //表示不可连接
        matrix[0] = new int[]{N, 5, 7, N, N, N, 2};
        matrix[1] = new int[]{5, N, N, 9, N, N, 3};
        matrix[2] = new int[]{7, N, N, N, 8, N, N};
        matrix[3] = new int[]{N, 9, N, N, N, 4, N};
        matrix[4] = new int[]{N, N, 8, N, N, 5, 4};
        matrix[5] = new int[]{N, N, N, 4, 5, N, 6};
        matrix[6] = new int[]{2, 3, N, N, 4, 6, N};
        //创建Graph对象
        Graph graph = new Graph(vertex, matrix);
        //测试
        graph.showGraph();
        graph.dsj(6);
        graph.showDijkstra();
    }
}

class Graph {
    private char[] vertex;  //顶点数组
    private int[][] matrix;  //邻接矩阵
    private VisitedVertex vv; //已经访问的顶点

    //构造器
    public Graph(char[] vertex, int[][] matrix) {
        this.vertex = vertex;
        this.matrix = matrix;
    }
    //显示结果
    public void showDijkstra(){
        vv.show();
    }

    //显示图
    public void showGraph() {
        for (int[] link : matrix) {
            System.out.println(Arrays.toString(link));
        }
    }
    //迪杰斯特拉算法的实现

    /**
     *
     * @param index 表示出发顶点对应的下标
     */
    public void dsj(int index){
        vv = new VisitedVertex(vertex.length,index);
        update(index); //跟新index顶点到周围顶点的距离和前驱顶点
        for (int j = 1; j < vertex.length; j++){
            index = vv.updateArr(); //选择并返回新的访问节点
            update(index);
        }
    }

    //更新index下标顶点到周围顶点的距离和周围顶点的前驱节点
    private void update(int index){
        int len = 0;
        //根据遍历我们的邻接矩阵的matrix[index]行
        for (int j = 0; j < matrix[index].length; j++){
            //len的含义是出发顶点到index的距离+从index顶点到j顶点的距离之和
            len = vv.getDis(index) + matrix[index][j];
            //如果j顶点没有被访问过，并且len小于出发顶点到j顶点的距离就需要更新
            if (!vv.in(j) && len < vv.getDis(j)){
                vv.updateDis(j,len);
                vv.updatePre(j,index);
            }
        }
    }
}

//已访问过顶点集合
class VisitedVertex {
    //记录各个顶点是否访问过：1表示访问过，0表示未访问，会动态更新
    public int[] already_arr;
    //每个下标对应的值为前一个顶点下标，会动态更新
    public int[] pre_visited;
    //记录出发顶点到其他所有顶点的距离，比如G为出发顶点，就会记录G到其他顶点的记录，会动态更新，求最短距离就会放到dis
    public int[] dis;

    //构造器

    /**
     * @param length :
     * @param index
     */
    public VisitedVertex(int length, int index) {
        this.already_arr = new int[length];
        this.pre_visited = new int[length];
        this.dis = new int[length];

        //初始化dis数组
        Arrays.fill(dis, 65535);
        this.already_arr[index] = 1;
        this.dis[index] = 0; //设置出发顶点的访问距离为0
    }

    /**
     * 功能：判断Index顶点是否被访问过
     * @param index
     * @return 如果访问过就返回true，否则的话返回false
     */
    public boolean in(int index){
        return already_arr[index] == 1;
    }

    /**
     * 功能：更新出发顶点到Index顶点的距离
     * @param index
     * @param len
     */
    public void updateDis(int index, int len){
        dis[index] = len;
    }

    /**
     * 功能：更新pre这个顶点的前驱顶点为index顶点
     * @param pre
     * @param index
     */
    public void updatePre(int pre, int index){
        pre_visited[pre] = index;
    }

    /**
     * 功能：返回出发顶点到Index顶点的距离
     * @param index
     * @return
     */
    public int getDis(int index){
        return dis[index];
    }

    /**
     * 继续选择并返回新的访问顶点，比如这里的G完后，就是A点作为新的访问顶点（注意不是出发点）
     * @return
     */
    public int updateArr(){
        int min = 65535, index = 0;
        for (int i = 0; i < already_arr.length; i++){
            if (already_arr[i] == 0 && dis[i] < min){
                min = dis[i];
                index = i;
            }
        }
        //更新Index顶点被访问过
        already_arr[index] = 1;
        return index;
    }

    //显示最后的结果，就是将三个数组的情况输出
    public void show(){
        System.out.println("=================================");
        //输出already_arr
        for (int i : already_arr){
            System.out.print(i + " ");
        }
        System.out.println();
        //输出pre_visited
        for (int i : pre_visited){
            System.out.print(i + " ");
        }
        System.out.println();
        //输出dis
        for (int i : dis){
            System.out.print(i + " ");
        }
        System.out.println();

        //为了好看最后的最短距离
        char[] vertex = {'A','B','C','D','E','F','G'};
        int count = 0;
        for (int i : dis){
            if (i != 65535){
                System.out.print(vertex[count]+"{"+i+"}");
            } else {
                System.out.println("N");
            }
            count++;
        }
        System.out.println();
    }
}
```



```
[65535, 5, 7, 65535, 65535, 65535, 2]
[5, 65535, 65535, 9, 65535, 65535, 3]
[7, 65535, 65535, 65535, 8, 65535, 65535]
[65535, 9, 65535, 65535, 65535, 4, 65535]
[65535, 65535, 8, 65535, 65535, 5, 4]
[65535, 65535, 65535, 4, 5, 65535, 6]
[2, 3, 65535, 65535, 4, 6, 65535]
=================================
1 1 1 1 1 1 1 
6 6 0 5 6 6 0 
2 3 9 10 4 6 0 
A{2}B{3}C{9}D{10}E{4}F{6}G{0}
```



## 12.9 弗洛伊德算法

### 12.9.1 算法介绍

**基本介绍**

1. 和Dijkstra算法一样，弗洛伊德算法也是一种用于寻找给定的加权图中顶点间最短路径的算法。
2. 弗洛伊德算法计算图中各个顶点之间的最短距离
3. 迪杰斯特拉算法用于计算图中某一个顶点到其他顶点的最短距离
4. **弗洛伊德算法VS迪杰斯特拉算法**：迪杰斯特拉算法通过选定的被访问的点，求出从出发访问顶点到其他顶点的最短路径，弗洛伊德算法中的**每一个顶点都是出发访问顶点，所以需要将每一个顶点看做被访问顶点，求出从每一个顶点到其他顶点的最短路径**。

**图解分析**

![image-20210415110923017](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415110923017.png)

**参考这张图分析**

![image-20210413194558324](file://C:/Users/dongwei/AppData/Roaming/Typora/typora-user-images/image-20210413194558324.png?lastModify=1618455236)





![image-20210415121407511](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415121407511.png)

![image-20210415121812213](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415121812213.png)



**将A作为中间顶点的情况有：**

1. C ==> A ==> G [9]
2. C ==> A ==> B [12]
3. G ==> A ==> B [7]  但是本身G==>B是[3]所以不需要更新

![image-20210415121831552](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415121831552.png)

**算法的核心**

中间顶点[A,B,C,D,E,F,G]

出发顶点[A,B,C,D,E,F,G]

终点       [A,B,C,D,E,F,G]

使用三层循环来更新距离表和前驱关系表，**因此弗洛伊德算法的复杂度是n^3**

### 12.9.2 算法的实现

```java
public class FloydAlgorithm {
    public static void main(String[] args) {
        //测试图是否创建成功
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;  //表示不可连接
        matrix[0] = new int[]{0, 5, 7, N, N, N, 2};
        matrix[1] = new int[]{5, 0, N, 9, N, N, 3};
        matrix[2] = new int[]{7, N, 0, N, 8, N, N};
        matrix[3] = new int[]{N, 9, N, 0, N, 4, N};
        matrix[4] = new int[]{N, N, 8, N, 0, 5, 4};
        matrix[5] = new int[]{N, N, N, 4, 5, 0, 6};
        matrix[6] = new int[]{2, 3, N, N, 4, 6, 0};

        Graph graph = new Graph(vertex.length,matrix,vertex);

        graph.floyd();
        graph.show();
    }
}

//创建图
class Graph {
    private char[] vertex;  //存放顶点的数组
    private int[][] dis; //保存，从各个顶点出发到其他顶点的距离，最后的结果，也是保留在该数组
    private int[][] pre; //保存到达目的顶点的前驱顶点

    //构造器

    /**
     * @param length 大小
     * @param matrix 邻接矩阵
     * @param vertex 顶点数组
     */
    public Graph(int length, int[][] matrix, char[] vertex) {
        this.vertex = vertex;
        this.dis = matrix;
        this.pre = new int[length][length];
        //对pre数组初始化，注意放的是前驱顶点的下标
        for (int i = 0; i < length; i++) {
            Arrays.fill(pre[i], i);
        }
    }

    //显示pre数组和dis数组
    public void show() {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        for (int k = 0; k < dis.length; k++) {
            //先将pre数组输出一行
            for (int i = 0; i < pre.length; i++) {
                System.out.print(vertex[pre[k][i]] + " ");
            }
            System.out.println();
            for (int i = 0; i < dis.length; i++) {
                System.out.print("("+vertex[k]+"==>"+vertex[i]+"的最短路径是"+dis[k][i] + ")  ");
            }
            System.out.println();
            System.out.println();
        }
    }

    //弗洛伊德算法
    public void floyd(){
        int len = 0; //变量保存距离
        //对中间顶点遍历，k 就是中间顶点的下标[A,B,C,D,E,F,G]
        for (int k = 0; k < dis.length; k++){
            for (int i = 0; i < dis.length; i++){
                for (int j = 0; j < dis.length; j++){
                    len = dis[i][k] + dis[k][j];  //==>求出从i顶点出发，经过k中间顶点，到达j顶点距离
                    if (len < dis[i][j]){
                        //如果len小于dis[i][j]
                        dis[i][j] = len;  //更新距离
                        pre[i][j] = pre[k][j];
                    }
                }
            }
        }
    }

}
```



```
A A A F G G A 
(A==>A的最短路径是0)  (A==>B的最短路径是5)  (A==>C的最短路径是7)  (A==>D的最短路径是12)  (A==>E的最短路径是6)  (A==>F的最短路径是8)  (A==>G的最短路径是2)  

B B A B G G B 
(B==>A的最短路径是5)  (B==>B的最短路径是0)  (B==>C的最短路径是12)  (B==>D的最短路径是9)  (B==>E的最短路径是7)  (B==>F的最短路径是9)  (B==>G的最短路径是3)  

C A C F C E A 
(C==>A的最短路径是7)  (C==>B的最短路径是12)  (C==>C的最短路径是0)  (C==>D的最短路径是17)  (C==>E的最短路径是8)  (C==>F的最短路径是13)  (C==>G的最短路径是9)  

G D E D F D F 
(D==>A的最短路径是12)  (D==>B的最短路径是9)  (D==>C的最短路径是17)  (D==>D的最短路径是0)  (D==>E的最短路径是9)  (D==>F的最短路径是4)  (D==>G的最短路径是10)  

G G E F E E E 
(E==>A的最短路径是6)  (E==>B的最短路径是7)  (E==>C的最短路径是8)  (E==>D的最短路径是9)  (E==>E的最短路径是0)  (E==>F的最短路径是5)  (E==>G的最短路径是4)  

G G E F F F F 
(F==>A的最短路径是8)  (F==>B的最短路径是9)  (F==>C的最短路径是13)  (F==>D的最短路径是4)  (F==>E的最短路径是5)  (F==>F的最短路径是0)  (F==>G的最短路径是6)  

G G A F G G G 
(G==>A的最短路径是2)  (G==>B的最短路径是3)  (G==>C的最短路径是9)  (G==>D的最短路径是10)  (G==>E的最短路径是4)  (G==>F的最短路径是6)  (G==>G的最短路径是0)
```



## 12.10 马踏棋盘算法

### 12.10.1 算法介绍

![image-20210415143950639](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415143950639.png)

**图文讲解**

![image-20210415144124042](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415144124042.png)

![image-20210415145038706](C:\Users\dongwei\AppData\Roaming\Typora\typora-user-images\image-20210415145038706.png)

### 12.10.2 代码实现

```java
public class HorseChessBoard {

    private static int X; //棋盘的列数
    private static int Y; //棋盘的行数
    //创建一个数组，标记棋盘的各个位置是否被访问过
    private static boolean visited[];
    //使用一个属性，标记是否棋盘所有的位置都被访问
    private static boolean finished;  //如果为true，表示成功

    public static void main(String[] args) {
        //测试
        X = 8;
        Y = 8;
        int row = 1;
        int column = 1;
        //创建棋盘
        int[][] chessboard = new int[X][Y];
        visited = new boolean[X*Y];
        //测试一下耗时
        long start = System.currentTimeMillis();
        traversalChessboard(chessboard,row-1,column-1,1);
        long end = System.currentTimeMillis();
        System.out.println("共耗"+(end- start)+"毫秒");

        //输出棋盘的最后情况
        for (int[] rows:chessboard){
            for (int step:rows){
                System.out.print(step +"\t");
            }
            System.out.println();
        }

    }
    public static void traversalChessboard(int[][] chessboard, int row, int column, int step){
        chessboard[row][column] = step;
        visited[row * X + column] = true;//标记该位置已经被访问
        ArrayList<Point> ps = next(new Point(column,row));
        //遍历ps
        while (!ps.isEmpty()){
            Point p = ps.remove(0); //取出下一个可以走的位置
            //判断该点是否已经够访问过
            if (!visited[p.y * X + p.x]){
                //说明还没有访问过
                traversalChessboard(chessboard,p.y,p.x,step+1);
            }
        }
        //判断马儿是否完成了任务，使用step和应该走的步数比较，如果没有达到数量，则表示没有完成任务，将整个棋盘置为0
        //说明：step < X*Y 成立的情况有两种
        //1. 棋盘到目前为止，仍然没有走完
        //2. 棋盘处于一个回溯的过程
        if (step < X*Y && !finished){
            chessboard[row][column] = 0;
            visited[row * X + column] = false;
        } else {
            finished = true;
        }
    }

    /**
     * 功能：根据当前位置(Point对象)，计算马儿还能走那些位置(Point)，并放入到一个集合中(ArrayList)，最多有八个位置
     * @param curPoint
     * @return
     */
    public static ArrayList<Point> next(Point curPoint){
        //创建一个ArrayList
        ArrayList<Point> ps = new ArrayList<Point>();
        //创建一个Point
        Point p1 = new Point();
        //表示马儿可以走5这个位置
        if ((p1.x = curPoint.x - 2) >= 0 && (p1.y = curPoint.y -1) >= 0){
            ps.add(new Point(p1));
        }
        //表示马儿可以走6这个位置
        if ((p1.x = curPoint.x - 1) >= 0 && (p1.y = curPoint.y -2) >= 0){
            ps.add(new Point(p1));
        }
        //表示马儿可以走7这个位置
        if ((p1.x = curPoint.x + 1) < X && (p1.y = curPoint.y -2) >= 0){
            ps.add(new Point(p1));
        }
        //表示马儿可以走0这个位置
        if ((p1.x = curPoint.x + 2) < X && (p1.y = curPoint.y -1) >= 0){
            ps.add(new Point(p1));
        }
        //表示马儿可以走1这个位置
        if ((p1.x = curPoint.x + 2) < X && (p1.y = curPoint.y +1) < Y){
            ps.add(new Point(p1));
        }
        //表示马儿可以走2这个位置
        if ((p1.x = curPoint.x + 1) < X && (p1.y = curPoint.y +2) < Y){
            ps.add(new Point(p1));
        }
        //表示马儿可以走3这个位置
        if ((p1.x = curPoint.x - 1) >= 0 && (p1.y = curPoint.y +2) < Y){
            ps.add(new Point(p1));
        }
        //表示马儿可以走4这个位置
        if ((p1.x = curPoint.x - 2) >= 0 && (p1.y = curPoint.y +1) < Y){
            ps.add(new Point(p1));
        }
        return ps;
    }
}
```

```
共耗103097毫秒
1	8	11	16	3	18	13	64	
10	27	2	7	12	15	4	19	
53	24	9	28	17	6	63	14	
26	39	52	23	62	29	20	5	
43	54	25	38	51	22	33	30	
40	57	42	61	32	35	48	21	
55	44	59	50	37	46	31	34	
58	41	56	45	60	49	36	47	
```

### 12.10.3 使用贪心算法优化

**使用贪心算法对原来的算法优化**

1. 我们获取当前位置，可以走的下一个位置的集合。获取当前位置可以走的下一个位置的集合ArrayList<Point> ps = next(new Point(cloumn,row))
2. 我们需要对ps中所有的**Point**的下一步的所有集合的数目，进行非递减排序就OK

9 , 7 , 6 , 5 , **4** , 3 , 2 , 1 //递减排序

1 , 2 , 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10 //递增排序

1 ， 2  ，2 ， 2 ，3 ， 3  ， 4 ， 5 ， 6 //非递减

9，7，6 ， 6， 5 ， 5， 3 ， 2， 1 //非递增

```java
//根据当前这一步的所有下一步的选择位置，进行非递减排序，减少回溯的次数
    public static void sort(ArrayList<Point> ps){
        ps.sort(new Comparator<Point>() {
            @Override
            public int compare(Point o1, Point o2) {
                //获取01的下一步的所有位置个数
                int count1 = next(o1).size();
                //获取到02的下一步的所有位置个数
                int count2 = next(o2).size();
                if (count1 < count2){
                    return -1;
                } else if (count1 == count2){
                    return 0;
                } else {
                    return 1;
                }
            }
        });
    }
```



```
共耗56毫秒
1	16	37	32	3	18	47	22	
38	31	2	17	48	21	4	19	
15	36	49	54	33	64	23	46	
30	39	60	35	50	53	20	5	
61	14	55	52	63	34	45	24	
40	29	62	59	56	51	6	9	
13	58	27	42	11	8	25	44	
28	41	12	57	26	43	10	7	
```



































































































