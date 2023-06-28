## RISCV-构建错误日志-数据集  

### 1.数据集简介

本数据集维护由RISC-V架构软件包构建错误日志组成的数据集，出于研究目的地免费访问这些数据集，日志数据集主要来源于openSuse和openEuler构建平台的构建日志。只要还在维护，日志就不会以任何方式进行清理、匿名化或修改。**后续还会持续收集日志并更新数据集**。  

- ##### 当前可用的日志数据集：

  | 构建平台  |               描述               | 标签 | 数据量 |
  | :-------: | :------------------------------: | :--: | :----: |
  | openSuse  | openSuse构建RISCV软件包错误日志  |  √   |  237   |
  | openEuler | openEuler构建RISCV软件包错误日志 |  √   |  602   |

### 2. 收集方式

日志数据收集主要通过各平台OBS官网,如：  

```
openSuse：https://build.opensuse.org/  
openEuler：https://build.openeuler.openatom.cn/   
```

查找关于RISCV构建的包，通过爬虫爬取，日志URL如下：

```python
openSuse_url_template = "https://build.opensuse.org/public/build/openSUSE:Factory:RISCV/standard/riscv64/{包名}/_log"
openEuler_url_template = "https://build.openeuler.openatom.cn/public/build/openEuler:Mainline:RISC-V/advanced_riscv64/riscv64/{包名}/_log
具体步骤：  
(1)在obs官网找到RISCV架构构建失败的软件包目录，并保存  
(2)通过以上日志URL获取requert请求：response = requests.get(url)
(3)通过：f.write(response.content)  #下载日志文件
```

### 3. 数据分类与标签

如图1所示，即为本数据集的分类情况，数字即为该类的标签名

<img src="[3.png](https://github.com/mstsky115/RISC-V-build-errors-data-set/blob/main/picture/3.png)" alt="RISCV构建错误日志分类" style="zoom:67%;" />

​                                                                                               图1  RISC-V软件包构建错误分类及其标签

### 4. 数据样本覆盖情况（类别占比）

如图2所示，饼图展示了样本标签的占比情况

<img src="[pieChart.png](https://github.com/mstsky115/RISC-V-build-errors-data-set/blob/main/picture/pieChart.png)" alt="饼图" style="zoom: 67%;" />

​                                                                                                            图2  构建错误日志样本的分布情况

### 5. 类别介绍

#### 5.1 不兼容问题

软件包构建错误的**主要原因**有**编译器不兼容、架构不兼容、函数更新、文件冲突**等。
(1**)编译器不兼容**，其**错误日志特征**一般包含有关**compiler**的相关信息，示例如下：

```
 “error: line 62: Version required: Requires:    ghc-compiler =   
 Building target platforms: riscv64-suse-linux”
```

(2)**架构不兼容**：其**错误日志特征**一般包含有关**Arch、Architecture**等的相关信息，示例如下：

```
“meson.build:90:1: ERROR: Problem encountered: Architecture "riscv64" is not supported. ”
```

(3)**函数更新**：其**错误日志特征**一般包含**有关函数已更新、deprecated**等的相关信息，示例如下：

```
“net_ssl.cpp:216:35: warning: 'EC_KEY* EC_KEY_new()' is deprecated: Since OpenSSL 3.0 [-Wdeprecated-declarations] ”
```

(4)**文件冲突**：其**错误日志特征**一般包含**文件conflicts**等的相关信息，示例如下 ：

```
“file …/libasm-0.185.so from install of elfutils-libs-0.185-3.oe1.riscv64 conflicts with file from package elfutils-0.185-5.oe1.riscv64 ”
```

#### 5.2 构建配置问题

软件包构建错误的**主要原因**有**依赖缺失、依赖版本不匹配**等。
(1)**依赖缺失**：其**错误日志特征**一般包含**’依赖’no module、failded build‘依赖’**等的相关信息，示例如下

```
“ModuleNotFoundError: No module named 'javapackages' ”
```

(2)**依赖版本不匹配**：其**错误日志特征**一般包含**’依赖’>=某个版本**等的相关信息，示例如下

