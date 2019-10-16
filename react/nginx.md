# Nginx

nignx.conf的配置

```
  location / {
            root   D:\hash；  文件的路径。比如我打包项目的文件的地址如下图
            index  index.html index.htm;
        }
```

![snipaste_20190919_220649](C:\Users\www\Desktop\snipaste_20190919_220649.png)

2、对那些url实行代理

```
		#这个是最为重要的，是标识那些文件需要代理的标志，只要识别到网址中带有/api，nginx就会对这个地址进行代理，因此我们需要在axios
		#的基础地址中，加个/api.也就是axios.default.baseURL:“/api”
		location /api/ {
	    proxy_pass http://127.0.0.1:5000/;
	    try_files $uri $uri/ /index.html; # 总是返回首页，避免了前台路由当做后台路由处理的情况
	}
```

