# 希尔排序

> 基本思想：先取一个小于`n`的整数`d1`作为第一个增量，把文件的全部记录分成`d1`个组。所有距离为`d1`的倍数的记录放在同一个组中。先在各组内进行直接插入排序；然后，取第二个增量`d2<d1`重复上述的分组和排序，直至所取的增量`dt=1(dt<dt-l<…<d2<d1)`，即所有记录放在同一组中进行直接插入排序为止。该方法实质上是一种分组插入方法。

![希尔排序](https://i.imgur.com/8ZWqp9l.png)

**Java实现**

```java
//不稳定
public class ShellSort {

    
    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //希尔排序
        int d = a.length;
        while(true){
            d = d / 2;
            for(int x=0;x<d;x++){
                for(int i=x+d;i<a.length;i=i+d){
                    int temp = a[i];
                    int j;
                    for(j=i-d;j>=0&&a[j]>temp;j=j-d){
                        a[j+d] = a[j];
                    }
                    a[j+d] = temp;
                }
            }
            if(d == 1){
                break;
            }
        }
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

}
```

> **Notice**

> 我们知道一次插入排序是稳定的，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以希尔排序是不稳定的。

**希尔排序的时间性能优于直接插入排序，原因如下：**

**（1）**当文件初态基本有序时直接插入排序所需的比较和移动次数均较少。

**（2）**当n值较小时，n和n2的差别也较小，即直接插入排序的最好时间复杂度O(n)和最坏时间复杂度0(n2)差别不大。

**（3）**在希尔排序开始时增量较大，分组较多，每组的记录数目少，故各组内直接插入较快，后来增量di逐渐缩小，分组数逐渐减少，而各组的记录数目逐渐增多，但由于已经按di-1作为距离排过序，使文件较接近于有序状态，所以新的一趟排序过程也较快。

因此，希尔排序在效率上较直接插人排序有较大的改进。
希尔排序的平均时间复杂度为`O(nlogn)`。
