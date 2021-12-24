# Cache-Control:
max-age = 30 这个页面只能缓存30秒
no-store 不允许缓存，常见于秒杀页面
no-cache 可以缓存但必需要服务器验证是否过期
must-revalidate如果缓存不过其就可以继续使用，如果过期了还想用就必须取服务器验证

1. 是否允许缓存 -> no-store
2. 使用缓存钱必须验证? -> no-cache
3. 缓存失效后必须验证？ -> must-revalidate
4. 缓存最多X秒? -> max-age=x

服务器与客户端都会有缓存限制这叫做协商缓存，cache-control
当刷新和ctrl+f5强制刷新效果其实是一样的，
刷新浏览器会在请求头里加一个Cache-Control:max-age = 0,
强制刷新其实是发了一个Cache-Control：no-cache和max-age = 0差不多

在“前进”“后退”“跳转”这些重定向动作中浏览器不会“夹带私货”，只用最基本的请求头，
没有“Cache-Control”，所以就会检查缓存，直接利用之前的资源，不再进行网络通信。

# if-Modified-Since和if-None-Match   还有“If-Unmodified-Since”“If-Match”和“If-Range”
# if-Modified-Since的Last-modified 和 ETag

浏览器可以用两个连续的请求组成“验证动作”：先是一个HEAD，获取资源的修改时间等信息，然后与缓存数据比较，如果改动了就获取新的资源。
如果没有改动就使用缓存，节省网络流量。
两个请求的网络成本太高了，定义了一系列if开头的条件请求字段，在一个请求中加入该字段，服务器可以用这些字段来判断资源是否改动。

last-modified是最后修改时间，
etag是资源的一个唯一标识解决修改时间不发准确区分文件变化的问题。

一个文件在一秒内修改了多次，这时候就需要etag来进行标识。
强etag要求资源在字节级别必须完全相符。
弱etag在值前有个W/标记，要求资源在语义上没有变化，但内部可能会有部分发生了改变(例如HTML里的标签顺序调整，或者多了几个空格)。
每个web服务器对etag的计算方法都不一样，只要保证数据变化后值不一样就好，但复杂的计算会增加服务器的负担。nginx的算法是"修改时间+长度"，实际上和last-modified基本等价。


tips: 除了cache-control服务器也可以用expires来标记资源的有效期，它的形式和cookie的差不多，同样属于“过时”的属性，优先级低于“cache-control”。
强制刷新后请求头中 没有了 If-None-Match ，而且 Cache-Control: no-cache，服务器无法处理缓存，就只能返回最新的数据。


# Cache 和 Cookie的相同和异同
Cache 和 Cookie 的相同点是：都会保存到浏览器中，并可以设置过期时间。   
不同点：   
1. Cookie 会随请求报文发送到服务器，而 Cache 不会，但可能会携带 if-Modified-Since（保存资源的最后修改时间）和 If-None-Match（保存资源唯一标识） 字段来验证资源是否过期。   
2. Cookie 在浏览器可以通过脚本获取（如果 cookie 没有设置 HttpOnly），Cache 则无法在浏览器中获取（出于安全原因）。   
3. Cookie 通过响应报文的 Set-Cookie 字段获得，cache缓存的是完整的报文。   
4. 用途不同。Cookie 常用于身份识别，Cache 则是由浏览器管理，用于节省带宽和加快响应速度。   
5. Cookie 的 max-age 是从浏览器拿到响应报文时开始计算的，而 Cache 的 max-age 是从响应报文的生成时间（Date 头字段）开始计算。   

