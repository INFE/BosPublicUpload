

# BosPublicUpload

提供了一个公共的页面可以实现直接上传到百度bos服务器。 
	北京 bucket： http://bj.bcebos.com/console-bos-uploader-bj/public-index.html
	广州 bucket： http://gz.bcebos.com/console-bos-uploader-gz/public-index.html

页面接受4个参数：

bucket   // 必填 bucket名字

path   // 可选 根据用户是否需要上传到某个文件夹中

auth   // 可选 如果bucket是非 公共读写的， 需要用户提供一个处理JSONP请求，并返回 签名信息 的URL

csrfToken  // 可选 如果是公共读写的 不需要填写， 非公共读写的建议携带一个 基于标识客户信息的cookie生成的token。以便后端接收jsonp请求签名时 核对用户信息。

##2个例子

例子1： 我要往北京的名为 test1的公共读写bucket 上传文件 需要拼出 ：

http://bj.bcebos.com/console-bos-uploader-bj/public-index.html?bucket=test1 放到用户页面中的一个 iframe中。



例子2：我要向广州的名为abc的私有bucket中的test文件夹上传文件 需要拼出：

http://gz.bcebos.com/console-bos-uploader-gz/public-index.html?bucket=abc&path=test/&auth=http://mytestsite.com/api/getauth&csrftoken=mytokenbaseoncookie  放到用户页面中的iframe中

其中：http://mytestsite.com/api/getauth  是需要用户自己提供的一个支持jsonp请求签名的接口，上传时会向该接口请求签名。请求的 参数有：

bucket:  用户传的bucket 名字

object：path + file名字

csrftoken: 用户传的token  （后端接收到csrftoken后需要校验是否与cookie信息一致，来防止跨站伪造请求）

签名接口返回的格式为JSONP标准格式：
callback({"key":"authorization","value":"bce-auth-v1%xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"})


具体的签名算法 参考 ：http://bce.baidu.com/doc/BCC/API.html#.F1.E1.8E.5D.A4.2D.55.13.D1.03.FE.3B.AB.70.23.05 




##其他

1.开始上传, 上传完毕, 上传完毕且全部成功时，分别会通过postmessage 向 容器页面 发送uploading， confirm 和 finish 消息

2.每上传成功一个文件，会通过postmessage 向容器页面发送文件名字 （15年12月1日更新）

3.有个 [nodejs的 sdk](https://www.npmjs.com/package/baidubce-sdk) , 有兴趣的可以去看看

4.[bce-bos-uploader](https://github.com/leeight/bce-bos-uploader) 是基于 bce-sdk-js 开发的一个 ui 组件，易用性更好 [https://github.com/leeight/bce-bos-uploader](https://github.com/leeight/bce-bos-uploader)

