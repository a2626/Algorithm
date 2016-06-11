# 如何用java实现一个ArrayList结构

`List` 接口

```java
public interface IList<T> {

    /**
     * 添加元素
     */
    public boolean add(T value);

    /**
     * 删除元素
     */
    public boolean remove(T value);

    /**
     * 清空列表
     */
    public void clear();

    /**
     * 判断是否存在某个元素
     */
    public boolean contains(T value);

    /**
     * 返回列表元素的总个数
     */
    public int size();
}
```

**接口实现代码**

```java
import java.util.Arrays;

public class ArrayList<T> implements IList<T> {

	// 默认加载因子
	private static final int DEFAULT_CAPACITY = 10;
	
	private int size = 0;
	
	@SuppressWarnings("unchecked")
	private T[] array = (T[]) new Object[DEFAULT_CAPACITY];

	@Override
	public boolean add(T value) {
		return add(size, value);
	}

	/**
	 * 添加一个元素到指定位置
	 */
	public boolean add(int index, T value) {
		if (size >= array.length)
			grow();
		if (index == size) {
			array[size++] = value;
		} else {
			// 数组扩容
			System.arraycopy(array, index, array, index + 1, size - index);
			array[index] = value;
		}
		return true;
	}

	/**
	 * 移除元素
	 */
	@Override
	public boolean remove(T value) {
		for (int i = 0; i < size; i++) {
			T obj = array[i];
			if (obj.equals(value)) {
				if (remove(i) != null)
					return true;
				return false;
			}
		}
		return false;
	}

	/**
	 * 根据索引移除元素
	 */
	public T remove(int index) {
		if (index < 0 || index >= size)
			return null;

		T t = array[index];
		if (index != --size) {
			// 重新计算数组容量
			System.arraycopy(array, index + 1, array, index, size - index);
		}
		array[size] = null;
		
		int shrinkSize = array.length >> 1;
		if (shrinkSize >= DEFAULT_CAPACITY && size < shrinkSize)
			shrink();

		return t;
	}

	// 数组按50%增长
	private void grow() {
		int growSize = size + (size << 1);
		array = Arrays.copyOf(array, growSize);
	}

	// 数组按50%收缩
	private void shrink() {
		int shrinkSize = array.length >> 1;
		array = Arrays.copyOf(array, shrinkSize);
	}

	/**
	 * 设置某个位置的元素
	 */
	public T set(int index, T value) {
		if (index < 0 || index >= size)
			return null;
		T t = array[index];
		array[index] = value;
		return t;
	}

	/**
	 * 根据索引获取元素
	 */
	public T get(int index) {
		if (index < 0 || index >= size)
			return null;
		return array[index];
	}

	/**
	 * 清空List
	 */
	@Override
	public void clear() {
		size = 0;
	}
	
//	@SuppressWarnings("unchecked")
//    public void ensureCapacity(int newCapacity) {
//        if( newCapacity < this.size )
//            return;
//        
//        T[] old = array;
//        array = (T[]) new Object[newCapacity];
//        for( int i = 0; i < size( ); i++ )
//        	array[i] = old[i];
//    }
	
	/**
	 * 判断是否存在某个元素
	 */
	@Override
	public boolean contains(T value) {
		for (int i = 0; i < size; i++) {
			T obj = array[i];
			if (obj.equals(value))
				return true;
		}
		return false;
	}

	/**
	 * 返回元素个数
	 */
	@Override
	public int size() {
		return size;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		for (int i = 0; i < size; i++) {
			builder.append(array[i]).append(", ");
		}
		return builder.toString();
	}
}
```

**其他实现：** [A simple ArrayList class in Java](http://codereview.stackexchange.com/questions/16371/a-simple-arraylist-class-in-java)
