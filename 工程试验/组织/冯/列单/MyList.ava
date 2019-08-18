package org.feng.list;

import java.util.Arrays;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Objects;
import java.util.function.Consumer;
import java.util.function.Predicate;
/**
 * 自定义容器类：</br>
 * 存储规则：不能存储null值（自定义时，默认的null不可避免），不重复，线程不安全。</br>
 * 其他与ArrayList类似。</br>
 * 需要指定泛型</br>
 * </br>
 * 实现{@linkplain Iterable}是为了jdk8中的foreach遍历
 * @author Feng
 * 2019年8月15日下午1:05:05
 * @param <E> 任意类型
 */
public final class MyList<E> implements Iterable<E>{

	/**数据：数组*/
	private E[] array;
	
	/**容器的真实大小：元素个数*/
	private int size;
	
	/**默认容积：10*/
	private static final int DEFAULT_CAPACITY = 10;
	/**容积*/
	private int capacity;
	
	/**最大容量*/
	private static final int MAX_CAPACITY = Integer.MAX_VALUE;
	
	/**构造函数：使用默认容量*/
	public MyList() {
		this(DEFAULT_CAPACITY);
	}
	
	/**构造函数：传入指定容量*/
	@SuppressWarnings("unchecked")
	public MyList(int capacity) {
		size = 0;
		this.capacity = capacity;
		array = (E[]) new Object[capacity];
	}
	
	
	/*维护容量*/
	@SuppressWarnings("unchecked")
	private void reCapacity(int capacity) {
		if(capacity > MAX_CAPACITY) {
			capacity = MAX_CAPACITY;
		}
		
		E[] newData = (E[])new Object[capacity];
		// 将原数组内容放到新的数组中
		for (int i = 0; i < size; i++) {
			newData[i] = array[i];
		}
		array = newData;
		this.capacity = capacity;
	}
	

	/**增加元素*/
	public boolean add(E e) {
		throwExceptionIfElementExit(e);
		Objects.requireNonNull(e, "[add(E e)]传入的参数e是null");
		// 维护容量
		if(size == array.length) {
			reCapacity(size << 1);
		}
		array[size ++] = e;
		return true;
	}
	
	/**通过下标：增加元素*/
	private boolean add(int index, E e) {
		throwExceptionIfElementExit(e);
		
		throwMyException("index[" + index + "]不存在！", index);
		Objects.requireNonNull(e, "[add(int index, E e)]传入的参数e是null");
		// 维护容量
		if(index == array.length) {
			reCapacity(array.length << 1);
		}
		// 存值
		array[index] = e;
		size ++;
		return true;
	}
	
	/**增加第一个元素*/
	public boolean addFirst(E e) {
		return add(0, e);
	}
	
	/*增加最后一个元素*/
	@SuppressWarnings("unused")
	private boolean addLast(E e) {
		return add(array.length - 1, e);
	}
	
	/**通过下标：得到指定元素*/
	public E get(int index) {
		throwMyException("index[" + index + "]不存在！", index);
		return array[index];
	}
	
	/**按照下标：删除指定元素，返回删除的元素*/
	public E remove(int index) {
		throwMyException("index[" + index + "]不存在！", index);

		E result = array[index];
		for (int i = index + 1; i < size; i++) {
			array[i - 1] = array[i];
		}
		size --;
		// 维护容量
		if(size < array.length >> 1) {
			reCapacity(array.length >> 1);
		}
		
		return result;
	}
	
	/**
	 * 删除指定元素：删除第一次出现的该元素。</br>
	 * 原因：类{@link MyList}的存储的内容可以是重复的。
	 * @param e
	 * @return 被删除的元素
	 */
	public E remove(E e) {
		int index = indexOf(e);
		if(index == -1 || index > size) {
			throw new NoSuchElementException("元素" + e + "不存在！");
		}
		return remove(index);
	}
	
