# http

## 一、http 状态码

状态码分类

- 1xx 服务器收到请求
- 2xx 请求成功
- 3xx 重定向
- 4xx 客户端错误
- 5xx 服务端错误

常用状态码

- 200 成功
- 301 永久重定向（配合 location（新的地址），浏览器自动跳转）
- 302 临时重定向（配合 location（新的地址），浏览器自动跳转）
- 304 资源未被修改（配合缓存，请求过再请求可能会返回）
- 403 没有权限
- 404 资源未找到
- 500 服务器错误
- 504 网关超时

## 二、http methods

传统的 methods

- get 获取服务器的数据
- post 向服务器提交数据

现在的 methods

- get 获取数据
- post 提交数据
- patch/put 更新数据
- delete 删除数据

`Restful API`

- 目前特别流行的 API 设计方法
- 传统的 API 设计:**把每一个 url 当做一个功能**
- Restful API 设计:**把每一个 url 当做唯一的资源**

如何设计成一个资源?

- 尽量不用 url 参数
- 用 method 表示操作类型

不使用 url 参数

- 传统的 API 设计: /api/list?page=2(获取第二页的数据)
- Restful API 设计: /api/list/2(唯一资源的唯一标识)

用 method 表示操作类型

- 传统的 API 设计

  - post 请求: /api/create-blog(创建一篇博客)
  - post 请求: /api/update-blog?id=100(更新第 100 篇的博客)
  - get 请求: /api/get-blog?id=100(获取第 100 篇的博客)

- Restful API 设计
  - post 请求: /api/blog(创建一篇博客)
  - patch: /api/blog/100(更新第 100 篇博客)
  - get 请求: /api/blog/100(获取第 100 篇博客)

## 三、http headers

常见的 Request Headers

- Accept 浏览器可以接收的数据格式
- Accept-Encoding 浏览器可接收的压缩算法,比如 gzip
- Accept-Languange 浏览器可接收的语言,比如 zh-CN
- Connection:keep-alive 一次 TCP 连接重复使用
- cookie
- Host
- User-Agent(简称 UA)浏览器信息
- Content-type 发送数据的格式,比如 application/json

常见的 Response Headers

- Content-type 返回数据的格式,比如 application/json
- Content-length 返回数据的大小,多少字节
- Content-Encoding 返回数据的压缩算法,比如 gzip
- Set-Cookie

## 四、http 缓存

什么是缓存？
