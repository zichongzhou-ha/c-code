### 老规矩：冒泡先摆上
```
#include<stdio.h>
void Swap(int *a,int *b)
{
	int tmp = *a;
	*a = *b;
	*b = tmp;
}
int main()
{
	int arr[10] = { 2,43,1,2,4,6,45,3,6,3 };

	int n = 10;
	for (int i = 0; i < n-1; i++)
	{
		for (int j = 0; j < n-i-1; j++)
		{
			if (arr[j] > arr[j + 1])Swap(&arr[j], &arr[j + 1]);
		}
		
	}

	for (int i = 0; i < n; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}
```
### qsort的使用
- qsort一共有四个参数：qsort(void*arr(也就是这样才能实现多种类型数组的排序)，数组个数，数组元素的大小，比较方法(函数指针))
```
#include<stdio.h>
#include<algorithm>     //注意头文件包含
int cmp_int(const void* e1, const void* e2)
{
	return *(int*)e1 - *(int*)e2;		//规则e1>e2 返回>0--》真----》要交换
}
int main()
{
	int arr[10] = { 2,43,1,2,4,6,45,3,6,3 };
	int n = sizeof(arr) / sizeof(arr[0]);
	qsort(arr, n, sizeof(arr[0]), cmp_int);

	for (int i = 0; i < n; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}
```
- qsort函数比较好用的主要体现在数组元素是struct时，这里就不赘述了

### 模仿qsort实现一个冒泡排序的通用算法
```
#include<stdio.h>
#include<algorithm>
int cmp_int(const void* e1, const void* e2)
{
	return *(int*)e1 - *(int*)e2;		
}

void  Swap(char* buf1, char* buf2, int width)
{
	int i = 0;
	for (int i = 0; i < width; i++)
	{
		char tmp = *buf1;
		*buf1 = *buf2;
		*buf2 = tmp;
		buf1++;
		buf2++;
	}
}
void bubble_sort(void* base, int sz, int width, int (*cmp)(const void* e1, const void* e2))
{
	for (int i = 0; i < sz - 1; i++)
	{
		for (int j = 0; j < sz- i - 1; j++)
		{
			if (cmp((char*)base+j*width, (char*)base + (j+1)* width) > 0)		//最核心的一步
			{
				Swap((char*)base + j * width, (char*)base + (j + 1) * width, width);		//交换

				//思考：为啥有的第三个参数：交换是以字节为单位的
			}
		}

	}
}
int main()
{
	int arr[10] = { 2,43,1,2,4,6,45,3,6,3 };
	int n = sizeof(arr) / sizeof(arr[0]);
	bubble_sort(arr, n, sizeof(arr[0]), cmp_int);

	for (int i = 0; i < n; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}
``` 