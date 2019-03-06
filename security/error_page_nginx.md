####Nginx上配置默认错误页
生产环境中，大部分应用场景都是，Nginx代理Tomcat，所以默认的错误页面可以在Nginx这里定义。
但是，有一个参数很重要，必须要加上，否则错误页还会到Tomcat那里。

#####配置Nginx
```
server
{
    listen 80 ;
    server_name aminglinux.com;

    error_page 404 /404.html;
    location /404.html
    {
        root /data/wwwroot/aminglinux.com;
    }
    proxy_intercept_errors on;
    
    location /
    {
        proxy_pass http://127.0.0.1:8080/;
        proxy_set_header Host   $host;
        proxy_set_header X-Real-IP      $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}

```

#####说明
```
将proxy_intercept_errors 设置为on
当上游服务器响应头回来后，可以根据响应状态码的值进行拦截错误处理，与error_page 指令相互结合。用在访问上游服务器出现错误的情况下。
```