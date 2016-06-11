# 快速排序

**基本思想**：选择一个基准元素,通常选择第一个元素或者最后一个元素,通过一趟扫描，将待排序列分成两部分,一部分比基准元素小,一部分大于等于基准元素,此时基准元素在其排好序后的正确位置,然后再用同样的方法递归地排序划分的两部分。

![快速排序](https://i.imgur.com/WTvQuIA.png)

**Java实现1**

```java
//不稳定
public class 快速排序 {
    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //快速排序
        quick(a);
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

    private static void quick(int[] a) {
        if(a.length>0){
            quickSort(a,0,a.length-1);
        }
    }

    private static void quickSort(int[] a, int low, int high) {
        if(low<high){ //如果不加这个判断递归会无法退出导致堆栈溢出异常
            int middle = getMiddle(a,low,high);
            quickSort(a, 0, middle-1);
            quickSort(a, middle+1, high);
        }
    }

    private static int getMiddle(int[] a, int low, int high) {
        int temp = a[low];//基准元素
        while(low<high){
            //找到比基准元素小的元素位置
            while(low<high && a[high]>=temp){
                high--;
            }
            a[low] = a[high]; 
            while(low<high && a[low]<=temp){
                low++;
            }
            a[high] = a[low];
        }
        a[low] = temp;
        return low;
    }
}
```

**Java实现2**

```java
/**  
 * 快速排序<br/>  
 * <ul>  
 * <li>从数列中挑出一个元素，称为“基准”</li>  
 * <li>重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分割之后，  
 * 该基准是它的最后位置。这个称为分割（partition）操作。</li>  
 * <li>递归地把小于基准值元素的子数列和大于基准值元素的子数列排序。</li>  
 * </ul>  
 *   
 * @param numbers  
 * @param start  
 * @param end  
 */  
public static void quickSort(int[] numbers, int start, int end) {   
    if (start < end) {   
        int base = numbers[start]; // 选定的基准值（第一个数值作为基准值）   
        int temp; // 记录临时中间值   
        int i = start, j = end;   
        do {   
            while ((numbers[i] < base) && (i < end))   
                i++;   
            while ((numbers[j] > base) && (j > start))   
                j--;   
            if (i <= j) {   
                temp = numbers[i];   
                numbers[i] = numbers[j];   
                numbers[j] = temp;   
                i++;   
                j--;   
            }   
        } while (i <= j);   
        if (start < j)   
            quickSort(numbers, start, j);   
        if (end > i)   
            quickSort(numbers, i, end);   
    }   
}
```

> **Notice**
> 
> 快速排序是不稳定的排序。

* 快速排序的时间复杂度为O(nlogn)。
* 当n较大时使用快排比较好，当序列基本有序时用快排反而不好。