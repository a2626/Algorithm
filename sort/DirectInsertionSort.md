# 直接插入排序 （从后向前找到合适位置后插入）

**基本思想**：每步将一个待排序的记录，按其顺序码大小插入到前面已经排序的字序列的合适位置（从后向前找到合适位置后），直到全部插入排序完为止。

![直接插入排序](https://i.imgur.com/UJAXA5m.png)

**Java实现1**

```java
public class DirectInsertionSort {

    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //直接插入排序
        for (int i = 1; i < a.length; i++) {
            //待插入元素
            int temp = a[i];
            int j;
            /*for (j = i-1; j>=0 && a[j]>temp; j--) {
                //将大于temp的往后移动一位
                a[j+1] = a[j];
            }*/
            for (j = i-1; j>=0; j--) {
                //将大于temp的往后移动一位
                if(a[j]>temp){
                    a[j+1] = a[j];
                }else{
                    break;
                }
            }
            a[j+1] = temp;
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
 * 插入排序<br/>  
 * <ul>  
 * <li>从第一个元素开始，该元素可以认为已经被排序</li>  
 * <li>取出下一个元素，在已经排序的元素序列中从后向前扫描</li>  
 * <li>如果该元素（已排序）大于新元素，将该元素移到下一位置</li>  
 * <li>重复步骤3，直到找到已排序的元素小于或者等于新元素的位置</li>  
 * <li>将新元素插入到该位置中</li>  
 * <li>重复步骤2</li>  
 * </ul>  
 * @param numbers  
 */  
public static void insertSort(int[] numbers) {   
    int size = numbers.length, temp, j;   
    for(int i=1; i<size; i++) {   
        temp = numbers[i];   
        for(j = i; j > 0 && temp < numbers[j-1]; j--)   
            numbers[j] = numbers[j-1];   
        numbers[j] = temp;   
    }   
}
```

**Java实现3**

```java
public class InsertionSort<T extends Comparable<T>> {

    private InsertionSort() { }

    public static <T extends Comparable<T>> T[] sort(T[] unsorted) {
        int length = unsorted.length;
        for (int i = 1; i < length; i++) {
            sort(i, unsorted);
        }
        return unsorted;
    }

    private static <T extends Comparable<T>> void sort(int i, T[] unsorted) {
        for (int j = i; j > 0; j--) {
            T jthElement = unsorted[j];
            T jMinusOneElement = unsorted[j - 1];
            if (jthElement.compareTo(jMinusOneElement) < 0) {
                unsorted[j - 1] = jthElement;
                unsorted[j] = jMinusOneElement;
            } else {
                break;
            }
        }
    }
}
```

> **Notice**

> 直接插入排序是稳定的排序。关于各种算法的稳定性分析可以参考 [常用排序算法稳定性分析](StabilityAnalysis.md)
> 
> 文件初态不同时，直接插入排序所耗费的时间有很大差异。若文件初态为正序，则每个待插入的记录只需要比较一次就能够找到合适的位置插入，故算法的时间复杂度为`O(n)`，这时最好的情况。若初态为反序，则第`i`个待插入记录需要比较`i+1`次才能找到合适位置插入，故时间复杂度为`O(n2)`，这时最坏的情况。
> 
> 直接插入排序的平均时间复杂度为O(n2)。