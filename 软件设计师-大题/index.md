# 软件设计师-大题


[TOC]
# 数据流图DFD
### 基本图形
1. 图形含义
	- 数据流、加工、数据存储（文件）、外部实体
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%95%B0%E6%8D%AE%E6%B5%81%E5%9B%BE%E5%90%84%E5%85%83%E7%B4%A0%E5%90%AB%E4%B9%89.png)
	- 案例
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%95%B0%E6%8D%AE%E6%B5%81%E5%9B%BE%E6%A1%88%E4%BE%8B.png)
2. 分层
	- 结构化、自定向下
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%95%B0%E6%8D%AE%E6%B5%81%E5%9B%BE%E5%88%86%E5%B1%82.png)
### 数据字典
1. 符号含义
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%95%B0%E6%8D%AE%E5%AD%97%E5%85%B8%E5%90%84%E9%83%A8%E5%88%86%E5%90%AB%E4%B9%89.png)
### 数据平衡原则
1. 父图和子图（如顶层、0层、1层之间的关系）
	- 进行对比，看哪一部分缺失
2. 子图内平衡（一个加工需要输入和输出）
	- 黑洞：只有输入
	- 奇迹：只有输出
# 数据库
### 设计过程
1. 概念结构设计的产物：ER模型
2. 逻辑结构设计的产物：关系模式（由ER图得到）
	- 如学生（学号，姓名）
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1%E8%BF%87%E7%A8%8B.png)
### ER图
1. 联系类型
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/ER%E5%9B%BE%E5%AE%9E%E4%BD%93%E4%B9%8B%E9%97%B4%E8%81%94%E7%B3%BB%E7%B1%BB%E5%9E%8B.png)
2. 例图
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E5%AE%9E%E4%BD%93%E5%85%B3%E7%B3%BB%E4%BE%8B%E5%9B%BE.png)
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/ER%E5%9B%BE%E6%A1%88%E4%BE%8B%E5%88%86%E6%9E%90.png)
	- 员工和经理是包含关系（使用一条线加一个圆圈）
	- 合并时可以将一的一端合并到多的一端作为外键
	
# UML
### 用例图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E7%94%A8%E4%BE%8B%E5%9B%BE.png)
1. 包含关系include
	- ==必须==使用到包含的用例
2. 扩展关系
	- ==可能不==使用到包含的用例
3. 泛化关系extend
### 类图与对象图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E7%B1%BB%E5%9B%BE%E4%B8%8E%E5%AF%B9%E8%B1%A1%E5%9B%BE.png)
1. 多重度
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E7%B1%BB%E5%9B%BE%E5%A4%9A%E9%87%8D%E5%BA%A6.png)
2. 关系
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E7%B1%BB%E5%9B%BE%E7%9A%84%E5%85%B3%E7%B3%BB.png)
### 顺序图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E9%A1%BA%E5%BA%8F%E5%9B%BE.png)
1. 强调时间顺序
### 活动图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%B4%BB%E5%8A%A8%E5%9B%BE.png)
1. 粗横线代表有并行的线程
2. 带泳道（活动按照归属对象进行划分）
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E5%B8%A6%E6%B3%B3%E9%81%93%E7%9A%84%E6%B4%BB%E5%8A%A8%E5%9B%BE.png) 
### 状态图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E7%8A%B6%E6%80%81%E5%9B%BE.png)
1. 箭线表示事件
2. 节点一般为状态，确定由哪几种状态
### 通信图（协作图） 
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E9%80%9A%E4%BF%A1%E5%9B%BE%EF%BC%88%E5%8D%8F%E4%BD%9C%E5%9B%BE%EF%BC%89.png)
1. 类似顺序图，但是不是很强调时间顺序
2. 节点为对象
### 构件图
# c语言算法
### 分治
1. 思想
	- 大问题拆分为小问题（相互独立）
	- 解决小问题比较容易
	- 小问题进行合并为原来的大问题
2. 递归
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E5%88%86%E6%B2%BB%E4%B8%AD%E7%9A%84%E9%80%92%E5%BD%92%E7%AE%97%E6%B3%95.png)

### 回朔
1. 深度优先搜索，走不通就退回

### 贪心
1. 每次都选择当前最优的
2. 不一定是全局最优的

### 动态规划
1. 大问题拆分为小问题
2. 不一定独立，因此会构造出一个表进行查找

# 面向对象程序设计
### C++
1. 类与派生类
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/c++%E7%B1%BB%E4%B8%8E%E6%B4%BE%E7%94%9F%E7%B1%BB.png)
2. 类外定义函数体
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E7%B1%BB%E5%A4%96%E5%AE%9A%E4%B9%89.png)
3. 构造函数
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/C++%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0.png)
4. 析构函数
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/C++%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0.png)
5. 对象指针与对象引用
	- 对象指针：类名 \*对象指针名
	- 对象引用：类名 &对象引用名 = 被引用对象
	- 通过对象名和对象引用访问对象的成员，使用的运算符是 .
	- 使用对象指针访问对象的成员，使用的运算符是 ->
		- 对象指针名 -> 数据成员名
		- 对象指针名 -> 成员函数名（参数表）
6. 虚函数
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/C++%E8%99%9A%E5%87%BD%E6%95%B0.png)
	- virtual 实现运行时多态
### Java
