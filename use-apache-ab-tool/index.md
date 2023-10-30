# 使用 Apache ab 工具测试网站性能


## Apache ab 介绍
Apache ab（Apache Bench）是一个用于进行 HTTP/HTTPS 性能测试的工具，它是 Apache HTTP 服务器的一部分。下面是对 Apache ab 工具的评价：

### 优点：
1. 简单易用：Apache ab 工具提供了简单的命令行接口，易于使用和配置。它不需要复杂的设置就可以进行基本的性能测试。

2. 轻量级：Apache ab 是一个轻量级的工具，安装和部署都非常简单，不需要过多的资源或依赖。

3. 高并发能力：Apache ab 可以模拟大量的并发请求，测试目标服务器在高负载情况下的性能表现。

4. 支持多种指标和统计数据：Apache ab 提供了多种测试结果的指标和统计数据，如请求速率、响应时间、吞吐量等，可以帮助分析和评估应用程序的性能。

5. 开源和免费：Apache ab 是开源的工具，可以免费使用和修改。
  
### 缺点：

1. 缺乏图形界面和可视化：Apache ab 是一个命令行工具，没有图形界面和可视化功能，对于非技术人员来说可能不够友好。

2. 功能较为有限：相对于其他性能测试工具，Apache ab 的功能相对简单，只能进行基本的负载测试，不支持复杂的场景和协议。

3. 对于复杂的应用程序可能不够准确：由于 Apache ab 的简单性，它可能无法准确模拟复杂的应用程序行为和场景，因此在某些情况下，测试结果可能不够准确。

## 下载

[Windows 版下载](https://httpd.apache.org/docs/current/platform/windows.html#down)

[Linux 版下载](https://httpd.apache.org/download.cgi#apache24)

## 安装
  
解压，通过命令行终端，进入文件目录（Apache/bin），执行。

```bash
ab -V
```

如图下显示，表示安装成功。

![安装成功](https://image.ericzzz.com/2023/10/24/6de21458-7866-4c15-9ff6-a68e57328b41.png)

## 使用

### 命令格式

```bash
ab [options] [http://]hostname[:port]/path
```

### 常用参数

- -n 总请求数量

- -c 并发用户数量

### 测试指标

- Server Software：被测试的 Web 服务器软件名称。

- Server Hostname：请求的 URL 中的主机部分名称。

- Server Port：被测试的 Web 服务器软件的监听端口。

- Document Path：请求的 URL 中的根绝对路径。

- Document Length：HTTP 响应数据的正文长度。

- Concurrency Level：并发用户数。

- Time taken for tests：所有这些请求被处理完成所花费的总时间。

- Complete requests：总请求数。

- Failed requests：失败的请求数。这里的失败是指请求在连接服务器、发送数据、接收数据等环节发生异常，以及无响应后超时的情况。对于超时时间的设置，可以使用 ab 的-t 参数。如果接收到的 HTTP 响应数据的头信息中含有 2xx 以外的状态码，则会在测试结果中显示另一个名为“Non-2xx requests”的统计项，用于统计这部分请求数，这些请求并不算是失败的请求。

- Total transferred：所有请求的响应数据长度总和，包括每个 HTTP 响应数据的头信息和正文数据的长度。注意，这里不包括 HTTP 请求数据的长度，所以 Total transferred 代表了从 Web 服务器流向用户 PC 的应用层数据总长度。通过使用 ab 的-v 参数，即可查看详细的 HTTP 头信息。

- HTML transferred：所有请求的响应数据中正文数据的总和，也就是减去了 Total transferred 中 HTTP 响应数据中头信息的长度。

- Request per second：**吞吐率。** 等于（Complete requests/Time taken for tests）

- Time per request：用户平均**请求等待时间。**

- Time per request (across all concurrent requests)：服务器平均**请求处理时间。**

- Transfer rate：这些请求在单位时间内从服务器获取的数据长度。**这个统计项可以很好地说明服务器在处理能力达到极限时，其出口带宽的需求量。**

- Percentage of the requests served within a certain time (ms)：用于描述每个请求处理时间的分布情况。

## 实战

使用我司测试服接口，不动态查询（不计算，不查询数据库），纯测试接口响应、吞吐量，请求总量为 1000。

### 对于不同并发用户数的吞吐率测试结果

| 并发用户数 | 吞吐率（reqs/s） | 请求等待时间（ms） | 请求处理时间（ms） |
|---|---|---|---|
| 1 | 7.9 | 126.566 | 126.566 |
| 2 | 15.76 | 126.894 | 66.447 |
| 5 | 19.90 | 251.301 | 50.260 |
| 10 | 19.87 | 503.215 | 50.321 |
| 20 | 19.91 | 1004.449 | 50.222 |
| 50 | 19.91 | 2511.152 | 50.223 |
| 100 | 19.91 | 5023.291 | 50.223 |
| 150 | 19.87 | 7547.763 | 50.318 |
| 200 | 19.79 | 10107.067 | 50.535 |
| 500 | 19.83 | 25214.845 | 50.430 |

### 吞吐率随并发用户数变化的曲线图

![吞吐率随并发用户数变化的曲线图](https://image.ericzzz.com/2023/10/24/bd49f3e3-49ce-4ce2-bd12-c4b3a6a980c3.png)

### 服务器平均请求处理时间随并发用户数变化的曲线图

![服务器平均请求处理时间随并发用户数变化的曲线图](https://image.ericzzz.com/2023/10/24/2959413e-2672-48e5-b0f0-57ab3dab7413.png)

### 用户平均请求等待时间随并发用户数变化的曲线图

![用户平均请求等待时间随并发用户数变化的曲线图](https://image.ericzzz.com/2023/10/24/f1628067-a81a-499b-951a-45b7abf6f433.png)

### 结论

在并发用户增加时，前期吞吐量有一定的增长，平均处理时间减少，后面趋于稳定，不再变化。当并发用户超过 50，平均用户等待时间长达 2s，对于用户是无法忍受的，500 用户并发，等待时间恐怖的达到了 20s 以上😱。

做为开发人员，设计、开发和提升产品用户体验，不仅是能力与技术的体现，而且是成为一名优秀的架构师迈不过去的坎，如何发现网站性能瓶颈，是重中之重，Apache ab 是一个简单易用、轻量级的性能测试工具，适用于进行基本的负载测试和性能评估，它可以提供一些有用的测试结果和指标。