# 归并排序

**基本思想**:归并（Merge）排序法是将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。然后再把有序子序列合并为整体有序序列。

![归并排序](https://i.imgur.com/XidCMJ0.png)

**Java实现1**

```java
//稳定
public class 归并排序 {
    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //归并排序
        mergeSort(a,0,a.length-1);
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

    private static void mergeSort(int[] a, int left, int right) {
        if(left<right){
            int middle = (left+right)/2;
            //对左边进行递归
            mergeSort(a, left, middle);
            //对右边进行递归
            mergeSort(a, middle+1, right);
            //合并
            merge(a,left,middle,right);
        }
    }

    private static void merge(int[] a, int left, int middle, int right) {
        int[] tmpArr = new int[a.length];
        int mid = middle+1; //右边的起始位置
        int tmp = left;
        int third = left;
        while(left<=middle && mid<=right){
            //从两个数组中选取较小的数放入中间数组
            if(a[left]<=a[mid]){
                tmpArr[third++] = a[left++];
            }else{
                tmpArr[third++] = a[mid++];
            }
        }
        //将剩余的部分放入中间数组
        while(left<=middle){
            tmpArr[third++] = a[left++];
        }
        while(mid<=right){
            tmpArr[third++] = a[mid++];
        }
        //将中间数组复制回原数组
        while(tmp<=right){
            a[tmp] = tmpArr[tmp++];
        }
    }
}
```

**Java实现2**

```java
/**  
 * 归并排序<br/>  
 * <ul>  
 * <li>申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列</li>  
 * <li>设定两个指针，最初位置分别为两个已经排序序列的起始位置</li>  
 * <li>比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置</li>  
 * <li>重复步骤3直到某一指针达到序列尾</li>  
 * <li>将另一序列剩下的所有元素直接复制到合并序列尾</li>  
 * </ul>  
 *   
 * @param numbers  
 */  
public static void mergeSort(int[] numbers, int left, int right) {   
    int t = 1;// 每组元素个数   
    int size = right - left + 1;   
    while (t < size) {   
        int s = t;// 本次循环每组元素个数   
        t = 2 * s;   
        int i = left;   
        while (i + (t - 1) < size) {   
            merge(numbers, i, i + (s - 1), i + (t - 1));   
            i += t;   
        }   
        if (i + (s - 1) < right)   
            merge(numbers, i, i + (s - 1), right);   
    }   
}   

/**  
 * 归并算法实现  
 *   
 * @param data  
 * @param p  
 * @param q  
 * @param r  
 */  
private static void merge(int[] data, int p, int q, int r) {   
    int[] B = new int[data.length];   
    int s = p;   
    int t = q + 1;   
    int k = p;   
    while (s <= q && t <= r) {   
        if (data[s] <= data[t]) {   
            B[k] = data[s];   
            s++;   
        } else {   
            B[k] = data[t];   
            t++;   
        }   
        k++;   
    }   
    if (s == q + 1)   
        B[k++] = data[t++];   
    else  
        B[k++] = data[s++];   
    for (int i = p; i <= r; i++)   
        data[i] = B[i];   
}
```

**Java实现3**

```java
@SuppressWarnings("unchecked")
public class MergeSort<T extends Comparable<T>> {

    public static enum SPACE_TYPE { IN_PLACE, NOT_IN_PLACE }

    private MergeSort() { }

    public static <T extends Comparable<T>> T[] sort(SPACE_TYPE type, T[] unsorted) {
        sort(type, 0, unsorted.length, unsorted);
        return unsorted;
    }

    private static <T extends Comparable<T>> void sort(SPACE_TYPE type, int start, int length, T[] unsorted) {
        if (length > 2) {
            int aLength = (int) Math.floor(length / 2);
            int bLength = length - aLength;
            sort(type, start, aLength, unsorted);
            sort(type, start + aLength, bLength, unsorted);
            if (type == SPACE_TYPE.IN_PLACE)
                mergeInPlace(start, aLength, start + aLength, bLength, unsorted);
            else
                mergeWithExtraStorage(start, aLength, start + aLength, bLength, unsorted);
        } else if (length == 2) {
            T e = unsorted[start + 1];
            if (e.compareTo(unsorted[start]) < 0) {
                unsorted[start + 1] = unsorted[start];
                unsorted[start] = e;
            }
        }
    }

    private static <T extends Comparable<T>> void mergeInPlace(int aStart, int aLength, int bStart, int bLength, T[] unsorted) {
        int i = aStart;
        int j = bStart;
        int aSize = aStart + aLength;
        int bSize = bStart + bLength;
        while (i < aSize && j < bSize) {
            T a = unsorted[i];
            T b = unsorted[j];
            if (b.compareTo(a) < 0) {
                // Shift everything to the right one spot
                System.arraycopy(unsorted, i, unsorted, i+1, j-i);
                unsorted[i] = b;
                i++;
                j++;
                aSize++;
            } else {
                i++;
            }
        }
    }

    private static <T extends Comparable<T>> void mergeWithExtraStorage(int aStart, int aLength, int bStart, int bLength, T[] unsorted) {
        int count = 0;
        T[] output = (T[]) new Comparable[aLength + bLength];
        int i = aStart;
        int j = bStart;
        int aSize = aStart + aLength;
        int bSize = bStart + bLength;
        while (i < aSize || j < bSize) {
            T a = null;
            if (i < aSize) {
                a = unsorted[i];
            }
            T b = null;
            if (j < bSize) {
                b = unsorted[j];
            }
            if (a != null && b == null) {
                output[count++] = a;
                i++;
            } else if (b != null && a == null) {
                output[count++] = b;
                j++;
            } else if (b != null && b.compareTo(a) <= 0) {
                output[count++] = b;
                j++;
            } else {
                output[count++] = a;
                i++;
            }
        }
        int x = 0;
        int size = aStart + aLength + bLength;
        for (int y = aStart; y < size; y++) {
            unsorted[y] = output[x++];
        }
    }
}
```

> **Notice**
> 
> 归并排序是稳定的排序方法。

- 归并排序的时间复杂度为O(nlogn)。
- 速度仅次于快速排序，为稳定排序算法，一般用于对总体无序，但是各子项相对有序的数列。