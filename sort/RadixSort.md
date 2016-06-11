# 基数排序

**基本思想**：将所有待比较数值（正整数）统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后,数列就变成一个有序序列。

![基数排序](https://i.imgur.com/qdX4dbf.png)

**Java实现1**

```java
//稳定
public class RadixSort {
    public static void main(String[] args) {
        int[] a={49,38,65,97,176,213,227,49,78,34,12,164,11,18,1};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //基数排序
        sort(a);
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

    private static void sort(int[] array) {
        //找到最大数，确定要排序几趟
        int max = 0;
        for (int i = 0; i < array.length; i++) {
            if(max<array[i]){
                max = array[i];
            }
        }
        //判断位数
        int times = 0;
        while(max>0){
            max = max/10;
            times++;
        }
        //建立十个队列
        List<ArrayList> queue = new ArrayList<ArrayList>();
        for (int i = 0; i < 10; i++) {
            ArrayList queue1 = new ArrayList();
            queue.add(queue1);
        }
        //进行times次分配和收集
        for (int i = 0; i < times; i++) {
            //分配
            for (int j = 0; j < array.length; j++) {
                int x = array[j]%(int)Math.pow(10, i+1)/(int)Math.pow(10, i);
                ArrayList queue2 = queue.get(x);
                queue2.add(array[j]);
                queue.set(x,queue2);
            }
            //收集
            int count = 0;
            for (int j = 0; j < 10; j++) {
                while(queue.get(j).size()>0){
                    ArrayList<Integer> queue3 = queue.get(j);
                    array[count] = queue3.get(0);
                    queue3.remove(0);
                    count++;
                }
            }
        }
    }
}
```

**Java实现2**

```java
public class RadixSort {

    private static final int NUMBER_OF_BUCKETS = 10; // 10 for base 10 numbers

    private RadixSort() { }

    public static Integer[] sort(Integer[] unsorted) {
        int[][] buckets = new int[NUMBER_OF_BUCKETS][10];
        for (int i = 0; i < NUMBER_OF_BUCKETS; i++)
            buckets[i][0] = 1; // Size is one since the size is stored in first element
        int numberOfDigits = getMaxNumberOfDigits(unsorted); // Max number of digits
        int divisor = 1;
        for (int n = 0; n < numberOfDigits; n++) {
            int digit = 0;
            for (int d : unsorted) {
                digit = getDigit(d, divisor);
                buckets[digit] = add(d, buckets[digit]);
            }
            int index = 0;
            for (int i = 0; i < NUMBER_OF_BUCKETS; i++) {
                int[] bucket = buckets[i];
                int size = bucket[0];
                for (int j = 1; j < size; j++) {
                    unsorted[index++] = bucket[j];
                }
                buckets[i][0] = 1; // reset the size
            }
            divisor *= 10;
        }
        return unsorted;
    }

    private static int getMaxNumberOfDigits(Integer[] unsorted) {
        int max = Integer.MIN_VALUE;
        int temp = 0;
        for (int i : unsorted) {
            temp = (int) Math.log10(i) + 1;
            if (temp > max)
                max = temp;
        }
        return max;
    }

    private static int getDigit(int integer, int divisor) {
        return (integer / divisor) % 10;
    }

    private static int[] add(int integer, int[] bucket) {
        int size = bucket[0]; // size is stored in first element
        int length = bucket.length;
        int[] result = bucket;
        if (size >= length) {
            result = Arrays.copyOf(result, ((length * 3) / 2) + 1);
        }
        result[size] = integer;
        result[0] = ++size;
        return result;
    }
}
```

> **Notice**
> 
> 基数排序是稳定的排序算法，基数排序的时间复杂度为O(d(n+r)),d为位数，r为基数。