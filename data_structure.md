# 6. 排序

## 6.1 排序的基本概念

排序算法的稳定性：指的是在排序的过程中，如果待排序序列有多个相同的元素，在排序完成后这些相同的元素的相对位置不变，那么称这种算法是稳定的，反之，就不是稳定的。***注：排序算法的稳定性与算法的性能无关***。

基于比较的排序算法所需比较次数的下界为 $n\log_{2}n \approx \log_{2}n!$ ，当$n \to \infty$时 。

以上式子可以根据斯特林公式得到，由斯特林公式可知 $ \lim\limits_{n\rightarrow\infty} \frac{n!}{\sqrt{2\pi n}(\frac{n}{e})^n} = 1$ ，从而当n充分大时，$n!$ 接近于 $\sqrt{2\pi n}(\frac{n}{e})^n$ ，将此表达式代入 $\log_{2}n!$ 取最高阶并去掉系数可得 $n\log_{2}n$。

## 6.2 插入排序

### 6.2.1 直接插入排序

直接插入排序是每次选取一个数key，然后将其插入到第 $j$ 个位置（即首个小于或等于其的数之后）。

此算法是一种稳定的算法。

代码如下：

``` c++
#include <iostream>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};

int main() {
    int n = sizeof(a) / sizeof(int);

    // 直接插入排序
    for (int i = 1, j;i < n;++i) {
        int key = a[i];
        for (j = i;j > 0 && a[j - 1] > key;--j) {
            a[j] = a[j - 1];
        }
        a[j] = key;
    }

    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    
    return 0;
}
```

此算法的最优时间复杂度为：$O(n)$（即待排序的序列是有序的，此时只需要进行一次扫描即可），最坏和平均时间复杂度为：$O(n^2)$ ，空间复杂度：$O(1)$。

### 6.2.2 插入排序（二分优化版）

在上面的朴素插入排序中，我们每次线性的查找合适的位置导致查找过程的最坏时间复杂度为 $O(n)$。事实上，在每次寻找合适位置的过程中，不难发现$[0,i)$ 这个子序列是有序的，因此，我们可以使用二分查找在 $[0,i)$ 上查找合适的位置。

代码如下：

``` c++
#include <iostream>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};

int main() {
    int n = sizeof(a) / sizeof(int);
    // 插入排序
    for (int i = 1, j;i < n;++i) {
        int key = a[i];
        int l = 0, r = i;
        // 二分查找（左中位数版本）
        while (l < r) {
            int mid = (l + r) / 2;
            if (a[mid] >= key) r = mid;
            else l = mid + 1;
        }
        for (j = i;j > l;--j) a[j] = a[j - 1];
        a[j] = key;
    }
    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    return 0;
}
```

此算法仅仅只是优化了朴素插入排序中的常数，简单来说就是移动元素仍需要 $O(n)$ 的时间复杂度，故总的时间复杂度仍然是 $O(n^2)$。



## 6.3 冒泡排序

冒泡排序的工作原理是每次检查相邻两个元素，如果前面的元素与后面的元素满足给定的排序条件，就将相邻两个元素交换。当没有相邻的元素需要交换时，排序就完成了。

经过 $i$ 次扫描后，数列的前 $i$ 项必然是最小的 $i$ 项（并且有序），因此冒泡排序最多需要扫描 $n-1$ 遍数组就能完成排序。

冒泡排序是一种稳定的算法。在序列有序时，只需要扫描一遍数组，不执行任何交换操作，此时时间复杂度为 $O(n)$。在最坏条件下（即序列完全逆序），此时需要进行 $\frac{(n-1)n}{2}$ 次交换，此时时间复杂度为 $O(n^2)$。其平均时间复杂度也为 $O(n^2)$。

过程演示：

<iframe src="https://publish.obsidian.md/help-zh/%E7%94%B1%E6%AD%A4%E5%BC%80%E5%A7%8B" style="width:800px; height:500px;" frameborder="0"></iframe>

