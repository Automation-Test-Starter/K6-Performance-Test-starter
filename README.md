<!-- markdownlint-disable MD041 -->
<!-- markdownlint-disable MD033 -->
<div align="right"><strong><a href="./README_ZH.md">🇨🇳中文</a></strong>  | <strong>🇬🇧English</strong></div>
<!-- markdownlint-disable MD041 -->
<!-- markdownlint-disable MD033 -->

# K6-Performance-Test-starter

A quick-start introductory document on performance testing with K6.

- [K6-Performance-Test-starter](#k6-performance-test-starter)
  - [Introduction of K6](#introduction-of-k6)
  - [Official website and documentation](#official-website-and-documentation)
  - [Installation](#installation)
    - [Installation on Mac systems](#installation-on-mac-systems)
    - [Windows installation](#windows-installation)
    - [Docker installation](#docker-installation)
    - [Installation on other systems](#installation-on-other-systems)
    - [Confirming a successful K6 installation](#confirming-a-successful-k6-installation)
  - [First k6 test script](#first-k6-test-script)
    - [Write the first test script](#write-the-first-test-script)
      - [Create a new K6 performance testing project directory and go to](#create-a-new-k6-performance-testing-project-directory-and-go-to)
      - [Create a file named `demo.js` for writing test scripts](#create-a-file-named-demojs-for-writing-test-scripts)
      - [Editing Test Scripts](#editing-test-scripts)
      - [Running the Test Script](#running-the-test-script)
      - [Check the test results](#check-the-test-results)
      - [Parsing demo test script](#parsing-demo-test-script)
  - [K6 common function](#k6-common-function)
    - [HTTP Requests](#http-requests)
      - [GET Request Example](#get-request-example)
      - [POST Request Example](#post-request-example)
      - [Supported HTTP Methods](#supported-http-methods)
      - [HTTP Request Tags](#http-request-tags)
    - [Metrics](#metrics)
      - [#### K6 Built-in Metrics](#-k6-built-in-metrics)
        - [Standard Built-in Metrics](#standard-built-in-metrics)
        - [HTTP 特定的内置指标](#http-特定的内置指标)
        - [其他内置指标](#其他内置指标)
      - [自定义指标](#自定义指标)
        - [自定义指标 demo 示例](#自定义指标-demo-示例)
          - [1.从导入 k6/metrics 模块引入 Trend 构造函数](#1从导入-k6metrics-模块引入-trend-构造函数)
          - [2.在 init 上下文中构造一个新的自定义度量 Trend 对象](#2在-init-上下文中构造一个新的自定义度量-trend-对象)
          - [3.在脚本中使用 add 方法记录指标测量值](#3在脚本中使用-add-方法记录指标测量值)
          - [4.demo\_custom\_metrics.js 自定义指标完整代码](#4demo_custom_metricsjs-自定义指标完整代码)
          - [5.运行 demo\_custom\_metrics.js 并查看自动化趋势指标](#5运行-demo_custom_metricsjs-并查看自动化趋势指标)
    - [Checks 检查](#checks-检查)
      - [1.检查 HTTP 响应状态](#1检查-http-响应状态)
      - [2.检查 HTTP 响应体](#2检查-http-响应体)
      - [3.添加多个检查](#3添加多个检查)
    - [Thresholds 阈值](#thresholds-阈值)
  - [References](#references)

## Introduction of K6

k6 is an open source tool for performance testing and load testing, primarily used to evaluate and validate the performance and stability of applications. Here are some key features and information about k6:

1. **Open Source:** k6 is a completely open source performance testing tool with code stored on GitHub. This means that users are free to access, use and modify the tool's source code.

2. **JavaScript scripting:** k6 uses the JavaScript language to write test scripts, which makes writing test cases relatively easy and more developer-friendly. Scripts can contain HTTP requests, WebSocket connections, script execution logic, and more.

3. **Support for multiple protocols:** k6 supports a variety of common protocols, including HTTP, WebSocket, Socket.IO, gRPC and so on, so it can be widely used in various types of applications. 4.

4. **Distributed Testing:** k6 has distributed testing capabilities, allowing tests to be run on multiple nodes to simulate a more realistic production environment load.

5. **Real-time results and reports:** k6 provides real-time results, including request response time, throughput, etc., and is able to generate detailed HTML reports to help users better understand the performance status of their applications.

6. **Containerization Support:** k6 adapts to containerized environments, can be easily integrated into CI/CD pipelines, and works with common container orchestration tools such as Kubernetes.

7. **Plugin ecosystem:** k6 supports plugins that allow users to extend its functionality to meet specific needs.

8. **Active Community:** Since k6 is an open source project, there is an active community that provides support, documentation, and examples to make it easier for users to get started and solve problems.

Overall, k6 is a flexible, powerful and easy-to-use performance testing tool for applications and systems of all sizes.

## Official website and documentation

- [Official website](https://k6.io/)
- [Official Documentation](https://k6.io/docs/)

## Installation

### Installation on Mac systems

Mac systems can install k6 via Homebrew:

```bash
brew install k6
```

### Windows installation

Windows systems can install k6 via Chocolatey:

```bash
choco install k6
```

Or you can install k6 via winget:

```bash
winget install k6
```

### Docker installation

k6 can also be installed via Docker:

```bash
docker pull grafana/k6
```

### Installation on other systems

In addition to the above systems, K6 also supports Linux (Debian/Ubuntu/Fedora/CentOS), and can be installed by downloading the K6 binaries and K6 extensions, please refer to the [official documentation](https://k6.io/docs/get-started/ For details on how to install K6, please refer to the official documentation ().

### Confirming a successful K6 installation

After the installation is complete, you can confirm that K6 has been installed successfully by using the following command:

```bash
k6 version
```

If the installation was successful, the k6 version information will be displayed:

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/QR8wKb.png)

## First k6 test script

### Write the first test script

#### Create a new K6 performance testing project directory and go to

```bash
mkdir k6-demo
cd k6-demo
```

#### Create a file named `demo.js` for writing test scripts

- A test script file can be created with the `k6 new` command:

```bash
k6 new demo.js
```

- You can also create a test script file called demo.js directly

```bash
touch demo.js
```

#### Editing Test Scripts

If the test script file is created with the `k6 new` command, a simple test script is automatically generated as shown below:

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export const options = {
  // A number specifying the number of VUs to run concurrently.
  vus: 10,
  // A string specifying the total duration of the test run.
  duration: '30s',

  // The following section contains configuration options for execution of this
  // test script in Grafana Cloud.
  //
  // See https://grafana.com/docs/grafana-cloud/k6/get-started/run-cloud-tests-from-the-cli/
  // to learn about authoring and running k6 test scripts in Grafana k6 Cloud.
  //
  // ext: {
  //   loadimpact: {
  //     // The ID of the project to which the test is assigned in the k6 Cloud UI.
  //     // By default tests are executed in default project.
  //     projectID: "",
  //     // The name of the test in the k6 Cloud UI.
  //     // Test runs with the same name will be grouped.
  //     name: "demo.js"
  //   }
  // },

  // Uncomment this section to enable the use of Browser API in your tests.
  //
  // See https://grafana.com/docs/k6/latest/using-k6-browser/running-browser-tests/ to learn more
  // about using Browser API in your test scripts.
  //
  // scenarios: {
  //   // The scenario name appears in the result summary, tags, and so on.
  //   // You can give the scenario any name, as long as each name in the script is unique.
  //   ui: {
  //     // Executor is a mandatory parameter for browser-based tests.
  //     // Shared iterations in this case tells k6 to reuse VUs to execute iterations.
  //     //
  //     // See https://grafana.com/docs/k6/latest/using-k6/scenarios/executors/ for other executor types.
  //     executor: 'shared-iterations',
  //     options: {
  //       browser: {
  //         // This is a mandatory parameter that instructs k6 to launch and
  //         // connect to a chromium-based browser, and use it to run UI-based
  //         // tests.
  //         type: 'chromium',
  //       },
  //     },
  //   },
  // }
};

// The function that defines VU logic.
//
// See https://grafana.com/docs/k6/latest/examples/get-started-with-k6/ to learn more
// about authoring k6 scripts.
//
export default function() {
  http.get('https://test.k6.io');
  sleep(1);
}
```

If the test script file was created directly, you can copy the above into the `demo.js` file.

#### Running the Test Script

In the directory where the `demo.js` file is located, run the following command:

```bash
k6 run demo.js
```

#### Check the test results

If all is well, you will see output similar to the following:

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/a4vK69.png)

Contains the following information:

- **execution:** execution information, including start time, end time, duration, number of VUs, number of iterations, etc.
- **scenarios:** Scenario information, including scenario name, number of VUs, number of iterations, duration, average response time, throughput, and so on.
- **http_reqs:** HTTP request information, including request name, number of requests, number of failures, average response time, throughput, and so on.

#### Parsing demo test script

- `import http from 'k6/http';`: import k6's HTTP module, used to send HTTP request.

- `import { sleep } from 'k6';`: Import k6's sleep method to wait for script execution.

- `export const options = { ... }`: Define the configuration items of the test script, including the number of VUs, duration, etc.

- `vus: 10,`: define the number of VUs to be 10 (specify the number of VUs running concurrently).

- `duration: '30s',`: define the duration as 30 seconds (specify the total duration of the test run).

- `export default function() { ... }`: defines the logic of the test script, including sending HTTP requests, executing waits, and so on.

- `http.get('https://test.k6.io');`: send a GET request to `https://test.k6.io`.

- `sleep(1);`: wait 1 second for execution.

> The other comments can be ignored, they are about some advanced features of k6, which will be introduced later.

## K6 common function

### HTTP Requests

The first step in performance testing with K6 is to define the HTTP requests to be tested.

#### GET Request Example

A simple HTTP request for the GET method is already included in the demo test script created with the `k6 new` command:

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export default function() {
  http.get('https://test.k6.io');
  sleep(1);
}
```

#### POST Request Example

This POST request example shows the application of some complex scenarios (POST request with email/password authentication load)

```javascript
import http from 'k6/http';

export default function () {
  const url = 'http://test.k6.io/login';
  const payload = JSON.stringify({
    email: 'aaa',
    password: 'bbb',
  });

  const params = {
    headers: {
      'Content-Type': 'application/json',
    },
  };

  http.post(url, payload, params);
}

```

> The above is taken from [K6 Official Documentation](https://k6.io/docs/using-k6/http-requests)

#### Supported HTTP Methods

The HTTP module provided by K6 can handle various HTTP requests and methods. The following is a list of supported HTTP methods:

| NAME | VALUE |
| ------- | ------- |
| batch() | Send multiple HTTP requests in parallel (like e.g. browsers tend to do).|
| del() | Send an HTTP DELETE request.|
| get() | Send an HTTP GET request.|
| head()|  Send an HTTP HEAD request.|
| options()|  Send an HTTP OPTIONS request.|
| patch()|  Send an HTTP PATCH request.|
| post()|  Send an HTTP POST request.|
| put() | Send an HTTP PUT request.|
| request() | Send any type of HTTP request.|

#### HTTP Request Tags

K6 allows you to add tags to each HTTP request. Combining tags and grouping makes it easy to better organize in test results, group requests and filter results to organize analysis.

The following is a list of supported tags:

| NAME | DESCRIPTION |
| ------- | ------- |
| expected_response | By default, response statuses between 200 and 399 are true. Change the default behavior with `setResponseCallback`.|
| group | When the request runs inside a `group`, the tag value is the group name. Default is empty.|
| name | Defaults to URL requested|
| method | Request method (GET, POST, PUT etc.)|
| scenario|  When the request runs inside a `scenario`, the tag value is the scenario name. Default is `default`.|
| status|  response status|
| url | defaults to URL requested|

Examples of HTTP requests using tag and group tags will be shown in subsequent demos.

You can also refer to the official examples:[https://grafana.com/docs/k6/latest/using-k6/http-requests/](https://grafana.com/docs/k6/latest/using-k6/http-requests/)

### Metrics

The metrics are used to measure the performance of the system under test conditions. By default, k6 automatically collects built-in metrics. In addition to the built-in metrics, you can create custom metrics.

Metrics generally fall into four categories:

1. Counters: Summing values.
2. Gauges: Tracking the smallest, largest, and most recent values.
3. Rates: Tracking how often non-zero values occur.
4. Trends: Calculating statistical information (such as mean, mode, or percentile) for multiple values.

To ensure that test assertions meet the criteria, thresholds can be written based on the conditions of the metrics required by the performance test (the specifics of the expression depend on the type of metric).

For subsequent filtering of metrics, labels and groupings can be used, allowing for better organization of test results.

> The test results output file can export metrics in a variety of summary and fine-grained formats. For more information, refer to the results output documentation. (This section will be covered in more detail in the later part of the test results output documentation.)

#### #### K6 Built-in Metrics

Every k6 test execution emits both built-in and custom metrics. Each supported protocol also has its specific metrics.

##### Standard Built-in Metrics

Regardless of the protocol used in the test, k6 always collects the following metrics:

| 指标名称 | 指标分类 | 指标描述 |
| ------- | ------- | -------|
| vus| Gauge |当前活跃虚拟用户数 |
| vus_max | Gauge | 最大可能虚拟用户数（VU 资源已预先分配，以避免扩大负载时影响性能）|
|iterations 迭代|Counter |VU 执行 JS 脚本（default 函数）的总次数。|
|iteration_duration|Trend|完成一次完整迭代的时间，包括在 setup 和 teardown 中花费的时间。要计算特定场景的迭代函数的持续时间，请尝试此解决方法|
|dropped_iterations|Counter|由于缺少 VU（对于到达率执行程序）或时间不足（基于迭代的执行程序中的 maxDuration 已过期）而未启动的迭代次数。关于删除迭代|
|data_received|Counter|接收到的数据量。此示例介绍如何跟踪单个 URL 的数据。|
|data_sent |Counter|发送的数据量。跟踪单个 URL 的数据以跟踪单个 URL 的数据。|
|checks|Rate|设置的检查成功率。|

> 指标分类分别为：计数器（Counter）、计量器（Gauges）、比率（Rates）、趋势（Trends）

##### HTTP 特定的内置指标

HTTP 特定的内置指标是仅在 HTTP 请求期间才会生成和收集的指标。其他类型的请求（例如 WebSocket）不会生成这些指标。

> 注意：对于所有 http_req_* 指标，时间戳在请求结束时发出。换句话说，时间戳发生在 k6 收到响应正文末尾或请求超时时。

下表列出了 HTTP 特定的内置指标：

| 指标名称 | 指标分类 | 指标描述 |
| ------- | ------- | -------|
|http_reqs|Counter|k6 总共生成了多少个 HTTP 请求。|
|http_req_blocked|Trend|在发起请求之前阻塞（等待空闲 TCP 连接槽）所花费的时间。`float`类型|
|http_req_connecting|Trend |与远程主机建立 TCP 连接所花费的时间。`float`类型|
|http_req_tls_handshaking|Trend|与远程主机握手 TLS 会话所花费的时间|
|http_req_sending|Trend|向远程主机发送数据所花费的时间。`float`类型|
|http_req_waiting|Trend|等待远程主机响应所花费的时间（也称为“第一个字节的时间”或“TTFB”）。`float`类型|
|http_req_receiving|Trend|从远程主机接收响应数据所花费的时间。`float`类型|
|http_req_duration|Trend|请求的总时间。它等于 http_req_sending + http_req_waiting + http_req_receiving（即远程服务器处理请求和响应所需的时间，没有初始 DNS 查找/连接时间）。`float`类型|
|http_req_failed|Rate|根据 setResponseCallback 的失败请求率。|

> 指标分类分别为：计数器（Counter）、计量器（Gauges）、比率（Rates）、趋势（Trends）

##### 其他内置指标

K6 内置指标除了标准内置指标和 HTTP 特定的内置指标外，还有其他内置指标：

- Browser metrics 浏览器指标：<https://grafana.com/docs/k6/latest/using-k6/metrics/reference/#browser>
- Built-in WebSocket metrics 内置 WebSocket 指标：<https://grafana.com/docs/k6/latest/using-k6/metrics/reference/#websockets>
- Built-in gRPC metrics 内置 gRPC 指标：<https://grafana.com/docs/k6/latest/using-k6/metrics/reference/#grpc>

#### 自定义指标

除了系统内建的指标之外，您还可以创建自定义指标。例如，您可以计算与业务逻辑相关的指标，或者利用 Response.timings 对象为特定的一组端点创建指标。

每种指标类型都有一个构造函数，用于生成自定义指标。该构造函数会生成一个声明类型的指标对象。每种类型都有一个 add 方法，用于记录指标测量值。

> 注意：必须在 init 上下文中创建自定义指标。这会限制内存并确保 K6 可以验证所有阈值是否评估了定义的指标。

##### 自定义指标 demo 示例

以下示例演示如何创建等待时间的自定义趋势指标：

> 项目文件中的 demo_custom_metrics.js 文件已经包含了这个 demo 示例，可以直接运行查看结果。

###### 1.从导入 k6/metrics 模块引入 Trend 构造函数

```javascript
import { Trend } from 'k6/metrics';
```

> 等待时间趋势指标属于趋势（Trends）指标，所以需要从 k6/metrics 模块引入 Trend 构造函数。

###### 2.在 init 上下文中构造一个新的自定义度量 Trend 对象

```javascript
const myTrend = new Trend('waiting_time');
```

> 在 init 上下文中构造一个新的自定义度量 Trend 对象，脚本中的对象为 myTrend，其指标在结果输出中显示为 `waiting_time`。

###### 3.在脚本中使用 add 方法记录指标测量值

```javascript
export default function() {
  const res = http.get('https://test.k6.io');
  myTrend.add(res.timings.waiting);
}
```

> 在脚本中使用 add 方法记录指标测量值，这里使用了 `res.timings.waiting`，即等待时间。

###### 4.demo_custom_metrics.js 自定义指标完整代码

```javascript
import http from 'k6/http';
import { Trend } from 'k6/metrics';

const myTrend = new Trend('waiting_time');

export default function () {
  const res = http.get('https://httpbin.test.k6.io');
  myTrend.add(res.timings.waiting);
  console.log(myTrend.name); // waiting_time
}
```

###### 5.运行 demo_custom_metrics.js 并查看自动化趋势指标

```bash
k6 run demo_custom_metrics.js
```

运行结果如下：

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/4tbqVc.png)

> 可以看到，自定义指标 `waiting_time` 已经在结果输出中显示出来了。

更多关于自定义指标的内容，请参考官方文档：[https://k6.io/docs/using-k6/metrics/#custom-metrics](https://k6.io/docs/using-k6/metrics/#custom-metrics)

### Checks 检查

> 这里也可以理解为断言，即对测试结果进行验证。

检查用来检验不同测试中的具体测试条件是否正确相应，和我们常规在做其他类型测试时也会对测试结果进行验证，以确保系统是否以期望的内容作出响应。

例如，一个验证可以确保 POST 请求的响应状态为 201，或者响应体的大小是否符合预期。

检查类似于许多测试框架中称为断言的概念，但是**K6 在验证失败并不会中止测试或以失败状态结束。相反，k6 会在测试继续运行时追踪失败验证的比率**。

> 每个检查都创建一个速率指标。要使检查中止或导致测试失败，可以将其与阈值结合使用。

下面会介绍如何使用不同类型的检查，以及如何在测试结果中查看检查结果。

#### 1.检查 HTTP 响应状态

K6 的检查非常适用于与 HTTP 请求相关的响应断言。

示例，以下代码片段来检查 HTTP 响应代码为 200：

```javascript
import { check } from 'k6';
import http from 'k6/http';


export default function () {
  const res = http.get('https://httpbin.test.k6.io');
  check(res, {
    'HTTP response code is status 200': (r) => r.status === 200,
  });
}
```

运行该脚本，可以看到如下结果：

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/aTXnpy.png)

> 当脚本包含检查时，摘要报告会显示通过了多少测试检查。

在此示例中，请注意检查“HTTP response code is status 200”在调用时是 100% 成功的。

#### 2.检查 HTTP 响应体

除了检查 HTTP 响应状态外，还可以检查 HTTP 响应体。

示例，以下代码片段来检查 HTTP 响应体大小为 9591 bytes：

```javascript
import { check } from 'k6';
import http from 'k6/http';


export default function () {
  const res = http.get('https://httpbin.test.k6.io');
  check(res, {
    'HTTP response body size is 9591 bytes': (r) => r.body.length == 9591,
  });
}
```

运行该脚本，可以看到如下结果：

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/AmbL0E.png)

> 当脚本包含检查时，摘要报告会显示通过了多少测试检查。

在此示例中，请注意检查“HTTP response body size is 9591 bytes”在调用时是 100% 成功的。

#### 3.添加多个检查

有时候我们在一个测试脚本中需要添加多个检查，那可以直接在单​​个 check() 语句中添加多个检查，如下面脚本所示：

```javascript
import { check } from 'k6';
import http from 'k6/http';


export default function () {
  const res = http.get('https://httpbin.test.k6.io');
  check(res, {
    'HTTP response code is status 200': (r) => r.status === 200,
    'HTTP response body size is 9591 bytes': (r) => r.body.length == 9591,
  });
}
```

运行该脚本，可以看到如下结果：

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/5yJyBw.png)

在此示例中，两个检查都是正常通过的（调用是 100% 成功的）。

> 注意：当检查失败时，脚本将继续成功执行，并且不会返回“失败”退出状态。如果您需要根据检查结果使整个测试失败，则必须将检查与阈值结合起来。这在特定环境中特别有用，例如将 k6 集成到 CI 管道中或在安排性能测试时接收警报。

### Thresholds 阈值

## References

- [Official K6 documentation: https://k6.io/docs/](https://k6.io/docs/)
- [Official website: https://k6.io/](https://k6.io/)
