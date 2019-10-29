#### tomcat-user配置
```
配置文件conf/tomcat-user.xml
```
##### 角色
```
manager-gui - allows access to the HTML GUI and the status pages
manager-script - allows access to the text interface and the status pages
manager-jmx - allows access to the JMX proxy and the status pages
manager-status - allows access to the status pages only
admin-gui - allows access to the HTML GUI
admin-script - allows access to the text interface
```


##### 配置样例
```
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">

<role rolename="manager-gui"/> 
<role rolename="admin-gui"/> 
<user username="tomcat" password="tomcat123" roles="manager-gui,admin-gui"/>
</tomcat-users>
```
##### host-manager需要
```
admin-gui  用于控制页面访问权限
admin-script 用于控制以简单的文本的形式进行访问host-manager
```

###### manager需要
```
manager-gui  用于控制manager页面的访问
manager-script  用于控制以简单的文本的形式进行访问manager
manager-jmx 用于控制jmx访问
manager-status 用于控制服务器状态的查看
```

##### 403问题
```
webapp/manager/META-INF/context.xml
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|192.168.133.*" />
#要更改这个allow的ip地址，.*表示通配。
```
