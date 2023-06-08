## 数据结构Day1

------



### 冒泡排序算法优化：



> **改良后：最好时间复杂度：0（n）;最坏时间与平均时间复杂度均为0（n平方）；具体为二分之n方减n**
>
> **空间复杂度为0（1）  稳定性：稳定排序**

```c++
int* bubbleSort(int* arr,int size) {

	for (int i = 0; i < size - 1; i++) {
		int target = 1;
		for (int j = 0; j < size - i - 1; j++) {
			if (arr[j] > arr[j + 1]) {
				int tmp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = tmp;
				target = 0;
			}
		}
		if (target == 1) {
			break;//说明本次之后，程序已经排序好，无需继续遍历排序
		}
	}
	return arr;
}
```







### 插入排序算法：



> **最好时间复杂度：0（n）;最坏时间与平均时间复杂度均为0（n平方）；具体为两者平均**
>
> **空间复杂度为0（1）  稳定性：稳定排序**





#### **不带哨兵的代码**

```c++
int* insertSort(int* arr, int size) 
{
	int i, j;

	for (i = 0; i < size; i++) {

		if (arr[i] < arr[i - 1]) 
		{
			int tmp = arr[i];
			for (j = i - 1; j >= 0 && arr[j] > tmp; j--)//若没有哨兵，则必须要进行判断  j>=0
			{
				arr[j + 1] = arr[j];
			}
			arr[j] = tmp;
		}

		else
		{
			continue;
		}
	}

	return arr;
}//不带哨兵情况下的插入排序
```

#### 带哨兵的代码:



### 二分查找算法



> **时间复杂度为：log2（n）**
>
> **最高时间复杂度：	0（1），最差时间复杂度：0(log2n)**
>
> **空间复杂度为0（1）**



> **前提：必须基于顺序表和有序数组**==（例如链表就不可以）==

```c++
int findDivided(int* arr, int size,int target) 
{
	int low = 0, high = size - 1;
	while (low <= high) {
		int mid = (low + high) / 2;
		if (target > arr[mid])
		{
			low = mid + 1;
		}
		else if(target<arr[mid])
		{
			high = mid - 1;
		}
		else if (target == arr[mid])
		{
			return mid;//直接返回mid的位置
		}
	}
	return -1;//出现high小于low的情况，说明数组没有元素
}//二分查找算法（数组必须是有序的）
```

### 分块查找算法

> **==核心思想：块内无序，块间有序==**

> **所以：快间可以用二分查找，块内可以用顺序查找**

<img src="C:/Users/M/Desktop/%E6%96%87%E7%AB%A04%E7%94%A8/100.jpg" alt="100" style="zoom:67%;" />
