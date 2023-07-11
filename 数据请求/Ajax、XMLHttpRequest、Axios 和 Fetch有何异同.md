# Ajax、XMLHttpRequest、Axios 和 Fetch：它们之间的关系与区别

## 关系图

为了更好地理解 Ajax、XMLHttpRequest、Axios 和 Fetch 之间的关系，我们先看下面的关系包含图。

![包含关系](../assets/image/browser%26net/ajax-relation.png)

XMLHttpRequest、Axios 和 Fetch 三个工具，它们都是基于 Ajax 概念实现数据的异步通信，同时 Ajax 也是他们的共同基础。

## 总结

更加直观地呈现 Ajax、XMLHttpRequest、Axios 和 Fetch 的区别，请看表格。
| 工具             | 定义                                           | 特点                                                                          | 区别                                                                                          |
| -------------- | -------------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Ajax           | 一种使用 JavaScript 进行异步数据交互的技术和概念。              | 前端通信的思想和模式，可以使用多种工具来实现。                                                     | 无法直接使用，需要借助其他工具或框架来实现。                                                                      |
| XMLHttpRequest | 由浏览器提供的原生 API，用于发送 HTTP 请求并接收响应。             | 支持多种请求方式（GET、POST、PUT、DELETE 等），对请求和响应进行详细的控制和定制。                           | 需要手动设置请求头信息，不能自动解析响应数据格式。                                                                   |
| Axios          | 基于 Promise 的 HTTP 客户端，可用于浏览器和 Node.js 等多种平台。 | 支持Promise API，支持全局axios配置，如默认的 URL、headers 等。提供了更简单、便捷的 API，并支持拦截器和响应转换等功能。 | 在浏览器中使用 XMLHttpRequest 对象发送请求和接收响应，在 Node.js 中使用 node-http 库发送请求和接收响应。可以拦截请求和响应，并对其进行判断和转换。 |
| Fetch          | 原生的API，用于替代 XMLHttpRequest。                  | 基于 Promise 的 API，内置了跨域资源共享（CORS）支持，使用更加简单明了，自带CORS支持。但需要注意兼容性问题。            | 不支持同步请求，API 使用了链式操作风格。                                                                      |

总结：以上工具的本质都是基于 Ajax 概念实现数据的异步通信。XMLHttpRequest 是较为原始的实现方式，Axios 是现代的、基于 Promise 的实现方式，Fetch 是最新的、用于替代 XMLHttpRequest 的实现方式，它们都有其各自特点和适用场景，需要根据实际需求来选择合适的工具。

## 结论

综上所述，XMLHttpRequest、Axios 和 Fetch 都是基于 Ajax 概念实现数据的异步通信。其中，XMLHttpRequest 具有一定的历史意义，它是最早实现 Ajax 的技术之一，也是前端开发中最常用的工具之一。Axios 是当前比较常用的工具之一，它提供了丰富的 API，并且可以同时在客户端和服务端使用。Fetch 是最新的实现方式，它依赖于 Promise，使用起来更加简洁，但浏览器兼容性一般。


## 参考资料

*   [XMLHttpRequest - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
*   [Axios - Promise based HTTP client for the browser and node.js](https://github.com/axios/axios)
*   [Fetch - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
