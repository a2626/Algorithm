# 二分法插入排序（按二分法找到合适位置插入）

**基本思想**：二分法插入排序的思想和直接插入一样，只是找合适的插入位置的方式不同，这里是按二分法找到合适的位置，可以减少比较的次数。

> 
> 在插入第i个元素时，对前面的0～i-1元素进行折半，先跟他们 
中间的那个元素比，如果小，则对前半再进行折半，否则对后半 
进行折半，直到left>right，然后再把第i个元素前1位与目标位置之间 
的所有元素后移，再把第i个元素放在目标位置上。
> 
> 二分法排序最重要的一个步骤就是查找要插入元素的位置，也就是要在哪一个位置上放我们要准备排序的这个元素。
当我们查找到位置以后就很好说了，和插入排序一样，将这个位置以后的所有元素都向后移动一位。这样就实现了二分法排序。
　　然后是怎么查找着一个位置呢，就是不断的比较已排序的序列中的中间元素和要排序元素，如果大于的话，说明这个要排
序的元素在已排序序列中点之前的序列。

![二分法插入排序](https://i.imgur.com/0FPAUAM.jpg)

**Java实现1**

```java
public class DichotomySort {
    public static void main(String[] args) {
        int[] a={49,38,65,97,176,213,227,49,78,34,12,164,11,18,1};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //二分插入排序
        sort(a);
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

    private static void sort(int[] a) {
        for (int i = 0; i < a.length; i++) {
            int temp = a[i];
            int left = 0;
            int right = i-1;
            int mid = 0;
            while(left<=right){
                mid = (left+right)/2;
                if(temp<a[mid]){
                    right = mid-1;
                }else{
                    left = mid+1;
                }
            }
            for (int j = i-1; j >= left; j--) {
                a[j+1] = a[j];
            }
            if(left != i){
                a[left] = temp;
            }
        }
    }
}
```

**Java实现2**

```java
public static void DichotomySort(int[] array) {
	for (int i = 0; i < array.length; i++) {
		int start, end, mid;
		start = 0;
		end = i - 1;
		mid = 0;
		int temp = array[i];
		while (start <= end) {
			mid = (start + end) / 2;
			if (array[mid] > temp){// 要排序元素在已经排过序的数组左边
				end = mid - 1;
			} else {
				start = mid + 1;
			}
		}
		for (int j = i - 1; j > end; j--) {// 找到了要插入的位置，然后将这个位置以后的所有元素向后移动
			array[j + 1] = array[j];
		}
		array[end + 1] = temp;
	}
}
```

> **Notice**

> 当然，二分法插入排序也是稳定的。
> 
> 二分插入排序的比较次数与待排序记录的初始状态无关，仅依赖于记录的个数。当n较大时，比直接插入排序的最大比较次数少得多。但大于直接插入排序的最小比较次数。算法的移动次数与直接插入排序算法的相同，最坏的情况为n2/2，最好的情况为n，平均移动次数为O(n2)。

