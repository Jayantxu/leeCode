*原题链接：👉：[旋转图像](https://leetcode-cn.com/problems/rotate-image/description/)*

题目描述:

1. 给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。
2. 你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

示例：
```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```
这个意思：
```
[
	[1, 2, 3],
	[4, 5, 6],
	[7, 8, 9]
]

=>

[
	[7, 4, 1],
	[8, 5, 2],
	[9, 6, 3]
]
```

思路：

> 原地旋转，那么就需要借用一些变量了，既然需要原地，那么就一定转换之间有公式关系
> 那就找找是啥关系。

[0, 0] = 1 ===> [0, 2]
[1, 0] = 4 ===> [0, 1]
[2, 0] = 7 ===> [0, 0]
[2, 1] = 8 ===> [1, 0]

在图纸上划了一下各自的对应关系，很清晰`旋转`前后的关系，想不出，没想出一个通用公式...🤒

- 题解

先水平翻转之后，再按照对角线翻转，这样利用原地的遍历即可。
```
[
	[1,2,3],
	[4,5,6],
	[7,8,9]
]

先水平翻转处理为:

[
	[7,8,9],
	[4,5,6]
	[1,2,3]
]

*然后再对角线翻转为：*
[
	[7, 4, 1],
	[8, 5, 2],
	[9, 6, 3]
]

其中对角线翻转的点再此：

for(let i = 0; i < mLen; i++) {
	for(let j = 0; j < i; j++) {
		[matrix[j][i], matrix[i][j]] = [matrix[i][j], matrix[j][i]];
	}
}
```

**code:**

```
var rotate = function(matrix) {
    let mLen = matrix.length;
    for(let i = 0; i < Math.floor(mLen / 2); i++) {
        [matrix[i], matrix[mLen - 1 -i]] = [matrix[mLen - 1 -i], matrix[i]];
    }
    for(let i = 0; i < mLen; i++) {
        for(let j = 0; j < i; j++) {
            [matrix[j][i], matrix[i][j]] = [matrix[i][j], matrix[j][i]];
        }
    }
	return matrix;
};
```