	/**
	 * 使用函数式接口！</br>
	 * 实现过滤式删除符合条件的元素。</br>
	 * @param predicate 包含单个返回值，单个参数的抽象方法的接口
	 * @return 当删除元素时，返回true；否则返回false
	 */
	@SuppressWarnings("unchecked")
	public boolean removeIf(Predicate<E> predicate) {
		Objects.requireNonNull(predicate);
		
		boolean remove = false;
		int count = 0;// 计数，算出要删除的个数
		for (int i = 0; i < size; i++) {
			if(predicate.test(array[i])) {
				array[i] = null;// 先置null
				count ++;
				remove = true;
			}
		}
		
		// 有删除元素时
		E[] e = null;
		if(remove) {
			int index = 0;// 新的数组的初始位置
			e = (E[]) new Object[size - count];
			for (int i = 0; i < size; i++) {
				// 将null的去掉，其他复制到新的数组中
				if(array[i] != null) {
					e[index ++] = array[i];
				}
			}
		}
		// 更新数组
		if(e != null) {
			array = e;
		}
		// 更新大小
		size -= count;
		return remove;
	}
	
	
	
	
	/***
	 * 找到指定元素第一次出现的位置的下标
	 * @param e
	 * @return 找不到时返回-1
	 */
	public int indexOf(E e) {
		Objects.requireNonNull(e, "[indexOf(E e)]传入的元素是 null");
		
		for (int i = 0; i < size; i++) {
			if(e.equals(array[i])) {
				return i;
			}
		}
		// 找不到该元素时，返回-1
		return -1;
	}
	
	/**得到真实容量*/
	public int getCapacity() {
		return capacity;
	}
	
	/**
	 * 清空容器
	 */
	public void clear() {
		for (int i = 0; i < size; i++) {
			array[i] = null;
		}
		size = 0;
	}
	
	/**
	 * 判断是否包含传入的元素
	 * @param e
	 * @return 包含返回true，否则返回false
	 */
	public boolean contains(E e) {
		return indexOf(e) >= 0;
	}
	
	/**
	 * 将容器转换到数组
	 * @return
	 */
	public Object[] toObjectArray() {
		return toArray(size);
	}
	
	/**
	 * 通过容器转换到指定类型的数组
	 * @return
	 */
	@SuppressWarnings("unchecked")
	public E[] toArray(E[] e) {
		Objects.requireNonNull(e);
		
		e = (E[]) java.lang.reflect.Array.
			newInstance(e.getClass().getComponentType(), size);
		// 赋值到新的数组
		for (int i = 0; i < e.length; i++) {
			e[i] = array[i];
		}
		return e;
	}
	
	
	
	
	
	
	
	
	/***
	 * 获得容器大小
	 * @return 容器中元素个数
	 */
	public int getSize() {
		return size;
	}
	
	
	/**
	 * 将容器转换位指定长度的数组
	 * @param length
	 * @return
	 */
	private Object[] toArray(int length) {
		// 长度不足时：
		if(length < size) {
			length = size;
		}
		Object[] e = new Object[length];
		// 复制数组
		for (int i = 0; i < size; i++) {
			e[i] = array[i];
		}
		return e;
	}
	
	/**
	 * 按照指定索引：更改容器中的元素
	 * @param index 指定索引
	 * @param newElement 新的元素
	 * @return 旧的元素
	 */
	public E set(int index, E newElement) {
		Objects.requireNonNull(newElement, "[set(int index, E newElement)]中新元素不能为null");
		throwMyException("index[" + index + "]不存在！", index);
		
		// 取出旧值，存入新值
		E oldElement = array[index];
		array[index] = newElement;
		return oldElement;
	}
	
	/**
	 * 元素设置：以旧换新。
	 * @param oldElement
	 * @param newElement
	 * @return {@code oldElement} 旧元素
	 */
	public E set(E oldElement, E newElement) {
		Objects.requireNonNull(oldElement, "[set(E oleElement, E newElement)]中旧元素不能为null");
		Objects.requireNonNull(newElement, "[set(E oleElement, E newElement)]中新元素不能为null");
		
		// 判断索引是否存在
		int index = indexOf(oldElement);
		throwMyException("index[" + index + "]不存在！", index);
		
		return set(index, newElement);
	}
	
