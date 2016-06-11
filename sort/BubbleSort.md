# 冒泡排序

**基本思想**：在要排序的一组数中，对当前还未排好序的范围内的全部数，自上而下对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。即：每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。

> 冒泡排序是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

![冒泡排序](https://i.imgur.com/rV4HFJ9.png)

**Java实现1**

```java
public class BubbleSort {
    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //冒泡排序
        for (int i = 0; i < a.length; i++) {
            for(int j = 0; j<a.length-i-1; j++){
                //这里-i主要是每遍历一次都把最大的i个数沉到最底下去了，没有必要再替换了
                if(a[j]>a[j+1]){
                    int temp = a[j];
                    a[j] = a[j+1];
                    a[j+1] = temp;
                }
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

**Java实现2**

```java
/**  
 * 冒泡法排序<br/>  

 * <li>比较相邻的元素。如果第一个比第二个大，就交换他们两个。</li>  
 * <li>对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。</li>  
 * <li>针对所有的元素重复以上的步骤，除了最后一个。</li>  
 * <li>持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。</li>  
 *   
 * @param numbers  需要排序的整型数组  
 */  
public static void bubbleSort(int[] numbers) {   
    int temp; // 记录临时中间值   
    int size = numbers.length; // 数组大小   
    for (int i = 0; i < size - 1; i++) {   
        for (int j = i + 1; j < size; j++) {   
            if (numbers[i] < numbers[j]) { // 交换两数的位置   
                temp = numbers[i];   
                numbers[i] = numbers[j];   
                numbers[j] = temp;   
            }   
        }   
    }   
}
```

**Java实现3**

```java
public class BubbleSort<T extends Comparable<T>> {

    private BubbleSort() { }

    public static <T extends Comparable<T>> T[] sort(T[] unsorted) {
        boolean swapped = true;
        int length = unsorted.length;
        while (swapped) {
            swapped = false;
            for (int i = 1; i < length; i++) {
                if (unsorted[i].compareTo(unsorted[i - 1]) < 0) {
                    swap(i, i - 1, unsorted);
                    swapped = true;
                }
            }
            length--;
        }
        return unsorted;
    }

    private static <T extends Comparable<T>> void swap(int index1, int index2, T[] unsorted) {
        T value = unsorted[index1];
        unsorted[index1] = unsorted[index2];
        unsorted[index2] = value;
    }
}
```

> **Notice**
> 
> 冒泡排序是一种稳定的排序方法。　

* 若文件初状为正序，则一趟起泡就可完成排序，排序码的比较次数为`n-1`，且没有记录移动，时间复杂度是`O(n)`
* 若文件初态为逆序，则需要`n-1`趟起泡，每趟进行`n-i`次排序码的比较，且每次比较都移动三次，比较和移动次数均达到最大值 `O(n2)`
* 起泡排序平均时间复杂度为`O(n2)`