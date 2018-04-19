# ASP.NET Core 2.0 Web API

针对ASP .Net Core的一些重要概念进行阐述。

## 1.Request Pipeline & Middleware

- Request Pipeline: 那些处理http requests并返回responses的代码就组成了request pipeline(请求管道).
- Middleware: 我们可以做的就是使用一些程序来配置那些请求管道 request pipeline以便处理requests和responses. 比如处理验证(authentication)的程序

## 2.Controller

原来的.net使用asp.net web api 和 asp.net mvc 分别来创建 web api和mvc项目. 但是 asp.net core mvc把它们整合到了一起.

## 3.Routing(路由)

1. Convention-based (按约定)：主要用于MVC (返回View或者Razor Page那种的).
2. Attribute-based(基于路由属性配置的，包含Route以及Http[verbs]—HttpGet, HttpPost等)
    - Web api 推荐
    - 基于属性配置的路由可以配置Controller或者Action级别, uri会根据Http method然后被匹配到一个controller里具体的action上.
    - Route用于Controller层，可以控制Action级的URI前缀
        - 使用[Route("api/[controller]")], 它使得整个Controller下面所有action的uri前缀变成了"/api/product", 其中[controller]表示XxxController.cs中的Xxx(其实是小写).

        - 也可以具体指定, [Route("api/product")], 这样做的好处是, 如果ProductController重构以后改名了, 只要不改Route里面的内容, 那么请求的地址不会发生变化.

        - GetProducts方法上面, 写上HttpGet, 也可以写HttpGet(). 它里面还可以加参数,例如: HttpGet("all"), 那么这个Action的请求的地址就变成了 "/api/product/All".

        - 如果GetProduct是获取单个Product，HttpGet参数里面{id}表示该action有一个参数名字是id. 这个action的地址是: "/api/product/{id}"

## 4.Status Code

http status code 是reponse的一部分, 它提供了这些信息: 请求是否成功, 失败的原因. web api 能涉及到的status codes主要是这些:
`200`: OK

`201`: Created, 创建了新的资源

`204`: 无内容 No Content, 例如删除成功

`400`: Bad Request, 指的是客户端的请求错误

`401`: 未授权 Unauthorized

`404`: Not Found

`500`: Internal Server Error, 服务器发生了错误.

- 因为web api不一定返回的都是json类型的数据, 也不一定只返回一堆json(可能还要包含其他内容). 所以JsonResult并不合适作为Action的返回结果.如
- 我们想要返回数据和Status Code, 那么可以这样做，new jsonresult(value){statuscode=200}
- asp.net core 内置了很多方法都可以返回IActionResult, OK, Not Found, BadRequest等
- **Common.RESTAPI中添加了HttpRequest的Extension方法，添加了以上各种需求的ActionResult返回值。**

## 5.子资源

有时候, 两个model之间有主从关系, 会根据主model来查询子model.比方说Teams，Students,具体实现如下

## 6.返回值格式

- asp.net core 2.0 默认返回的结果格式是Json, 并使用json.net对结果默认做了camel case的转化(即首字母小写).
- 这一点与老.net web api 不一样, 原来的 asp.net web api 默认不适用任何NamingStrategy, 需要手动加上camelcase的转化.
- 如果非得把这个规则去掉, 那么就在configureServices里面改一下，可以做如下改动

## 7.Content Negotiation

- asp.net core 默认提供的是json格式, 也可以配置xml等格式
- 如果需要支持XML, Accept Header中设置Application/xml, 然后ConfigureServices中做如下添加

## 8.API Version

- Set Up
  - Install-Package Microsoft.AspNetCore.Mvc.Versioning
  - ConfigureServices中添加如下配置
  - SA目前采用URL Path Based Versioning

- URL Query Based Versioning
- URL Path Based Versioning
- Http Header Based Versioning

> 参考资料:
>
> <https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-2.1>
>
> <https://docs.microsoft.com/en-us/aspnet/core/web-api/action-return-types?view=aspnetcore-2.1>
>
> <https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1>
