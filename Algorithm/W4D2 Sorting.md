# Sorting

![Untitled](https://user-images.githubusercontent.com/39071638/210305867-668f8648-7aee-4f1e-a4a4-35dd9902ced9.png)
정렬 알고리즘은 다음과 같이 나눠볼 수 있다.

- 단순하지만 비효율적인 방법 : 선택 정렬, 삽입 정렬, 버블 정렬
- 복잡하지만 조금 더 효율적인 방법 : 퀵 정렬, 힙 정렬, 합병 정렬

# Insertion Sort (삽입 정렬)

삽입 정렬이라고 불리는 알고리즘이다. 2번째 원소부터 시작하여 해당 원소의 앞 원소들과 비교하여 삽입할 위치를 지정한 후, 원소를 뒤로 옮기고, 지정된 자리에서 데이터를 삽입하여 정렬하는 알고리즘이다.

![Untitled 1](https://user-images.githubusercontent.com/39071638/210305879-2d4e0f61-6ac5-45dd-bd86-cbdfaa710b34.png)
**로직**

1. 정렬은 2번째 위치(index)의 값을 standard에 저장한다
2. standard와 이전에 있는 원소들과 비교하여 자리를 바꾸며 삽입해 나간다.
3. 1번으로 돌아가서 다음 위치(index)의 값을 standard에 저장하고 이 과정을 반복한다.

**Code**

```java
private static void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) { // 1
            int standard = arr[i];
            int index = i - 1;

            while ((0 <= index) && standard < arr[index]) {//2
                arr[index + 1] = arr[index];
                index--;
            }
            arr[index + 1] = standard; // 3

            print(arr, i);
        }
    }

    private static void print(int[] arr, int step) {
        System.out.print(step + "단계 : ");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }

        System.out.println();
    }
// 단계별 결과.
1단계 : 6 7 2 4 3 9 1
2단계 : 2 6 7 4 3 9 1
3단계 : 2 4 6 7 3 9 1
4단계 : 2 3 4 6 7 9 1
5단계 : 2 3 4 6 7 9 1
6단계 : 1 2 3 4 6 7 9
```

**장점**

- 알고리즘이 단순하다.
- 대부분의 원소가 이미 정렬되어 있는 경우, 매우 효율적일 수 있다.
- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다.
- 선택 정렬이나 버블 정렬에 비하여 상대적으로 빠르다.

**단점**

- 비교적 많은 수들의 이동을 포함한다.
- 비교할 수가 많고 크기가 클 경우에 적합하지 않다.(배열의 길이가 길어질수록 비효율적)
- 평균과 최악의 시간 복잡도가 O(N^2)이므로 비효율적이다.

<aside>
⏰ Insertion Sort’s 
Time Complexity : 
**O($n*2$)** (Worst, Average) / **O(n)** Best
****Space Complexity : **O(n)**

</aside>

# Selection Sort (선택 정렬)

선택 정렬이라고 불리는 정렬 방법이고, 주어진 배열 중에 최솟값을 찾아 정렬되지 않은 배열의 맨앞의 값과 자리를 바꾸어 나가는 정렬 방법이다 가장 기초적이고 원시적인 알고리즘

**Logic**

1. 정렬 되지 않은 인덱스의 맨 앞에서 부터, 이를 포함한 그 이후의 배열값 부터 가장 작은 값을 찾아간다.
2. 가장 작은 값을 찾으면, 그 값을 현재 인덱스의 값과 바꿔준다.
3. 다음 인덱스에서 위 과정을 반복해준다.

![Untitled 2](https://user-images.githubusercontent.com/39071638/210305895-281019b7-1d30-4807-a6f4-d51ece287cb2.png)
> **28 13 23 25 19** : 초기배열
13 **28 23 25 19** : 최솟값 13을 맨 앞의 수 28과 자리 바꾸기13 19 **23 25 28** : 다음 최솟값 19를 맨 앞의 수 28과 자리 바꾸기
13 19 23 **25 28** : 다음 최솟값은 23이니깐 자리 그대로!
13 19 23 25 **28** : 다음 최솟값은 25이니깐 자리 그대로!
13 19 23 25 28 : 정렬 완료!
> 

**Code** 

Java

```java
void selectionSort(int[] list) {
    int indexMin, temp;

    for (int i = 0; i < list.length - 1; i++) {
        indexMin = i;
        for (int j = i + 1; j < list.length; j++) {
            if (list[j] < list[indexMin]) {
                indexMin = j;
            }
        }
        temp = list[indexMin];
        list[indexMin] = list[i];
        list[i] = temp;
    }
}
```

 

- 장점
    - 자료 이동 횟수가 미리 결정된다.
- 단점
    - 안정성을 만족하지 않는다.
    즉, 값이 같은 레코드가 있는 경우에 상대적인 위치가 변경될 수 있다.

<aside>
⏰ Selection Sort’s 
Time Complexity :  **O($n*2$)** 
Space Complexity : **O(n)**

</aside>

# Bubble Sort (버블 정렬)

- 서로 인접한 두 원소를 검사하여 정렬하는 알고리즘이다.
- 서로 크기를 비교하는데 순서대로 정렬되어있지 않으면 서로 교환한다
- Insertion Sort와 유사하다

**Logic**

1. 1회전에 (1, 2), (2, 3), (3, 4) … (n - 1, n)이런식으로 인접할 두 개씩 비교를 해가면서 정렬 조건에 맞지 않으면 교체한다
2. 각 회전에 끝나면 맨 뒤에는 가장 크거나 작은 원소가 위치하게 되므로 마지막을 제외하고 n-1개의 데이터의 정렬을 재시작한다.

![Untitled 3](https://user-images.githubusercontent.com/39071638/210305907-fb93f1ef-ac24-4375-8eda-2cb5ed8c4589.png)
**Code**

```java
void bubbleSort(int[] arr) {
    int temp = 0;
    for(int i = 0; i < arr.length; i++) {
        for(int j= 1 ; j < arr.length-i; j++) {
            if(arr[j]<arr[j-1]) {
                temp = arr[j-1];
                arr[j-1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    System.out.println(Arrays.toString(arr));
}

```

```java
private static void sort(int[] arr) {
        for (int i = 0; i < arr.length; i++) { // 1
            for (int j = 0; j < arr.length - i - 1; j++) { // 2
                if (arr[j] > arr[j + 1]) { // 3
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }

            System.out.print((i + 1) + "단계 : ");
            print(arr);
        }
    }

private static void print(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

// 단계별 결과.
1단계 : 6 2 4 3 7 1 9 
2단계 : 2 4 3 6 1 7 9 
3단계 : 2 3 4 1 6 7 9 
4단계 : 2 3 1 4 6 7 9 
5단계 : 2 1 3 4 6 7 9 
6단계 : 1 2 3 4 6 7 9 
7단계 : 1 2 3 4 6 7 9
```

**장점**

- 구현이 간단하고 소스코드가 직관적이다.
- 이미 정렬된 데이터를 정렬할 때, 가장 빠르다.
- 정렬하고자 하는 배열 안에서 정렬하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다.
- 안정 정렬이다.

**단점**

- 시간 복잡도가 최악, 최선, 평균 모두 O(N^2)이므로 비효율적이다.
- 다른 정렬에 비해 정렬 속도가 느리다.
- 교환 횟수가 많다.
- 역순배열을 정렬할 때, 가장 느리다.

<aside>
⏰ Bubble Sort’s 
Time Complexity : **O($n^2$)**
Space Complexity : **O(n)**

</aside>

# Quick Sort (퀵 정렬)

Divide & Conquer을 기반으로 하는 정렬

특정한 값(Pivot)을 기준으로 큰 숫자와 작은 숫자를 서로 교환한 뒤에 배열을 반으로 나눈다

```c
int num = 10;
int data[] = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};

void show(){
	for(int i = 0; i < num; i++)
		printf("%d ", data[i]);
}

void quickSort(int* data, int start, int end){
	if(start >= end)
		return;   // 원소가 1개인 경우는 그냥 두기
	int key = start; // 첫번째 원소가 key
	int i = start + i, j = end, tmp;
	while(i <= j){
		while(i <= end && data[i] <= data[key]) i++; // key 보다 큰 값을 만날 때 까지
		while(j > start && data[j] <= data[key]) j--; // key 보다 작은 값을 만날 때 까지
		if(i > j){ // 현재 엇갈린 상태면 키 값을 교체함
			tmp = data[j];
			data[j] = data[key];
			data[key] = tmp;
		}
		else{ // 엇갈리지 않았다면 i, j 서로 교체
			tmp = data[i];
			data[i] = data[j];
			data[j] = tmp;
		}
	}
	quickSort(data, start, j - 1);
	quickSort(data, j + 1, end);
}

quickSort(data, 0, num - 1);
show();
```

<aside>
⏰ Quick Sort’s 
Time Complexity : **O($n^2$)** 
Space Complexity : **O(n)**

</aside>

# Merge Sort (병합 정렬)

병합. 합병 정렬이라고도 불리며 Divide and Conquer 방식으로 구현된다.

속도가 빨라서 Quick Sort와 함께 많이 언급된다. 하지만 Quick Sort와는 다르게 **안정 정렬**에 속한다.

![Untitled 4](https://user-images.githubusercontent.com/39071638/210305923-dc0d5f86-1f40-49a3-822c-e7cc6d65170f.png)
**Code**

```java
/*
    * i : 왼쪽 부분 배열을 관리하는 인덱스
    * j : 오른쪽 부분 배열을 관리하는 인덱스
    * */
    private static void merge(int[] a, int left, int mid, int right) {
        int i, j, k, l;
        i = left;
        j = (mid + 1);
        k = left;

        // 왼쪽 부분 배열과 오른쪽 부분 배열을 비교하면서
        // 각각의 원소 중에서 작은 원소가 sorted 배열에 들어간다.
        // 왼쪽 혹은 오른쪽의 부분 배열 중 하나의 배열이라도 모든 원소가 sorted 배열에 들어간다면
        // 아래의 반복문은 조건을 만족하지 하지 않기 때문에 빠져 나온다.
        // sorted 배열에 들어가지 못한 부분 배열은 아래 쪽에서 채워진다.
        while (i <= mid && j <= right) {
            if (a[i] < a[j]) sorted[k++] = a[i++];
            else sorted[k++] = a[j++];
        }

        // mid < i 인 순간, i는 왼쪽 부분 배열의 인덱스를 관리하므로
        // 즉, 왼쪽 부분 배열이 sorted 에 모두 채워졌음을 의미한다.
        // 따라서 남아 있는 오른쪽 부분 배열의 값을 일괄 복사한다.
        if (mid < i) {
            for (l = j; l <= right; l++) sorted[k++] = a[l];
        } else {
            // mid > i 인 것은 오른쪽 부분 배열이 sorted 배열에 정렬된 상태로 모두 채워졌음을 의미한다.
            // 따라서, 왼쪽 부분 배열은 아직 남아 있는 상태이다.
            // 즉, 남아 있는 왼쪽 부분 배열의 값을 일괄 복사한다.
            for (l = i; l <= mid; l++) sorted[k++] = a[l];
        }

        // sorted 배열에 들어간 부분 배열을 합하여 정렬한 배열은
        // 원본 배열인 a 배열로 다시 복사한다.
        for (l = left; l <= right; l++) a[l] = sorted[l];
    }
```

```java
private static void mergeSort(int[] a, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;

            mergeSort(a, left, mid); // 왼쪽 부분 배열을 나눈다.
            mergeSort(a, mid + 1, right); // 오른쪽 부분 배열을 나눈다.
            merge(a, left, mid, right);
            // 나눈 부분 배열을 합친다.
            // 핵심 로직이다.
        }
    }
```

mergeSort()로 나누어진 왼쪽, 오른쪽의 두 부분 배열을 합치는 과정이 merge()에서 일어난다.

자세한 내용을 말로 하기에는 아직 설명 능력이 부족하여, 코드 상의 주석으로 설명했으니 참고 바란다.

참고로, 합병 정렬은 순차적인 비교로 정렬을 진행하므로, LinkedList의 정렬이 필요할 때, 사용하면 효율적이다.

LinkedList를 퀵 정렬에 사용해 정렬하면 성능이 좋지 않다.

이유는 퀵 정렬은 순차 접근이 아닌 임의 접근이기 때문이다.

LinkedList는 삽입과 삭제 연산에서는 유용하지만, 접근 연산에서는 비효율적이다.

**장점**

- 데이터의 분포에 영향을 덜 받는다. 즉, 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다. -> O(N logN)
- 크기가 큰 레코드를 정렬한 경우, LinkedList를 사용한다면 병합 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법보다 효율적이다.
- 안정 정렬에 속한다.

**단점**

- 레코드를 배열로 구성한다면, 임시 배열이 필요하다.
    - 메모리 낭비를 초래한다.
    - 제자리 정렬이 아니다.
- 레코드의 크기가 큰 경우에는 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래한다.

<aside>
⏰ Merge Sort’s 
Time Complexity : **O($nlog n$)** (Best, Worst, Average)
****Space Complexity : **O(n)**

</aside>

# Heap Sort (힙 정렬)

Heap을 기반으로 한 정렬 방식이다

**정렬 방식**

1. Max Heap을 구성한다
2. 현재 Root에는 가장 큰 값이 존재한다. root의 값을 마지막 요소와 바꾸고, Heap의 크기를 한 줄인다
3. Heap의 크기가 1보다 크면 위의 과정을 반복한다

![Untitled 5](https://user-images.githubusercontent.com/39071638/210305931-063bb13a-a29a-4bc3-81ba-a4d635545787.png)
![Untitled 6](https://user-images.githubusercontent.com/39071638/210305942-c069a444-5cad-4d78-9aeb-55f60cba0858.png)
**Code**

```java
private static void heapSort(int[] arr) {
        int n = arr.length;

  			// max heap 초기화.
        for (int i = (n / 2) - 1; i >= 0; i--) {
            heapify(arr, n, i); // 1
        }

  			// extract 연산.
        for (int i = n - 1; i > 0; i--) {
            swap(arr, 0, i);
            heapify(arr, i, 0);
        }
    }
```

1번째 heapify

- 일반 배열을 힙으로 구성하는 역할을 한다.
- 자식노드로부터 부모 노드를 비교한다.
- (n/2)-1부터 0까지 인덱스를 돌리는 이유는?
    - 부모 노드의 인덱스를 기준으로 왼쪽 자식 노드 : ix2+1, 오른쪽 자식 노드 : ix2+2이기 때문이다.

2번째 heapify

- 요소 하나가 제거된 이후에 다시 최대 힙을 구성하기 위함이다.
- 루트를 기준으로 진행한다.

```java
private static void heapify(int[] arr, int n, int i) {
        int p = i; // 부모 노드.
        int l = i * 2 + 1; // 왼쪽 자식 노드.
        int r = i * 2 + 2; // 오른쪽 자식 노드.

        // 왼쪽 자식 노드와 부모 노드를 비교하여 큰 값을 부모 노드로 올린다.
        if (l < n && arr[p] < arr[l]) p = l;

        // 오른쪽 자식 노드와 부모 노드를 비교하여 큰 값을 부모 노드로 올린다.
        if (r < n && arr[p] < arr[r]) p = r;

        // 부모 노드를 가리키는 p 값이 바뀌면 위치를 교환하고
        // heapify()를 호출하여 과정을 반복한다.
        if (i != p) {
            swap(arr, p, i);
            heapify(arr, n, p);
        }
    }
```

- 다시 최대 힙을 구성할 때까지 부모 노드와 자식 노드를 swap 하며 재귀를 호출한다.
- 퀵 정렬과 합병 정렬의 성능이 좋기 때문에 힙 정렬의 사용 빈도가 높지는 않다.
- 하지만, 힙 자료구조가 많이 활용되고 있으며, 이때 함께 따라오는 개념이 Heap Sort이다.
- Heap Sort가 유용할 때
    - **가장 크거나 가장 작은 값을 구할 때** : 최소 힙 or 최대 힙의 루트 값이기 때문에 한 번의 힙 구성을 통해 구하는 것이 가능하다.
    - **최대 k 만큼 떨어진 요소들을 정렬할 때** : 삽입 정렬보다 더욱 개선된 결과를 얻어낼 수 있다.

```java
package sort;

/**
 * created by victory_woo on 2020/03/14
 */
public class HeapSort {
    public static void main(String[] args) {
        int[] arr = {230, 10, 60, 550, 40, 220, 20};
        heapSort(arr);

        for (int i = 0; i < arr.length; i++) System.out.print(arr[i] + " ");
    }

    private static void heapSort(int[] arr) {
        int n = arr.length;

        // max heap 초기화.
        for (int i = (n / 2) - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }

        for (int i = n - 1; i > 0; i--) {
            swap(arr, 0, i);
            heapify(arr, i, 0);
        }
    }

    private static void heapify(int[] arr, int n, int i) {
        int p = i; // 부모 노드.
        int l = i * 2 + 1; // 왼쪽 자식 노드.
        int r = i * 2 + 2; // 오른쪽 자식 노드.

        // 왼쪽 자식 노드와 부모 노드를 비교하여 큰 값을 부모 노드로 올린다.
        if (l < n && arr[p] < arr[l]) p = l;

        // 오른쪽 자식 노드와 부모 노드를 비교하여 큰 값을 부모 노드로 올린다.
        if (r < n && arr[p] < arr[r]) p = r;

        // 부모 노드를 가리키는 p 값이 바뀌면 위치를 교환하고
        // heapify()를 호출하여 과정을 반복한다.
        if (i != p) {
            swap(arr, p, i);
            heapify(arr, n, p);
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }

}
```

<aside>
⏰ Heap Sort’s 
Time Complexity :  **O($NlogN$)** 
Space Complexity : **O(n)**

</aside>

# Counting Sort (계수 정렬)

**범위 조건**이 있는 경우에 한해서 사용할 수 있는 굉장히 빠른 정렬방법. 단순히 크기를 기준으로 

배열의 범위만큼 크기의 배열을 하나 만든다 (count)

그리고 배열을 한 번 훑으면서 해당 원소에 해당한느 값을 ++ 해준다

그러면 그냥 풀면 1111122223333444555 이런식으로 풀기만 하면 된다

![Untitled 7](https://user-images.githubusercontent.com/39071638/210305954-dfff3e5b-8c64-429b-9065-8282532446c7.png)
![Untitled 8](https://user-images.githubusercontent.com/39071638/210305960-34bfedca-1682-4a80-8986-46dad9c96c46.png)
![Untitled 9](https://user-images.githubusercontent.com/39071638/210305968-2e63689a-e747-42bb-beb8-818ee64e5dfe.png)
![Untitled 10](https://user-images.githubusercontent.com/39071638/210305977-373c9dcf-6389-4900-9ab9-545b601a2998.png)
![Untitled 11](https://user-images.githubusercontent.com/39071638/210305985-f7d803ae-5f8d-4d47-b3f9-e20cab9b309e.png)
![Untitled 12](https://user-images.githubusercontent.com/39071638/210305992-499d6b7b-da21-4b51-a84b-1af82f9eb151.png)
![Untitled 13](https://user-images.githubusercontent.com/39071638/210306004-49103670-7f00-448f-97a7-cc60d406077a.png)
![Untitled 14](https://user-images.githubusercontent.com/39071638/210306009-62d84bd9-19a3-4cd7-b1c5-2eb1c5c6d1f8.png)
![Untitled 15](https://user-images.githubusercontent.com/39071638/210306020-3452a0ec-40df-47da-8f29-3b3d8beae642.png)
크기를 기준으로 하니깐 속도가 매우 빠름

```c
#include <stdio.h>
int tmp;
int count[6];
int array[30] = {1, 2, 4, 5, 4, 3, 5 ,4, 4, 3, 3, 2, 2, 1, 3, 5, 5, 1, 1, 2, 2,
	3, 2, 2, 5, 4, 4, 2, 1, 2};
int main(void){
	for(int i = 0; i < 30; i++) count[array[i]]++;
	for(int i = 0l i <= 5l i++)
		if(count[i] != 0)
			for(int j = 0; j < count[i]; j++)
				printf("%d ", i);
	return 0l
}
```

<aside>
⏰ Counting Sort’s 
Time Complexity :  **O($N$)** 
Space Complexity : **O(n)**

</aside>

# Binary Search

이진 / 이분 탐색으로 불리는 탐색 기법으로 우리가 숫자 말하기 게임을 할 때, 사용하는 기법이다

처음부터 끝까지 돌면서 탐색을 하는 것 보다 훨씬 빠르다

```java
import java.util.Arrays;

/**
 * created by victory_woo on 2020/04/30
 */
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {2, 13, 6, 5, 12, 15, 23, 17, 19, 10,};
        System.out.println(solution(17, arr));
    }

    private static int solution(int target, int[] arr) {
        Arrays.sort(arr);
        int left = 0, right = arr.length - 1, mid = 0;

        while (left < right) {
            mid = (left + right) / 2;

            if (target == arr[mid]) {
                System.out.println("Find Target : " + target + ", Value : " + arr[mid]);
                return arr[mid];
            }

            if (target < arr[mid]) right = mid - 1;
            else left = mid + 1;
        }

        return -1;
    }
}
```

<aside>
⏰ Heap Sort’s 
Time Complexity :  **O($logN$)** 
Space Complexity : **O(n)**

</aside>

[GitHub - JaeYeopHan/Interview_Question_for_Beginner: Technical-Interview guidelines written for those who started studying programming. I wish you all the best.](https://github.com/JaeYeopHan/Interview_Question_for_Beginner)

[GitHub - Seogeurim/CS-study: 🌎 진정한 컴퓨터공학도가 되기 위한 우리들의 지식 정리 공간 💥](https://github.com/Seogeurim/CS-study)

[https://github.com/gyoogle/tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer)

[https://github.com/WooVictory/Ready-For-Tech-Interview](https://github.com/WooVictory/Ready-For-Tech-Interview)
