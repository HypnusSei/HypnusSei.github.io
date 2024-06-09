## XAF
> MES开发
是否直接github更容易



## MVC

Controller起着核心的调度和逻辑处理作用: 接收来自用户的输入（通常是HTTP请求在Web应用中），处理这些输入（包括可能的数据操作和业务逻辑处理），并最终决定要呈现给用户的响应（通常是视图）。在Web开发框架如ASP.NET MVC或Spring MVC中，控制器是处理Web请求的关键组件。

1. 请求处理：接收HTTP请求并根据请求类型（GET, POST等）执行相应操作。
2. 业务逻辑：封装业务规则和数据处理逻辑。
3. 视图呈现：决定并准备数据以供视图层展示给用户。
4. 导航控制：决定应用内部的页面跳转或重定向。

依赖特性（Dependency Attributes），也称为特性（Attributes）或注解（Annotations）（在Java中），是一种元数据的形式，可以附加到类、方法、属性等程序元素上，用以改变或增强这些元素的行为。在Web开发上下文中，依赖特性常用于定义路由、处理HTTP方法、声明依赖注入等。它们是实现诸如路由映射、请求验证、安全控制等功能的重要手段。

1. 路由配置：如ASP.NET Core中的[Route]，用于定义URL路径模板。
2. HTTP方法约束：如[HttpGet], [HttpPost]等，指定方法应响应的HTTP请求类型。
3. 参数绑定：如[FromBody], [FromQuery]，指导参数从请求的特定部分提取。
4. 安全验证：如[Authorize]，控制访问权限。
5. 模型验证：辅助数据验证，确保数据完整性。
6. 依赖注入：如.NET中的[Service]或Java Spring框架的@Autowired，自动注入依赖服务。