代码如下：

``` c++
#include <iostream>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};
// 冒泡排序
void bubble_sort(int a[], int n) {
    for (int i = 0;i < n - 1;++i) {
        for (int j = n - 1;j > i;--j) {
            if (a[j] < a[j - 1]) swap(a[j], a[j - 1]);
        }
    }
}

int main() {
    int n = sizeof(a) / sizeof(int);
    bubble_sort(a, n);
    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    return 0;
}
```



## 6.4 选择排序

选择排序的工作原理是每次找出第 $i$ 小的元素（也就是 $A_{i .. n}$ 中最小的元素，$i=1,2,3,...,n$），然后将这个元素与数组第 $i$ 个位置上的元素交换。

选择排序的最优、最坏和平均时间复杂度均为 $O(n^2)$，并且它是一种不稳定的算法。

代码如下：

``` c++
#include <iostream>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};
// 选择排序
void selection_sort(int a[], int n) {
    for (int i = 0;i < n - 1;++i) {
        int it = i;
        // 找到当前最小元素下标
        for (int j = i + 1;j < n;++j) {
            if (a[it] > a[j]) it = j;
        }
        swap(a[it], a[i]);
    }
}

int main() {
    int n = sizeof(a) / sizeof(int);
    selection_sort(a, n);
    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    return 0;
}
```



## 6.5 希尔排序

希尔排序的过程如下：

1. 将待排序序列分为若干子序列（每个子序列的元素在原始数组中间距相同），例如：$a_i,a_{i+k},a_{i+2k},...$；
2. 对这些子序列进行插入排序；
3. 减小每个子序列中元素之间的间距，重复上述过程直至间距减少为 $1$。

希尔排序是一种不稳定的算法。其最优时间复杂度为 $O(n)$ ，平均和最坏时间复杂度与划分子序列所用间距有关，选取合适的间距可以使算法总的时间复杂度降到 $O(n\log^2_2n)$。

具体代码如下：

``` c++
#include <iostream>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};
// 希尔排序
void shell_sort(int a[], int n) {
    int k = 1;  // 间距
    while (k < n / 3) {
        k = 3 * k + 1;
    }
    while (k > 0) {
        // 对子序列进行排序
        for (int i = k, j;i < n;++i) {
            int key = a[i];
            for (j = i;j >= k && a[j - k] > key;j -= k) {
                swap(a[j], a[j - k]);
            }
            a[j] = key;
        }
        k /= 3;
    }
}

int main() {
    int n = sizeof(a) / sizeof(int);
    shell_sort(a, n);
    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    return 0;
}
```



## 6.6 快速排序

快速排序的工作原理是通过分治的方式来将一个数组排序。

主要分为以下三个过程：

1. 选取主元，将数列划分为两部分（要求保证相对大小关系）；
2. 递归到两个子序列中分别进行快速排序；
3. 不用合并，因为此时数列已经完全有序（因为是原地排序）。

快速排序是一种不稳定的排序算法。最优和平均时间复杂度为 $ O(n\log_2n)$ ，最坏时间复杂度为 $O(n^2)$（即在降序序列中选择首元素作为主元时会导致分割的区间不均匀）。

代码如下：

```c++
#include <iostream>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};
// 快速排序
void quick_sort(int a[], int l, int r) {
    // 随机选取主元（小优化）
    int p = a[rand() % (r - l) + l];
    int i = l, j = r - 1;
    // 将当前区间划分成两部分 a[l...i] <= p 且a[j...r] >= p
    while (i <= j) {
        // 寻找大于或等于主元的元素
        for (;i < r && a[i] < p;++i);
        // 寻找小于或等于主元的元素
        for (;j >= l && a[j] > p;--j);
        // 交换两元素
        if (i <= j) {
            swap(a[i], a[j]);
            ++i, --j;
        }
    }
    // 如果左侧或右侧区间有2个或2个以上的元素就继续排序
    if (j > l) quick_sort(a, l, j + 1); // 这样的区间[...)
    if (r - i > 1) quick_sort(a, i, r);
}

int main() {
    int n = sizeof(a) / sizeof(int);
    quick_sort(a, 0, n);
    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    return 0;
}
```

