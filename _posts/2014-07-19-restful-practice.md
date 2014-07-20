---
title: Restful 设计
layout: post
category : dev
tagline: restful 架构策略
tags : [web]
---

<h3 id="restful-api-">RESTful API 设计指南</h3>
<p>摘自:http://www.ruanyifeng.com/blog/2014/05/restful_api.html</p>

<h3 id="section">协议</h3>

<pre><code>只使用https协议
</code></pre>

<h3 id="section-1">使用专用域名并包含版本</h3>

<pre><code>https://api.example.com/v1  
</code></pre>

<h3 id="endpoint">资源Endpoint</h3>

<pre><code>https://api.example.com/v1/{resourceses}

例如:
https://api.example.com/v1/books
https://api.example.com/v1/users
</code></pre>

<h3 id="http">使用Http协议的动词表示执行方法</h3>

<p>常用的</p>

<pre><code>GET   （SELECT）：从服务器取出资源（一项或多项）。
POST  （CREATE）：在服务器新建一个资源。
PUT   （UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH （UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
</code></pre>

<p>*PUT适合资源明细修改操作 PATCH处理资源属性（例如：状态）变更</p>

<p>不常用的</p>

<pre><code>HEAD            ：获取资源的元数据。
OPTIONS         ：获取信息，关于资源的哪些属性是客户端可以改变的。
</code></pre>

<h3 id="apifiltering">Api参数（Filtering）</h3>
<p>由于系统数据增长，不能每次请求都全部返回，可以考虑增加如下参数</p>

<pre><code>?pagesize=10    ：每次请求返回记录的数量
?pageno=10      ：当前请求的页号。
?sortby=name    ：当前请求使用的排序属性
?order=asc      ：当前请求的排序顺序。asc desc
?{para}={value} : 当前资源的过滤参数 可以多个

例如:
GET https://api.example.com/v1/books?name=restful&amp;sortby=name&amp;order=desc&amp;pagesize=10&amp;pageno=10 
</code></pre>

<h3 id="http-1">Http状态码</h3>

<pre><code>200 OK                    - [GET]            ：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
201 CREATED               - [POST/PUT/PATCH] ：用户新建或修改数据成功。
204 NO CONTENT            - [DELETE]         ：用户删除数据成功。
400 INVALID REQUEST       - [POST/PUT/PATCH] ：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。。
404 NOT FOUND             - [*]              ：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
4XX ERROR                 - [*]              : 如果状态码是4xx，就应该向用户返回出错信息
500 INTERNAL SERVER ERROR - [*]              ：服务器发生错误，用户将无法判断发出的请求是否成功。
</code></pre>

<h3 id="section-2">返回结果</h3>

<pre><code>GET                       /resourceses       ：返回资源对象的列表（数组）
GET                       /resourceses/id    ：返回单个资源对象
POST                      /resourceses       ：返回新生成的资源对象
PUT                       /resourceses/id    ：返回完整的资源对象
PATCH                     /resourceses/id    ：返回完整的资源对象
DELETE                    /resourceses/id    ：返回一个空文档
</code></pre>

<h3 id="hypermedia-api">Hypermedia API</h3>

<p>RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。
比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。</p>

<pre><code>{"link": {
  "rel":   "collection https://www.example.com/zoos",
  "href":  "https://api.example.com/zoos",
  "title": "List of zoos",
  "type":  "application/vnd.yourformat+json"
}}
</code></pre>

<p>上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。Hypermedia API的设计被称为HATEOAS。Github的API就是这种设计，访问api.github.com会得到一个所有可用API的网址列表。</p>

<pre><code>{
  "current_user_url": "https://api.github.com/user",
  "authorizations_url": "https://api.github.com/authorizations",
  // ...
}
</code></pre>

<p>从上面可以看到，如果想获取当前用户的信息，应该去访问api.github.com/user，然后就得到了下面结果。</p>

<pre><code>{
  "message": "Requires authentication",
  "documentation_url": "https://developer.github.com/v3"
}
</code></pre>

<p>上面代码表示，服务器给出了提示信息，以及文档的网址。</p>

<h3 id="section-3">结果实例结构</h3>

<pre><code>{
    error:"",                                            ：错误为空为正常结果
    count:2,                                             ：全部资源数量合计 用于分页控件使用
    pagenum:1,                                           ：当前页码 用于分页控件使用
    pagecount:1,                                         ：全部页数合计 用于分页控件使用
    resourceses:                                         
    [
        {
            id=1,                                        ：资源属性
            name="xxx",                                  ：资源属性
            isbn="xxx-xxxx-xxxxxx",                      ：资源属性
            action:[                                     ：资源允许操作
                "PUT",                                   ：允许更新
                "DELETE"                                 ：允许删除
            ]
            uri:https://api.example.com/v1/books/1       ：当前资源定位地址
        },
        {
            id=2,
            name="yyy",
            isbn="yyy-yyyy-yyyyyy",
            action:[
                "PUT"
            ]
            uri:https://api.example.com/v1/books/2
        }
    ]
}

{
    error:"invalid auth"                                  ：错误不为空为异常结果
}
</code></pre>

<h3 id="section-4">其他信息</h3>
<p>因为Restful的stateless，API的身份认证应该使用类似OAuth的集中身份认证方式
<br>
服务端参考 </p>
<pre><code>
Java:

<li><a href="http://oltu.apache.org/">Apache Oltu</a>

</li><li><a href="http://static.springsource.org/spring-security/oauth/">Spring Security for OAuth</a>

</li><li><a href="https://github.com/OpenConextApps/apis">Apis Authorization Server (v2-31)</a>

</li><li><a href="http://www.restlet.org/">Restlet Framework (draft 30)</a>

</li><li><a href="http://cxf.apache.org/">Apache CXF</a>
</li>

PHP:

<li><a href="https://github.com/bshaffer/oauth2-server-php">PHP OAuth2 Server</a> and <a href="https://github.com/bshaffer/oauth2-demo-php">Demo</a></li>
<li><a href="https://github.com/thephpleague/oauth2-server">PHP OAuth 2.0 Auth and Resource Server</a> and <a href="https://github.com/lncd/oauth2-example-auth-server">Demo</a></li>
<li><a href="https://github.com/fkooman/php-oauth">PHP OAuth 2.0</a> (AS with SAML/BrowserID AuthN, with management REST API, see <a href="https://frko.surfnetlabs.nl/workshop/">DEMO</a>)</li>

Python:

<li><a href="https://github.com/StartTheShift/pyoauth2">Python OAuth 2.0 Provider</a> (see <a href="http://tech.shift.com/post/39516330935/implementing-a-python-oauth-2-0-provider-part-1">Tutorial</a>)</li>
<li><a href="https://github.com/idan/oauthlib">OAuthLib</a> (a generic implementation of the OAuth request-signing logic) is avaliable for <a href="https://github.com/evonove/django-oauth-toolkit">Django</a> and <a href="https://github.com/lepture/flask-oauthlib">Flask</a> web frameworks</li>

Nodejs:

<li><a href="https://github.com/t1msh/node-oauth20-provider">NodeJS OAuth 2.0 Provider</a></li>

Ruby:

<li><a href="https://github.com/nov/rack-oauth2">Ruby OAuth2 Server (draft 18)</a></li>

.Net:

<li><a href="http://www.dotnetopenauth.net/">.NET DotNetOpenAuth</a></li>

Erlang:

<li><a href="https://github.com/kivra/oauth2">Erlang Oauth2 Server framework</a></li>

</code></pre>

