# 各种各样的排序算法


### 冒泡排序
学习排序算法，大部分首先接触的就是排序算法，因为好理解，命名也很形象。

![冒泡](https://miasanmia.oss-cn-beijing.aliyuncs.com/picture/2023/01/ef6a43c0-6001-46ca-9a06-b8972a63a5e8.png)

顾名思义：数值就像是在水中的气泡，数值越大气泡越大，越容易冒出水面。

当前索引的值与下一位索引值进行比较，如果当前值大于下一位值，进行换位，继续循环遍历直到最后一位。

此时已找到最大值，且在数组最后一位，再次循环找到剩下数组中的最大值，以此类推，直到遍历还是一个数字。（ps. 为什么要有外循环，就是这个意思）

代码如下：
```java
    public int[] bubbleSort(int[] arr) {
        // 外循环，每次寻找最大值，从数组 n 位开始，n-1,n-2 依次往下循环
        for (int i = arr.length - 1; i > 0; i--) {
            // 内循环，比较 arr[j] > arr[j+1]，换位，找到最大值      
            for (int j = 0; j < i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
        return arr;
    }
```
真的到这里就完了？（ps. 以前我就是这么想的🤦‍♂️），其实还可以优化，考虑下面这种情况，当需要排序的数组恰好是从小到大的顺序呢？🤔

优化代码如下：
```java
    public int[] bubbleSort(int[] arr) { 
        // 外循环，每次寻找最大值，从数组 n 位开始，n-1,n-2 依次往下循环
        for (int i = arr.length - 1; i > 0; i--) {
            // 设置标志，是否有进行过换位
            boolean isChange = false; 
            // 内循环，比较 arr[j] > arr[j+1]，换位，找到最大值      
            for (int j = 0; j < i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    isChange = true;
                }
            }
            // 发现无换位，停止外循环
            if(!isChange) break;
        }
        return arr;
    }
```

### 插入排序
插入排序，对未排序序列，在有排序序列从后往前扫描，找到相应位置并插入。扫描过程中，就是不断元素不断移位，为新元素提供插入空间。

想象打扑克场景

```java
    public int[] insertSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int temp = arr[i];
            int j = i;
            while (j > 0 && temp < arr[j - 1]) {
                arr[j] = arr[j - 1];
                j--;
            }
            if (j != i)
                arr[j] = temp;
        }

        return arr;
    }
```
### 快速排序
快速，就是快

本质：通过分治，把长数组不断变成短数组
```java
    public void swap(int[] nums, int i, int j) {
        // 数组中，不同索引值交换
        int temp = nums[j];
        nums[j] = nums[i];
        nums[i] = temp;
    }

    public int partition(int[] nums, int left, int right) {
        // 以数组最左边的值为基准值
        int i = left, j = right;
        while (i < j) {
            // 从数组最右边往左寻找，第一个大于基准的数
            while (i < j && nums[j] >= nums[left])
                j--;
            // 从数组最左边往右寻找，第一个小于基准的数
            while (i < j && nums[i] <= nums[left])
                i++;
            // 边界值进行交换
            swap(nums, i, j);
        }
        // 与基准值交换
        swap(nums, i, left);
        // 返回基准值索引
        return i;
    }

    public void quickSort(int[] nums, int left, int right) {
        // 数组长度小于 1，结束递归
        if (left >= right)
            return;
        // 设置基准值
        int pivot = partition(nums, left, right);
        // 递归遍历基准值左、右子数组
        quickSort(nums, pivot + 1, right);
        quickSort(nums, left, pivot - 1);
    }
```

### 选择排序
循环遍历剩余数组，找到最小、最大值，插入数组位置，时间复杂度与冒泡排序一样，是个效率很慢的排序，但是很好理解。😄

```java
    public int[] chooseSort(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            // 最小值索引
            int min = i;
            for (int j = i + 1; j < nums.length; j++) {
                // 遍历剩余数组，寻找最小值索引
                if (nums[j] < nums[min]) {
                    min = j;
                }
            }
            if (min != i) {
                // 最小值索引有变化，进行值交换
                int tmp = nums[i];
                nums[i] = nums[min];
                nums[min] = tmp;
            }
        }
        return nums;
    }
```

### 计数排序
用空间换时间的思想，不进行比较值大小，找出最大值，开辟一段内存空间，一般是数组，用索引代表数字，值代表在原数组中出现的次数，分配好后，最后统一排序。

由于不需要比较，时间复杂度是线性的，缺点也很明显，随着数据量的增大、数据值间的差值越大，占用内存会加大。

```java
    public int[] countSort(int[] nums) {

        // 找出最大值，用于分配空间
        int max = getMax(nums);
        int[] bucket = new int[max + 1];

        // 分配空间
        for (int i = 0; i < nums.length; i++) {
            bucket[nums[i]]++;
        }

        // 重新进行排序
        int sortIndex = 0;
        for (int i = 0; i < bucket.length; i++) {
            while (bucket[i] != 0) {
                nums[sortIndex++] = i;
                bucket[i]--;
            }
        }

        return nums;

    }

    public int getMax(int[] nums) {
        // 获取最大值
        int max = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > max) {
                max = nums[i];
            }
        }
        return max;
    }
```

### 桶排序
桶排序是计数排序的升级版。空间换时间的思路。

把符合区间的元素放入一个桶里，桶里元素排序，最后按桶的顺序把元素合并。

把元素**平均**分配到**不同**的桶里时，排序最快。

所以当所有元素被分配到一个桶里时，排序最慢。

如何均匀分配，如何确定桶的数量和桶的容量？来看看 ChatGPT 给出的答案🙂
1. 找出数据的最大值和最小值。
2. 确定桶的数量，一般时通过（最大值-最小值）/ 平均每个桶中元素的数量 来计算得到。
3. 确定每个元素应该分配到哪个桶中，一般是通过（元素值-最小值）/桶的容量 来计算得到。
4. 将元素放入对应的桶中。
5. 确定桶的数量以及每个桶中元素的数量是一个权衡的过程。一般来说，当数据的分布较为均匀时，使用较少的桶，而当数据的分布不均匀时，使用较多的桶。同时，桶的数量还与待排序的数据量有关，如果数据量很大，那么桶的数量就要相应的增加。

``` java
    public int[] bucketSort(int[] nums) {
        int max = nums[0];
        int min = nums[0];
        // 桶容量大小，默认 5，实际使用中根据数据特征进行优化
        int bucketSize = 5;
        // 找出数组中最大值，最小值
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > max) {
                max = nums[i];
            }
            if (nums[i] < min) {
                min = nums[i];
            }
        }
        // 计算桶数量
        int bucketCount = (int) Math.floor((max - min) / bucketSize) + 1;
        // 设置桶
        int[][] buckets = new int[bucketCount][0];
        // 将元素放到不同桶中
        for (int i = 0; i < nums.length; i++) {
            int index = (int) Math.floor((nums[i] - min) / bucketSize);
            buckets[index] = appandArr(buckets[index], nums[i]);
        }
        // 对不为空的桶，进行排序
        int index = 0;
        for (int i = 0; i < buckets.length; i++) {
            if (buckets[i].length <= 0) {
                continue;
            }
            Arrays.sort(buckets[i]);
            for (int j = 0; j < buckets[i].length; j++) {
                nums[index++] = buckets[i][j];
            }
        }
        return nums;
    }

    public int[] appandArr(int[] nums, int value) {
        // 自动扩容
        nums = Arrays.copyOf(nums, nums.length + 1);
        nums[nums.length - 1] = value;
        return nums;
    }
```
