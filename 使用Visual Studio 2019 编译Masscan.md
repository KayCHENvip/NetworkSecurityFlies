**目录**

使用Visual Studio 2019 编译Masscan

1.从github下载源码

2.安装Visual Stiudio 2019

3.准备编译Masscan

4.编译Masscan

总结

* * *

## **使用Visual Studio 2019 编译Masscan**

### **1.从github下载源码**

可以直接在GitHub上搜索，下载后的源码目录结构。 

![](https://img-blog.csdnimg.cn/5336b9cf3b104de2afa7cd2a865ac294.png)

### **2.安装Visual Stiudio 2019**

![](https://img-blog.csdnimg.cn/9b56022cdd9641ad9e0f2b5f800849aa.png)

### **3.准备编译Masscan**

在源码中找到vs10目录，里面有一个.sln文件，双击点开，会默认使用VS2019工具打开。

![](https://img-blog.csdnimg.cn/7707510bc87f46e9b7f874324c731b0a.png)

在第一次打开的时候会自动根据平台的不同切换平台，只能说相当智能，让小白也能流畅使用。

![](https://img-blog.csdnimg.cn/09331693ee614394913e48fa8493854a.png)

进入src目录，找到string_s.h打开

在81行后面加上以下代码

![](https://img-blog.csdnimg.cn/d7e82a906bea4596bada0f412a65dde2.png)

这里简答介绍一下变量_MSC_VER == 1929，需根据当前VS的版本号来判断。

![](https://img-blog.csdnimg.cn/b029858b35c64c0db1e0817b20936d73.png)

详细信息请参考Microsoft官方：[预定义宏 | Microsoft Learn](https://learn.microsoft.com/zh-cn/cpp/preprocessor/predefined-macros?view=msvc-170&viewFallbackFrom=vs-2019 "预定义宏 | Microsoft Learn")

> 官方解释：
> 
> _MSC_VER：定义为编码编译器版本号的主版本号和次版本号元素的整数文本。 主版本号是用句点分隔的版本号的第一个元素，而次版本号是第二个元素。 例如，如果 Microsoft C/C++ 编译器的版本号为 17.00.51106.1，则 _MSC_VER 宏计算结果为 1700。 在命令行中键入 cl /?，查看编译器的版本号。 任何情况下都会定义此宏。

官网关于_MSC_VER变量的定义如下：

Visual Studio 版本

| 

_MSC_VER  
  
---|---  
  
Visual Studio 6.0

| 

1200  
  
Visual Studio .NET 2002 (7.0)

| 

1300  
  
Visual Studio .NET 2003 (7.1)

| 

1310  
  
Visual Studio 2005 (8.0)

| 

1400  
  
Visual Studio 2008 (9.0)

| 

1500  
  
Visual Studio 2010 (10.0)

| 

1600  
  
Visual Studio 2012 (11.0)

| 

1700  
  
Visual Studio 2013 (12.0)

| 

1800  
  
Visual Studio 2015 (14.0)

| 

1900  
  
Visual Studio 2017 RTW (15.0)

| 

1910  
  
Visual Studio 2017 版本 15.3

| 

1911  
  
Visual Studio 2017 版本 15.5

| 

1912  
  
Visual Studio 2017 版本 15.6

| 

1913  
  
Visual Studio 2017 15.7 版

| 

1914  
  
Visual Studio 2017 版本 15.8

| 

1915  
  
Visual Studio 2017 版本 15.9

| 

1916  
  
Visual Studio 2019 RTW (16.0)

| 

1920  
  
Visual Studio 2019 版本 16.1

| 

1921  
  
Visual Studio 2019 版本 16.2

| 

1922  
  
Visual Studio 2019 版本 16.3

| 

1923  
  
Visual Studio 2019 版本 16.4

| 

1924  
  
Visual Studio 2019 版本 16.5

| 

1925  
  
Visual Studio 2019 版本 16.6

| 

1926  
  
Visual Studio 2019 版本 16.7

| 

1927  
  
Visual Studio 2019 v16.8、v16.9

| 

1928  
  
Visual Studio 2019 版本 16.10、16.11

| 

1929  
  
Visual Studio 2022 RTW (17.0)

| 

1930  
  
Visual Studio 2022 版本 17.1

| 

1931  
  
Visual Studio 2022 版本 17.2

| 

1932  
  
### 4.编译Masscan

使用工具直接生成masscan可执行文件

![](https://img-blog.csdnimg.cn/109791637de241c482c9c9bfdcb538a9.png)

执行上述操作后会在bin目录下生成exe文件

![](https://img-blog.csdnimg.cn/5a51f37b0efa431e9cf5503b633e8538.png)

测试一波

![](https://img-blog.csdnimg.cn/16b754e8facc447791f3cefbc2f57bff.png)

### 总结

编译后的程序是依赖VS2019的，换其他PC测试会提示缺少dll文件，所以方法如此简单完全可以自己编译，有时间再找一下原因......