```
 “ pkg_resources.DistributionNotFound: The 'pytz>=2015.7' distribution was not found and is required by babel ”
```

#### 5.3 测试案例问题

软件包构建错误的**主要原因**有**测试案例执行失败**等。
(1)**测试案例执行失败**：其**错误日志特征**一般包含**Test fialed、Test error、Test failures**的相关信息，示例如下 

```
“[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.22.0:test (default-test) on project commons-pool2: There are test failures. ”
```

#### 5.4 源代码问题

软件包构建错误的**主要原因**有**变量/函数等未定义、函数错误使用、代码拼写/语法错误**等。

(1**)变量/函数等未定义**：其**错误日志特征**一般包含**变量等undefined**的相关信息，示例如下

```
“…/cc7rEUJG.ltrans5.ltrans.o: in function `.L0 ':

…/ns_turn_utils.c:606: undefined reference to `FIPS_mode' ”
```

(2**)函数错误使用**：其**错误日志特征**一般包含**函数error**的相关信息，示例如下

```
 “turnrest.c:164:9: error: void value not ignored as it ought to be 164 | curl_easy_setopt(curl, api_http_get ? CURLOPT_HTTPGET : CURLOPT_POST, 1); ”
```

(3)**代码拼写/语法错误**：其**错误日志特征**一般包含**某个变量的error**等的相关信息，示例如下

```
 “error: 'class Quotient::AccountSettings' has no member named 'accessToken'; did you mean 'clearAccessToken'? ”
```

#### 5.5 内存问题

软件包构建错误的**主要原因**有**内存溢出**等。
(1)**内存溢出**：其**错误日志特征**一般包含**memory、Kill process**等的相关信息，示例如下 

```
Out of memory: Killed process 16993 (pandoc) total-vm:1079089668kB, anon-rss:2877596kB, file-rss:0kB, shmem-rss:0kB, UID:399 pgtables:9740kB oom_score_adj:0 
```

#### 5.6 其他问题

无法归类任何一类，主要原因有①判断不出错误的主要原因，②样本数量过少（少于5个）无法形成一个独立的类别。

#### 5.7 超时问题

软件包构建错误的**主要原因**有**代码执行时间超过阈值**等。

(1)**代码执行时间超过阈值**：其**错误日志特征**一般包含**after seconds of inactivity、Timeout**等的相关信息，示例如下

```
 “Job seems to be stuck here, killed. (after 28800 seconds of inactivity) ”
```

#### 5.8 文件/目录缺失问题

软件包构建错误的**主要原因**有**构建过程找不到相关文件或目录**等。

(1)**构建过程找不到相关文件或目录**：其**错误日志特征**一般包含**No such file or directory**的相关信息，示例如下

```
 “touch: cannot touch …/prerelease-lifecycle-gen': No such file or directory ”
```

#### 5.9 插件问题

软件包构建错误的**主要原因**有**插件无法编译**等。

(1)**插件无法编译**：其**错误日志特征**一般包含**Plugin error**等的相关信息，示例如下

```
“ [ERROR] Some problems were encountered while processing the POMs:Unresolveable build extension: Plugin org.apache.felix:maven-bundle-plugin:2.5.0 or one of its dependencies could not be resolved”
```

#### 5.10 网络问题

软件包构建错误的**主要原因**有**无法下载依赖项**等。

(1)**无法下载依赖项**：其**错误日志特征**一般包含**Could not transfer artifact from/to**等的相关信息，如示例如下

```
“Plugin org.antlr:antlr3-maven-plugin:3.5 or one of its dependencies could not be resolved: Cannot access huaweicloud in offline mode and the artifact …”
```

### 6. CSV数据集文件属性  

|  列数   | 含义                                |
| :-----: | ----------------------------------- |
| column1 | label		（标签类别）           |
| column2 | log		（日志的错误关键信息行） |
| column3 | package	（包名）                 |

```
示例如下：
2	,ModuleNotFoundError: No module named 'javapackages' ,aalto-xml 
8	,Job seems to be stuck here, killed. (after 28800 seconds of inactivity)	,apache-poi 
3	,TESTFAIL: These test cases failed: 582	,curl
```
