<!-- markdownlint-disable MD041 -->
<!-- markdownlint-disable MD033 -->
<div align="right"><strong>🇨🇳中文</a></strong>  | <strong><a href="./README.md">🇬🇧English</strong></div>
<!-- markdownlint-disable MD041 -->
<!-- markdownlint-disable MD033 -->

# K6 性能测试快速启动项目

一个关于使用 K6 进行性能测试的快速启动介绍性文档。

- [K6 性能测试快速启动项目](#k6-性能测试快速启动项目)
  - [什么是 K6](#什么是-k6)
  - [官方网站及文档](#官方网站及文档)
  - [安装](#安装)
    - [Mac 系统安装](#mac-系统安装)
    - [Windows 系统安装](#windows-系统安装)
    - [Docker 安装](#docker-安装)
    - [其他系统安装](#其他系统安装)
    - [确认 K6 安装成功](#确认-k6-安装成功)
  - [第一个 K6 测试脚本](#第一个-k6-测试脚本)
    - [编写第一个测试脚本](#编写第一个测试脚本)
      - [新建一个 K6 性能测试项目目录并进入](#新建一个-k6-性能测试项目目录并进入)
      - [创建一个名为 `demo.js` 的文件，用于编写测试脚本](#创建一个名为-demojs-的文件用于编写测试脚本)
      - [编辑测试脚本](#编辑测试脚本)
      - [运行测试脚本](#运行测试脚本)
      - [查看测试结果](#查看测试结果)
      - [解析 demo 测试脚本](#解析-demo-测试脚本)
  - [K6 常用功能](#k6-常用功能)
    - [HTTP Requests](#http-requests)
      - [GET 请求例子](#get-请求例子)
      - [POST 请求例子](#post-请求例子)
      - [支持的 HTTP 方法](#支持的-http-方法)
      - [HTTP 请求标签](#http-请求标签)
    - [Metrics 指标](#metrics-指标)
      - [K6 内置指标](#k6-内置指标)
        - [标准内置指标](#标准内置指标)
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
  - [参考资料](#参考资料)

## 什么是 K6

k6 是一款用于性能测试和负载测试的开源工具，主要用于评估和验证应用程序的性能和稳定性。以下是关于 k6 的一些主要特点和信息：

1. **开源性：** k6 是一款完全开源的性能测试工具，代码存储在 GitHub 上。这意味着用户可以自由访问、使用和修改工具的源代码。

2. **JavaScript 编写脚本：** k6 使用 JavaScript 语言编写测试脚本，这使得编写测试用例相对简单，并且对于开发人员而言更加友好。脚本可以包含 HTTP 请求、WebSocket 连接、脚本执行逻辑等。

3. **支持多种协议：** k6 支持多种常见的协议，包括 HTTP、WebSocket、Socket.IO、gRPC 等，使其可以广泛应用于各种类型的应用程序。

4. **分布式测试：** k6 具有分布式测试的能力，允许在多个节点上运行测试，从而模拟更真实的生产环境负载。

5. **实时结果和报告：** k6 提供实时结果，包括请求响应时间、吞吐量等，并能够生成详细的 HTML 报告，帮助用户更好地理解应用程序的性能状况。

6. **容器化支持：** k6 适应容器化环境，可以轻松集成到 CI/CD 流水线中，并与常见的容器编排工具（如 Kubernetes）配合使用。

7. **插件生态系统：** k6 支持插件，用户可以通过插件扩展其功能，满足特定需求。

8. **活跃的社区：** 由于 k6 是一个开源项目，拥有一个积极的社区，提供支持、文档和示例，使用户更容易上手和解决问题。

总体而言，k6 是一个灵活、强大且易于使用的性能测试工具，适用于各种规模的应用程序和系统。

## 官方网站及文档

- [官方网站](https://k6.io/)
- [官方文档](https://k6.io/docs/)

## 安装

### Mac 系统安装

Mac 系统可以通过 Homebrew 安装 k6：

```bash
brew install k6
```

### Windows 系统安装

Windows 系统可以通过 Chocolatey 安装 k6：

```bash
choco install k6
```

或者通过 winget 安装 k6：

```bash
winget install k6
```

### Docker 安装

k6 也可以通过 Docker 安装：

```bash
docker pull grafana/k6
```

### 其他系统安装

K6 除了支持上述系统外，还支持 Linux（Debian/Ubuntu/Fedora/CentOS），也支持下载 K6 二进制文件和 K6 扩展进行安装，具体安装方式请参考[官方文档](https://k6.io/docs/get-started/installation)。

### 确认 K6 安装成功

安装完成后，可以通过以下命令确认 k6 是否安装成功：

```bash
k6 version
```

如果安装成功，会显示 k6 的版本信息：

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/QR8wKb.png)

## 第一个 K6 测试脚本

### 编写第一个测试脚本

#### 新建一个 K6 性能测试项目目录并进入

```bash
mkdir k6-demo
cd k6-demo
```

#### 创建一个名为 `demo.js` 的文件，用于编写测试脚本

- 可以通过 `k6 new` 命令创建一个测试脚本文件：

```bash
k6 new demo.js
```

- 也可以直接创建一个名为 demo.js 的测试脚本文件

```bash
touch demo.js
```

#### 编辑测试脚本

如果是通过 `k6 new` 命令创建的测试脚本文件，会自动生成一个简单的测试脚本，如下所示：

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

如果是直接创建的测试脚本文件，可以将上述内容复制到 `demo.js` 文件中。

#### 运行测试脚本

在 `demo.js` 文件所在目录下，运行以下命令：

```bash
k6 run demo.js
```

#### 查看测试结果

如果一切正常，会看到类似如下的输出：

![ ](https://cdn.jsdelivr.net/gh/naodeng/blogimg@master/uPic/a4vK69.png)

包含以下信息：

- **execution:** 执行信息，包括开始时间、结束时间、持续时间、VU 数量、迭代次数等。
- **scenarios:** 场景信息，包括场景名称、VU 数量、迭代次数、持续时间、平均响应时间、吞吐量等。
- **http_reqs:** HTTP 请求信息，包括请求名称、请求数量、失败数量、平均响应时间、吞吐量等。

#### 解析 demo 测试脚本

- `import http from 'k6/http';`：导入 k6 的 HTTP 模块，用于发送 HTTP 请求。

- `import { sleep } from 'k6';`：导入 k6 的 sleep 方法，用于执行脚本等待。

- `export const options = { ... }`：定义测试脚本的配置项，包括 VU 数量、持续时间等。

- `vus: 10,`：定义 VU 数量为 10（指定并发运行的 VU 数量）。

- `duration: '30s',`：定义持续时间为 30 秒（指定测试运行总持续时间）。

- `export default function() { ... }`：定义测试脚本的逻辑，包括发送 HTTP 请求、执行等待等。

- `http.get('https://test.k6.io');`：发送一个 GET 请求到 `https://test.k6.io`。

- `sleep(1);`：执行等待 1 秒。

> 其他注释内容可以忽略，这些内容是关于 k6 的一些高级功能，后续会介绍。

## K6 常用功能

### HTTP Requests

使用 K6 进行性能测试的第一步就是定义要测试的 HTTP 请求。

#### GET 请求例子

使用 `k6 new` 命令创建的 demo 测试脚本中，已经包含了一个简单的 GET 方法 HTTP 请求：

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export default function() {
  http.get('https://test.k6.io');
  sleep(1);
}
```

#### POST 请求例子

这个 POST 请求例子展示一些复杂的场景的应用（带有电子邮件/密码身份验证负载的 POST 请求）

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

> 以上内容参考自 [K6 官方文档](https://k6.io/docs/using-k6/http-requests)

#### 支持的 HTTP 方法

K6 提供的 HTTP 模块能处理各种 HTTP 请求和方法。以下是支持的 HTTP 方法列表：

| 方法 | 作用 |
| ------- | ------- |
| batch()| 并行发出多个 HTTP 请求（例如浏览器往往会这样做）。|
| del() | 发出 HTTP DELETE 请求。|
| get() | 发出 HTTP GET 请求。|
| head() | 发出 HTTP HEAD 请求。|
| options() | 发出 HTTP OPTIONS 请求。|
| patch() | 发出 HTTP PATCH 请求。|
| post() | 发出 HTTP POST 请求。|
| put() | 发出 HTTP PUT 请求。|
| request() | 发出任何类型的 HTTP 请求。|

#### HTTP 请求标签

K6 允许为每个 HTTP 请求添加标签，结合标签和分组，可以很方便的在测试结果中更好地组织，分组请求和过滤结果组织分析。

以下为支持的标签列表：

| 标签 | 作用 |
| ------- | ------- |
| name | 请求名称。默认为请求的 URL|
| method | 请求方法（GET、POST、PUT 等）|
| url | 默认为请求的 URL。|
| expected_response | 默认情况下，200 到 399 之间的响应状态为 true。使用 setResponseCallback 更改默认行为。|
| group | 当请求在组内运行时，标记值是组名称。默认为空。|
| scenario | 当请求在场景内运行时，标记值是场景名称。默认为 default。|
| status | 响应状态|

HTTP 请求使用 tag 和 group 标签的例子会在后续的 demo 中展示。

大家也可以参考官方的例子：[https://grafana.com/docs/k6/latest/using-k6/http-requests/](https://grafana.com/docs/k6/latest/using-k6/http-requests/)

### Metrics 指标

指标用于衡量系统在测试条件下的性能。默认情况下，k6 会自动收集内置指标。除了内置指标，您还可以创建自定义指标。

指标一般分为四大类：

1. 计数器（Counters）：对值求和。
2. 计量器（Gauges）：跟踪最小、最大和最新的值。
3. 比率（Rates）：跟踪非零值发生的频率。
4. 趋势（Trends）：计算多个值的统计信息（如均值、模式或百分位数）。

要使测试断言符合需求标准，可以根据性能测试要求的指标条件编写阈值（表达式的具体内容取决于指标类型）。

为了后续进行筛选指标，可以使用标签和分组，这样可以更好地组织测试结果。

> 测试结果输出文件可以以各种摘要和细粒度格式导出指标，具体信息请参阅结果输出文档。（后面测试结果输出文档会详细介绍这一部分）

#### K6 内置指标

每个 k6 测试执行都会发出内置和自定义指标。每个支持的协议也有其特定的指标。

##### 标准内置指标

无论测试使用什么协议，k6 始终收集以下指标：

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


## 参考资料

- [K6 官方文档：https://k6.io/docs/](https://k6.io/docs/)
- [官方网站：https://k6.io/](https://k6.io/)