快速排序稍微修改即可在 $O(n)$ 的时间复杂度找出第k大的元素（快选算法，有空再加入）。



## 6.7 堆排序

堆排序的过程如下（假定是升序排序）：

1. 建立大根堆（堆的建立及其相关操作参见堆）；
2. 每次取堆顶元素和待排序序列的最后一个元素交换（每次交换会使得待排序序列长度减一，保证整个序列后 $n-i+1$（已经参与交换的元素个数）个元素是有序的，即 $a_i,a_{i+1},a_{i+2},...,a_{n}$有序）；
3. 重复步骤2，直到待排序序列长度为1即可。

堆排序是一种不稳定的排序算法，其总体时间复杂度为 $O(n\log_2n)$，空间复杂度是 $O(1)$（原地排序算法）。

代码如下：

```c++
#include <iostream>
using namespace std;

// 索引从1开始
int a[] = {0, 3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};
int n = sizeof(a) / sizeof(int) - 1;
// 下沉操作
void down(int i, int n) {
    while (i <= n) {
        int t = 2 * i;
        if (t + 1 <= n && a[t] < a[t + 1]) t++;
        if (t > n || a[i] >= a[t]) break;
        swap(a[i], a[t]);
        i = t;
    }
}
// 线性建堆
void make_heap(int a[]) {
    for (int i = n;i > 0;--i) down(i, n);
}
// 堆排序
void heap_sort(int a[], int n) {
    make_heap(a); // 建立大根堆
    // cur_len为当前待排序的序列长度
    for (int i = 1, cur_len = n;i < n;++i) {
        swap(a[1], a[cur_len--]);
        down(1, cur_len);
    }
}

int main() {
    heap_sort(a, n);
    for (int i = 1;i <= n;++i) printf("%d ", a[i]);
    return 0;
}
```



## 6.8 归并排序

归并排序基于分治法，其原理就是将原数组分割成若干个子数组，直到每个子数组元素个数不超过1个，再向上合并。因为分割以后每个子数组最初元素个数不超过1个，那么此时任意子数组必然是有序的，这个时候将两个有序数组进行合并会得到一个更大的有序数组，一直向上合并，直到合并为原数组，数组就排好序了。

归并排序的总体时间复杂度为 $O(n\log_2n)$ ，空间复杂度是 $O(n)$，是稳定的排序算法。

示例代码如下：

```c++
#include <iostream>
#include <vector>
using namespace std;

int a[] = {3, 2, 2, 1, 0, 18, 15, 60, 41, 1, 78};
int n = sizeof(a) / sizeof(int);
vector<int> t;
// 归并排序
void merge_sort(int a[], int l, int r) {
    if (r - l <= 1) return;
    
    int mid = (l + r) / 2;
    merge_sort(a, l, mid);merge_sort(a, mid, r);
    // 将子数组的数据复制到临时数组
    for (int i = l;i < mid;++i) t[i] = a[i];
    for (int i = mid;i < r;++i) t[i] = a[i];
    // 将结果合并到原数组
    for (int i = l, x = l, y = mid;i < r;++i) {
        if (x < mid && y < r) {
            // 为保证其稳定性 所以需要 <=
            if (t[x] <= t[y]) a[i] = t[x++];
            else a[i] = t[y++];
        }else if (x < mid) {
            a[i] = t[x++];
        }else {
            a[i] = t[y++];
        }
    }
}

int main() {
    t = vector<int>(1e4);
    merge_sort(a, 0, n);
    for (int i = 0;i < n;++i) printf("%d ", a[i]);
    return 0;
}
```

归并排序的基本应用是统计逆序对个数（由于810不考，所以以后闲的时候更新）
