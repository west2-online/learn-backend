# learn-backend 第一轮考核：Go 起步任务与赛事数据处理

本轮考察 Go 基础语法、用 HTTP 抓取数据和解析 JSON，以及运用类、集合和文件读写完成小程序的能力。完成情况以提交代码为准，**不安排答辩**。

## 开始前

- 推荐使用 **Goland**、**Go 1.26**。
- 安装和学习资料见[第 0 轮的 Go 资料](0-开始之前.md#go-第一轮资料)。

## 目的

Go语言基本语法

- 条件，选择
- 循环
- 键值对
- 切片，集合
- 函数
- 通道 Channel
- Go协程 Goroutine

**计算机网络基础**：URL 的组成、HTTP 请求与响应、请求方法、常用请求头（如 `User-Agent`、`Accept`）、常见状态码（如 200、403、429）、超时与重试

**JSON**：对象、数组、字符串与数值类型、嵌套结构，把 JSON 解析成结构体、把结构体序列化为 JSON；

**Git** 的基本使用。

## 基础语法

请使用golang完成下列任务

1. 洛谷P1001：<https://www.luogu.com.cn/problem/P1001>
2. 洛谷P1046：<https://www.luogu.com.cn/problem/P1046>
3. 洛谷P5737：<https://www.luogu.com.cn/problem/P5737>
4. AtCoder ARC017A：<https://www.luogu.com.cn/problem/AT_arc017_1>
   - 对于这道题，请编写一个判断质数的函数`isPrime(x int) bool` ，并且在主函数中调用它

5. 创建一个**切片(slice)** 使其元素为数字`1-50`，从切⽚删掉数字为`3`的倍数的数，并且在末尾再增加⼀个数`114514`，输出切⽚。
**输出示例**

```go
[1 2 4 5 7 8 10 11 13 14 16 17 19 20 22 23 25 26 28 29 31 32 34 35 37 38 40 41 43 44 46 47 49 50 114514]
```

### Bonus

1. 写一个 99 乘法表，代码文件命名为 `6.go`，运行后把结果保存到同目录下的 `ninenine.txt`。

2. 回答问题：Go语言中的切片和数组的区别有哪些？答案越详细越好。Go中创建切片有几种方式？创建map
   呢？

3. 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target 的那
   两个 整数，并返回它们的数组下标。

   你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

   你可以按任意顺序返回答案。

   **示例 1：**

   > 输入：nums = [2,7,11,15], target = 9
   > 输出：[0,1]
   > 解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1]

   **示例2**

   > 输入：nums = [3,2,4], target = 6
   > 输出：[1,2]

- 是否有复杂度`O(n)`的算法？

1. 运行下面代码，在你认为重要的地方写好注释，同时回答下面这些问题
   - 这个代码实现了什么功能？
   - 这个代码利用了golang的什么特性？
   - 这个代码相较于普通写法，是否有性能上的提升？（性能提升：求解速度更快了）

```go
package main

import (
 "fmt"
)

func generate(ch chan int) {
 for i := 2; ; i++ {
  ch <- i
 }
}

func filter(in chan int, out chan int, prime int) {
 for {
  num := <-in
  if num%prime != 0 {
   out <- num
  }
 }
}

func main() {
 ch := make(chan int)
 go generate(ch)
 for i := 0; i < 6; i++ {
  prime := <-ch 
  fmt.Printf("prime:%d\n", prime)
  out := make(chan int)
  go filter(ch, out, prime)
  ch = out
 }
}
```

4.思考一下m个线程打印n个数，如何保证打印的有序性

## 爬取 OSPP

> 爬取  <https://summer.ospp.ac.cn/2025/org/projectlist> 的全部项目，通过浏览器 F12 网络调试工具，找到对应的接口，然后编写爬虫程序，向目标网站发起 HTTP(S) 请求，然后获取我们需要的数据！

- 需要包含以下信息：
  - 项目编号
  - 项目名称
  - 社区名称
  - 项目难度
  - 支持语言

- 可自动翻页（爬虫可以自动对后续页面进行爬取，而不需要我们指定第几页）
- 将爬取到的数据保存为 `data.json`
- 使用 [.gitignore](https://blog.csdn.net/m0_63230155/article/details/134471033) 忽略你爬取的文件

> [!IMPORTANT]
> 遇到不会的概念可以查阅 [第 0 轮的 Go 资料](0-开始之前.md#go-第一轮资料)  
> 也可以自己搜索相关资料或者询问 AI

### Bonus

1. 爬取项目的所有申请书 PDF 文件
2. 并发爬取，同时给出加速比（加速比：相较于普通爬取，快了多少倍）
3. 将爬取的数据存入 SQLite 数据库（`data.db`），原生 SQL 或 ORM 都可以

## 提交

作业需要提交到作业仓库的 [`work1/`](https://github.com/west2-online-reserve/collection-golang/tree/main/work1) 目录

结构如下：

```shell
📁 work1/
└── 📁 你的 GitHub 用户名/
    ├── 📁 golang-basic/ Go 语言基础
    └── 📁 ospp-crawler/ OSPP 爬虫
```

> [!IMPORTANT]
> 禁止提交你爬取的文件（`data.json`、`data.db`）以及你的二进制程序，只需要提交你的项目源代码

## 要求

1. 不要抄袭
2. 不要抄袭
3. 不要抄袭
4. 遇到不会的地⽅时，⾸先尝试⾃⼰去解决，可以去百度、⾕歌上寻求帮助，能⽤`搜索引擎`解决**⾃⼰**的问题是⼀项⾮常⾮常重要的能⼒。
