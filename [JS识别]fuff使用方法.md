`ffuf`是一个用Go语言编写的快速网络模糊测试工具。

# ffuf - Fuzz Faster U Fool

一个用Go语言编写的快速Web模糊器。

- [安装](https://github.com/ffuf/ffuf#installation)
- [示例使用](https://github.com/ffuf/ffuf#example-usage)
  - [内容发现](https://github.com/ffuf/ffuf#typical-directory-discovery)
  - [Vhost发现](https://github.com/ffuf/ffuf#virtual-host-discovery-without-dns-records)
  - [参数模糊](https://github.com/ffuf/ffuf#get-parameter-fuzzing)
  - [POST数据模糊](https://github.com/ffuf/ffuf#post-data-fuzzing)
  - [使用外部mutator](https://github.com/ffuf/ffuf#using-external-mutator-to-produce-test-cases)
  - [配置文件](https://github.com/ffuf/ffuf#configuration-files)
- [帮助](https://github.com/ffuf/ffuf#usage)
  - [交互模式](https://github.com/ffuf/ffuf#interactive-mode)

## 安装

- 从[releases页面](https://github.com/ffuf/ffuf/releases/latest)下载预构建的二进制文件，解压并运行。

  _或者_
  
- 如果你使用macOS并安装了[homebrew](https://brew.sh)，可以通过`brew install ffuf`安装ffuf。

  _或者_
  
- 如果你已经安装了较新版本的Go编译器：`go install github.com/ffuf/ffuf/v2@latest`（相同的命令也适用于更新）。

  _或者_
  
- `git clone https://github.com/ffuf/ffuf ; cd ffuf ; go get ; go build`

Ffuf依赖于Go 1.16或更高版本。

## 示例用法

下面的用法示例展示了使用`ffuf`完成的一些最简单的任务。

更详细的文档，其中包含许多功能和示例，可以在`ffuf`的维基页面上找到：[https://github.com/ffuf/ffuf/wiki](https://github.com/ffuf/ffuf/wiki)

你还可以查看这个出色的指南，其中包含真实用例和技巧："[Everything you need to know about FFUF](https://codingo.io/tools/ffuf/bounty/2020/09/17/everything-you-need-to-know-about-ffuf.html)"，由Michael Skelton ([@codingo](https://github.com/codingo)) 编写。

### 典型的目录发现（Typical directory discovery）

![asciicast](https://asciinema.org/a/211350.png)

通过在URL的末尾使用`FUZZ`关键字（`-u`）：

```bash
ffuf -w /path/to/wordlist -u https://target/FUZZ
```

### 虚拟主机发现（无需DNS记录）【Virtual host discovery (without DNS records)】

![asciicast](https://asciinema.org/a/211360.png)

假设默认虚拟主机响应大小为4242字节，我们可以在模糊Host头的同时过滤掉所有这些大小的响应（`-fs 4242`）：

```bash
ffuf -w /path/to/vhost/wordlist -u https://target -H "Host: FUZZ" -fs 4242
```

### GET参数模糊测试（GET parameter fuzzing）

GET参数名的模糊测试与目录发现非常相似，通过将`FUZZ`关键字定义为URL的一部分来实现。这还假定无效GET参数名的响应大小为4242字节。

```bash
ffuf -w /path/to/paramnames.txt -u https://target/script.php?FUZZ=test_value -fs 4242
```

如果参数名已知，可以以相同的方式对值进行模糊测试。此示例假定错误的参数值返回HTTP响应代码401。

```bash
ffuf -w /path/to/values.txt -u https://target/script.php?valid_name=FUZZ -fc 401
```

### POST数据模糊测试（POST data fuzzing）

这是一个非常简单的操作，再次使用`FUZZ`关键字。此示例仅对POST请求的一部分进行模糊测试。我们再次过滤掉401响应。

```bash
ffuf -w /path/to/postdata.txt -X POST -d "username=admin\&password=FUZZ" -u https://target/login.php -fc 401
```

### 最大执行时间（Maximum execution time）

如果你不希望`ffuf`无限制地运行，可以使用`-maxtime`。这在给定时间（以秒为单位）后停止__整个__过程。

```bash
ffuf -w /path/to/wordlist -u https://target/FUZZ -maxtime 60
```

在使用递归时，可以使用`-maxtime-job`控制__每个任务__的最大执行时间。这将在递归功能检测到子目录时停止当前任务并继续下一个任务。新的任务在递归功能检测到子目录时创建。

```bash
ffuf -w /path/to/wordlist -u https://target/FUZZ -maxtime-job 60 -recursion -recursion-depth 2
```

还可以组合这两个标志，限制每个任务的最大执行时间以及整体执行时间。如果不使用递归，则这两个标志的行为相同。

### 使用外部变异器生成测试用例（Using external mutator to produce test cases）

在此示例中，我们将对通过POST发送的JSON数据进行模糊测试。我们使用[Radamsa](https://gitlab.com/akihe/radamsa)作为变异器。

当使用`--input-cmd`时，`ffuf`将匹配显示为它们的位置。这个相同的位置值将作为环境变量`$FFUF_NUM`对调用者可用。我们将使用这个位置值作为变异器的种子。文件`example1.txt`和`example2.txt`包含有效的JSON负载。我们匹配所有响应，但过滤掉响应代码`400 - Bad request`：

```bash
ffuf --input-cmd 'radamsa --seed $FFUF_NUM example1.txt example2.txt' -H "Content-Type: application/json" -X POST -u https://ffuf.io.fi/FUZZ -mc all -fc 400
```

当然，为每个负载调用变异器并不是很有效，因此我们还可以预先生成负载，仍然使用[Radamsa](https://gitlab.com/akihe/radamsa)作为示例：

```bash
# 生成1000个示例负载
radamsa -n 1000 -o %n.txt example1.txt example2.txt

# 这将生成文件1.txt ... 1000.txt
# 现在我们可以从文件中循环读取负载数据用于ffuf
ffuf --input-cmd 'cat $FFUF_NUM.txt' -H "Content-Type: application/json" -X POST -u https://ffuf.io.fi/ -mc all -fc 400
```

### 配置文件

在运行`ffuf`时，它首先检查是否存在默认配置文件。`ffufrc`文件的默认路径为`$XDG_CONFIG_HOME/

ffuf/ffufrc`。你可以在此文件中配置一个或多个选项，它们将应用于每个随后的`ffuf`任务。`ffufrc`文件的示例可以在[这里](https://github.com/ffuf/ffuf/blob/master/ffufrc.example)找到。

有关配置文件位置的更详细描述可以在维基中找到：[https://github.com/ffuf/ffuf/wiki/Configuration](https://github.com/ffuf/ffuf/wiki/Configuration)

通过命令行提供的配置选项将覆盖从默认`ffufrc`文件加载的选项。注意：这不适用于可以多次提供的CLI标志。其中一个例子是`-H`（头部）标志。在这种情况下，命令行上提供的`-H`值将被附加到配置文件中的值上。

此外，如果你希望为不同的用例使用一堆配置文件，你可以通过使用`-config`命令行标志定义配置文件的路径，该标志采用配置文件路径作为其参数。

## 用法

要定义`ffuf`的测试用例，可以在URL（`-u`）、标头（`-H`）或POST数据（`-d`）中的任何地方使用关键字`FUZZ`。

```bash
Fuzz Faster U Fool - v2.1.0

HTTP OPTIONS:
  -H                  头部 `"Name: Value"`，由冒号分隔。可以接受多个 -H 标志。
  -X                  要使用的HTTP方法
  -b                  用于复制为curl功能的Cookie数据`"NAME1=VALUE1; NAME2=VALUE2"`。
  -cc                 用于身份验证的客户端证书。此时需要定义客户端密钥。
  -ck                 用于身份验证的客户端密钥。此时需要定义客户端证书。
  -d                  POST数据
  -http2              使用HTTP2协议（默认：false）
  -ignore-body        不获取响应内容（默认：false）
  -r                  跟随重定向（默认：false）
  -raw                不编码URI（默认：false）
  -recursion          递归扫描。仅支持FUZZ关键字，URL（-u）必须以它结束（默认：false）
  -recursion-depth    最大递归深度（默认：0）
  -recursion-strategy 递归策略："default"为基于重定向的，"greedy"为在所有匹配上递归（默认：default）
  -replay-proxy       使用此代理重播匹配的请求。
  -sni                目标TLS SNI，不支持FUZZ关键字
  -timeout            HTTP请求超时（秒）（默认：10）
  -u                  目标URL
  -x                  代理URL（SOCKS5或HTTP）。例如：http://127.0.0.1:8080或socks5://127.0.0.1:8080

一般选择GENERAL OPTIONS:
  -V                  显示版本信息（默认：false）
  -ac                 自动校准过滤选项（默认：false）
  -acc                自定义自动校准字符串。可以多次使用。意味着 -ac
  -ach                每个主机自动校准（默认：false）
  -ack                自动校准关键字（默认：FUZZ）
  -acs                自定义自动校准策略。可以多次使用。意味着 -ac
  -c                  色彩化输出（默认：false）
  -config             从文件加载配置
  -json               JSON输出，打印换行分隔的JSON记录（默认：false）
  -maxtime            整个进程的最大运行时间（秒）（默认：0）
  -maxtime-job        每个任务的最大运行时间（秒）（默认：0）
  -noninteractive     禁用交互式控制台功能（默认：false）
  -p                  请求之间的秒数`delay`，或者是随机延迟的范围。例如"0.1"或"0.1-2.0"
  -rate               请求每秒的速率（默认：0）
  -s                  不打印附加信息（静默模式）（默认：false）
  -sa                 在所有错误情况下停止。意味着 -sf 和 -se。 （默认：false）
  -scraperfile        自定义刮削器文件路径
  -scrapers           活动刮削器组（默认：all）
  -se                 在偶发错误时停止（默认：false）
  -search             在ffuf历史记录中搜索FFUFHASH负载
  -sf                 当> 95％的响应返回403 Forbidden时停止（默认：false）
  -t                  并发线程数（默认：40）
  -v                  详细输出，带有完整的URL和重定向位置（如果有的话）以及结果。 （默认：false）

匹配器选项MATCHER OPTIONS:
  -mc                 匹配HTTP状态码，或者匹配"all"以匹配一切。 （默认：200-299,301,302,307,401,403,405,500）
  -ml                 匹配响应中的行数
  -mmode              匹配器集运算符。其中之一：and、or（默认：or）
  -mr                 匹配正则表达式
  -ms                 匹配HTTP响应大小
  -mt                 匹配第一个响应字节需要多少毫秒，要么大于要么小于。例如：>100或<100
  -mw                 匹配响应中的单词数

```

```markdown
过滤选项：
  -fc                 从响应中过滤HTTP状态码。逗号分隔的代码和范围列表
  -fl                 根据响应中的行数过滤。逗号分隔的行数和范围列表
  -fmode              过滤集合运算符。可以是：and、or（默认：or）
  -fr                 过滤正则表达式
  -fs                 过滤HTTP响应大小。逗号分隔的大小和范围列表
  -ft                 根据第一个响应字节的毫秒数过滤，可以是大于或小于。例如：>100或<100
  -fw                 根据响应中的单词数量过滤。逗号分隔的单词计数和范围列表

输入选项：
  -D                  DirSearch字典兼容模式。与-e标志一起使用。 （默认：false）
  -e                  逗号分隔的扩展列表。扩展FUZZ关键字。
  -enc                关键字的编码器，例如 'FUZZ:urlencode b64encode'
  -ic                 忽略字典注释（默认：false）
  -input-cmd          生成输入的命令。使用此输入方法时，--input-num是必需的。覆盖-w。
  -input-num          要测试的输入数量。与--input-cmd一起使用。 （默认：100）
  -input-shell        用于运行命令的Shell
  -mode               多字典操作模式。可用模式：clusterbomb，pitchfork，sniper（默认：clusterbomb）
  -request            包含原始http请求的文件
  -request-proto      与原始请求一起使用的协议（默认：https）
  -w                  字典文件路径和（可选的）由冒号分隔的关键字。例如。'/path/to/wordlist:KEYWORD'

输出选项：
  -debug-log          将所有内部日志写入指定的文件。
  -o                  将输出写入文件
  -od                 存储匹配结果的目录路径。
  -of                 输出文件格式。可用格式：json、ejson、html、md、csv、ecsv（或'all'以获取所有格式）（默认：json）
  -or                 如果没有结果，则不创建输出文件（默认：false）

示例用法：
  从wordlist.txt模糊文件路径，匹配所有响应，但过滤掉内容大小为42的响应。
  带有颜色的详细输出。
    ffuf -w wordlist.txt -u https://example.org/FUZZ -mc all -fs 42 -c -v

  模糊Host-header，匹配HTTP 200响应。
    ffuf -w hosts.txt -u https://example.org/ -H "Host: FUZZ" -mc 200

  模糊POST JSON数据。匹配所有不包含文本“error”的响应。
    ffuf -w entries.txt -u https://example.org/ -X POST -H "Content-Type: application/json" \
      -d '{"name": "FUZZ", "anotherkey": "anothervalue"}' -fr "error"

  模糊多个位置。仅匹配反映“VAL”关键字值的响应。带有颜色。
    ffuf -w params.txt:PARAM -w values.txt:VAL -u https://example.org/?PARAM=VAL -mr "VAL" -c

  更多信息和示例：https://github.com/ffuf/ffuf
```

### 交互模式

通过在ffuf执行期间按下 `ENTER` 键，进程将暂停，并将用户放入类似于Shell的交互模式：
```
进入交互模式
输入 "help" 获取命令列表，或按ENTER恢复。
> help

可用命令：
 afc  [value]             - 追加到状态码过滤器 
 fc   [value]             - (重新)配置状态码过滤器 
 afl  [value]             - 追加到行数过滤器 
 fl   [value]             - (重新)配置行数过滤器 
 afw  [value]             - 追加到单词数过滤器 
 fw   [value]             - (重新)配置单词数过滤器 
 afs  [value]             - 追加到大小过滤器 
 fs   [value]             - (重新)配置大小过滤器 
 aft  [value]             - 追加到时间过滤器 
 ft   [value]             - (重新)配置时间过滤器 
 rate [value]             - 调整每秒请求的速率（活动中：0）
 queueshow                - 显示作业队列
 queuedel [number]        - 删除队列中的作业
 queueskip                - 转到下一个排队的作业
 restart                  - 重新启动并恢复当前的ffuf作业
 resume                   - 恢复当前的ffuf作业（或：按ENTER键）
 show                     - 显示当前作业的结果
 savejson [filename]      - 将当前匹配保存到文件
 help                     - 就在这里
> 
```

在此模式下，可以重新配置过滤器、管理队列并将当前状态保存到磁盘。

在(重新)配置过滤器时，它们会事后应用，所有在内存中的误报匹配，即新添加的过滤器将被删除的匹配，都会被删除。

可以使用 `show` 命令打印匹配的新状态，该命令将打印出所有匹配，就像它们是通过 `ffuf` 发现的一样。

由于“负面”匹配未存储到内存中，放宽过滤器不幸地不能带回丢失的匹配。对于这种情况，用户可以使用 `restart` 命令，它会重置状态并从头开始执行当前的作业。

<p align="

center">
  <img width="250" src="_img/ffuf_waving_250.png">
</p>

## 许可证

ffuf采用MIT许可证发布。请参阅[LICENSE](https://github.com/ffuf/ffuf/blob/master/LICENSE)。
```