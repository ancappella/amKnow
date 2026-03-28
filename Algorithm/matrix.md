# 矩阵

## 矩阵置零

给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

### 方法1

时间复杂度 `o(m*n)`，空间复杂度 `o(m+n)`

使用两个一维数组分别记录，原始二维数组是否需要记录信息。

**我们约定**

- `col []int` 记录每一列是否要置为 0
- `Row []int` 记录每一行是否要置为 0

### 方法2

时间复杂度 `o(m*n)`，空间复杂度 `o(1)`，两个标记变量

使用matrix的第一行和第一列的一维空间，代替方法1的额外空间col和row，但是原始的行和列是否有0，需要额外使用两个标记变量标记一下。最后的时候单独处理第一行和第一列，如果原始就有需要分别置为0。

**我们约定**

- `Rowhaszero` 原始的第 0 行是否要置 0
- `Colhaszero` 原始的第 0 列是否要置 0
- `matrix[0][j]` 标记第 j 列是否要置 0
- `matrix[i][0]` 标记第 i 行是否要置 0

### 方法3

时间复杂度 `o(m*n)`，空间复杂度 `o(1)`，一个标记变量

与方法2的改进，这次只使用标记变量记录第 0 行是否要置为 0。

这个问题可以从数据存储数量反推，已知需要 n+m  
存储每一个行：列是否需要清空的情况，但是第一行和第一列一共是 m+n-1 位置，所以需要额外一个变量，重叠位置 `[0][0]` 它只能表示第一行或者第一列。

**我们约定**

- `firstrowhaszero` 原始的第 0 行的是否要置为 0
- `matrix[0][j]` 标记第 j 列是否要置为 0
- `matrix[i][0]` 标记第 i 行是否要置为 0
- `matrix[0][0]` 原始第 0 列是否要置为 0

**firstrowiszero**

```text
[0][1]=0
[0][2]=0
[0][3]=0

[0][0]=0 // firstcoliszero

[1][0]=0
[2][0]=0
[3][0]=0
```

### 代码实现

```go
func setZeroes(matrix [][]int)  {
    n := len(matrix)
    m := len(matrix[0])
    firstRowisZero := false
    for i := range matrix {
        for j ,v := range matrix[i] {
            if v == 0 {
                if i == 0 {
                    firstRowisZero = true
                } else if j == 0 {
                    matrix[0][0] = 0
                } else {
                   matrix[i][0] = 0
                   matrix[0][j] = 0
                }
            }
        }
    }
    for i := 1;i < n; i++ {
        for j := 1;j < m ; j++ {
            if matrix[i][0] == 0 || matrix[0][j] == 0 {
               matrix[i][j] = 0
            }
        }
    }
    if firstRowisZero {
       // 先不要覆盖matrix[0][0] 这里记录的是第0列是否需要被覆盖
       for j := 1;j < m; j++ {
            matrix[0][j] = 0
        }
    }
    if matrix[0][0] == 0 {
        for i := 0;i < n ; i++ {
            matrix[i][0] = 0
        }
    }
    // 这里再处理一下firstrowzero
    if firstRowisZero && matrix[0][0] != 0 {
        matrix[0][0] = 0
    }
}
```

---

## 螺旋矩阵

给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

**旋转顺序**

左->右、上->下、右->左、下->上

**四个 for 循环**

每次旋转边界：

```text
top = 0
button = len(matrix) - 1
left = 0
right = len(matrix[0]) - 1
```

---

## 旋转图像

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

逆时针旋转理解成先顺时针旋转90, 再水平旋转180度。

```text
[i,j] => [j,i] => [j,n-i-1]
```

化简下来就是

```text
[i,j] => [j, n - i - 1]
```

我们在枚举的时候，是枚举最终位置 `[i,j]`，找到存放它的数。

```text
[n-j-1,i] => [i,j]
[n-i-1,n-j-1] => [n-j-1,i]
[j, n-i-1] => [n-i-1,n-j-1]
[i,j] => [j,n-i-1]
```
