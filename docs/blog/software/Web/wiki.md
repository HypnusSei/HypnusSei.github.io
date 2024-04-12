>intranet wiki

# Web API
关注的是前后端数据和服务的交互,指Web应用程序通过HTTP协议对外暴露的一系列接口，允许客户端（通常是Web应用或移动应用）通过发送HTTP请求获取数据或执行服务器端的功能。Web API可以由各种后端技术实现，如ASP.NET Web API、Node.js的Express框架、Spring Boot框架等。

在实际应用中，WebAssembly和Web API可能有以下联系：

- WebAssembly模块可以在Web应用中执行复杂的计算任务，而Web API则可以为WebAssembly模块提供数据。例如，WebAssembly模块可能负责高性能图像处理，而Web API则负责从服务器获取或发送需要处理的图像数据。
- WebAssembly模块可以调用JavaScript编写的API，这些API可以进一步与Web API交互，实现前后端的通信和数据交换。


# RESTful API

REST（Representational State Transfer）是一种架构风格，用于设计网络应用程序，它通过HTTP协议提供服务。与Web API类似，RESTful API也是通过HTTP方法（GET、POST、PUT、DELETE等）来操作资源，实现数据传输和功能调用。RESTful API可以使用多种编程语言实现，如Java（Spring Boot框架）、Python（Django、Flask框架）、Node.js（Express框架）等。
# GraphQL

GraphQL是一种查询语言和运行时环境，用于API接口的构建。与Web API不同，GraphQL提供了一种客户端指定数据需求的机制，允许客户端一次性获取所有需要的数据，避免了冗余和过度获取的问题。它可以用在多种后端技术之上。
# SOAP Web Services

SOAP（Simple Object Access Protocol）是一种基于XML的消息传递协议，用于在Web服务之间交换结构化信息。与RESTful API相比，SOAP服务通常具有更强的规范性和安全性，但其复杂性和较重的协议栈导致其在现代Web应用中逐渐被RESTful API取代。
# gRPC

gRPC是一个高性能、开源且通用的RPC（Remote Procedure Call）框架，由Google开发，支持多种语言环境。gRPC基于HTTP/2协议并通过Protocol Buffers作为接口描述和数据交换格式，提供强类型的服务定义、流式传输和双向通信等特性，性能优于传统的Web API。
# WebSocket

WebSocket提供了一种在单个TCP连接上进行全双工通信的协议，它允许服务端推送数据到客户端，而不仅仅是响应客户端请求。虽然它不是严格意义上的API实现方式，但在实时通信、聊天应用和游戏场景中，WebSocket可以替代或补充常规的Web API以实现双向通信功能。