	/**
	 * 倒序查询元素{@code e}第一次出现的位置
	 * @param e
	 * @return 找不到时，返回-1；否则返回该元素在容器中的位置(索引)
	 */
	public int lastIndexOf(E e) {
		Objects.requireNonNull(e, "lastIndexOf(E e)中的元素e不能为空");
		// 倒序
		for (int i = size - 1; i >= 0; i++) {
			if(e.equals(array[i])) {
				return i;
			}
		}
		// 找不到时
		return -1;
	}
	
	/**
	 * 增加一个容器到本容器中。
	 * @param list
	 * @return 
	 */
	@SuppressWarnings("unchecked")
	public boolean addAll(MyList<? extends E> list) {
		Objects.requireNonNull(list, "addAll(List<? extends E> list)传入的集合为null");
		Object[] arr = list.toObjectArray();// 目标数组
		return addArray((E[])arr);
	}
	
	/**
	 * 增加一个数组到本容器中
	 * @param arr
	 * @return
	 */
	@SuppressWarnings("unchecked")
	public boolean addArray(E[] arr) {
		Objects.requireNonNull(arr, "addArray(E[] arr)中arr不能为空！");
		
		int length = arr.length;
		int newLen = size + length;
		// 将新的数组的长度翻一倍
		Object[] result = new Object[newLen << 1];
		// 复制原来的数组
		for (int i = 0; i < size; i++) {
			result[i] = array[i];
		}
		
		// 复制集合到新的数组中，接着原来数组的后边复制
		for(int i = size; i < newLen; i++) {
			result[i] = arr[i - size];
		}
		array = (E[]) result;// 更新数组
		size += length;// 更新大小
		return true;
	}
	

	
	
	
	
	
	
	@Override
	public void forEach(Consumer<? super E> action) {
		Objects.requireNonNull(action);
		Iterable.super.forEach(action);
	}
	
	

	/*抛出自己的异常：对index下标进行判断*/
	private void throwMyException(String message, int index) {
		if(index < 0 || index >= size) {
			throw new IllegalArgumentException(message);
		}
	}
	
	/*当元素存在容器中时，抛出异常*/
	private void throwExceptionIfElementExit(E e) {
		// 当容器中存在该元素时
		if(contains(e)) {
			throw new IllegalArgumentException("元素[" + e + "]已经存在了！");
		}
	}
	
	@Override
	public int hashCode() {
		int result = Arrays.deepHashCode(this.toObjectArray());
		return result;
	}
	
	/**
	 * 重写{@link Object}中的{@code euqals} 方法：</br>
	 * 比较地址、所属类名。
	 * 特殊地，在比较时，比较了数组内容和真实长度{@code size}</br>
	 * 另外，其他地方当比较的参数{@code obj}是{@code null}时，返回{@code false}</br>
	 */
	@SuppressWarnings("unchecked")
	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		MyList<E> other = (MyList<E>) obj;
		if (!Arrays.deepEquals(this.toObjectArray(), other.toObjectArray()))
			return false;
		if (size != other.size)
			return false;
		return true;
	}
	
	
	@Override
	public String toString() {
		if(size == 0) {// 无元素
			return "MyList: [null]";
		}
		// 有元素时
		StringBuilder sb = new StringBuilder("MyList: [");
		for (int i = 0; i < size - 1; i++) {
			sb.append(array[i]).append(", ");
		}
		
		return sb.append(array[size - 1]).
				append("]").
				toString();
	}
	

	
	/**
	 * 迭代器：用来迭代容器中的元素
	 * @return
	 */
	public Iterator<E> iterator() {
		return new MyIterator();
	}
	
	/*****
	 * 此处建立一个内部类：实现Iterator接口。
	 */
	private class MyIterator implements Iterator<E>{
		// 游标
		int course = 0;
		
		@Override
		public boolean hasNext() {
			return course < size;
		}

		@Override
		public E next() {
			return get(course ++);
		}
		
	}



	
	

}
