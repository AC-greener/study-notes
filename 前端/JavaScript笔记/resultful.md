#### RESRful(representational state transfer)，也叫做表现层状态转化

- 设计哲学：主要讲服务器端提供的内容实体看作一个资源，并表现在url上面

- 过去： post    /user/add?username=jack， 现在post   /user/jack，用方法来描述对资源的操作，在请求中没有动词

- 过去： get /user/jack.json， 以前设计资源的格式与后缀有很大的关系，在resttful设计中，资源的具体格式由请求报文中的Accept字段和服务器端支持的情况来决定。比如客户端同时接受json和xml格式，那么他的Accpet字段如下：Accept: application/json, application/xml。 响应字段可以如下：Content-Type:application/json

  

  

  

  ##### 

  

  

  

