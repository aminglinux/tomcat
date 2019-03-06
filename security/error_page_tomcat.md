####Tomcat配置默认错误页
错误页，指的是当访问出现403、404、400、500、502等错误码时，给用户反馈的一个网页。

默认tomcat自带了这样的错误页，但是比较丑陋，我们可以自定义。

#####确定好web.xml在哪里
```
首先确定好网站应用所在的目录，
1 如果是默认的webapps，那么web.xml是在webapps/ROOT/WEB-INF/下面

2 如果自定义了docBase，那么web.xml是在docBase下的WEB-INF目录下
```

#####修改web.xml
```
加入或者更改如下内容：

	<error-page>
		<error-code>400</error-code>
		<location>/error.jsp</location>
	</error-page>

	<error-page>
		<error-code>404</error-code>
		<location>/error.jsp</location>
	</error-page>

	<error-page>
		<error-code>500</error-code>
		<location>/error.jsp</location>
	</error-page>

注意，这部分内容要放到<web-app> </web-app>里面
```

#####定义error.jsp
```
这个error.jsp是相对该应用的根目录来说的，
1 如果是默认的webapps，则应该在webapps/ROOT/下

2 如果定义了docBase，则应该是在docBase目录下

error.jsp内容就是程序员该考虑的事情了。这里给一个参考文件：
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	尊敬的朋友，后台服务器有问题，工程师们正在努力抢修中，请稍后访问......
</body>
</html>
```