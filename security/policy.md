####Tomcat中的policy
```
在tomcat的conf/目录下面有一个catalina.policy文件，这个文件里规定了JVM的安全策略。
当tomcat在启动时加上了-security选项后，则会以该文件中的安全策略来定义JVM的安全。
该文件通过grant语句来定义目录、文件的读、写与执行权限。
```

#####grant格式
```
在Policy文件中的每一个grant记录含有一个CodeSource（一个指定的代码）及其permission(许可)。 

Policy文件中的每一条grant记录遵循下面的格式，以保留字“grant”开头，表示一条新的记录的开始，
“Permission”是另一个保留字，在记录中用来标记一个新的许可的开始。每一个grant记录授予一个指定的代码（CodeBase）一套许可（Permissions）。 

permission_class_name必须是一个合格并存在的类名，例如java.io.FilePermission，不能使用缩写（例如，FilePermission）。 

target_name用来指定目标类的位置，action用于指定目标类拥有的权限。 

target_name可以直接指定类名（可以是绝对或相对路径），目录名，也可以是下面的通配符： 
directory/* 目录下的所有文件
*当前目录的所有文件
directory/-目录下的所有文件，包括子目录
- 当前目录下的所有文件，包括子目录
《ALL FILES》文件系统中的所有文件

对于java.io.FilePermission，action可以是：
read, write, delete和execute

对于java.net.SocketPermission，action可以是：
listen，accept，connect，read，write
```

#####grant示例
```
grant { 
//对系统和用户目录“读”的权限
permission java.util.PropertyPermission "user.dir", "read";
permission java.util.PropertyPermission "user.home", "read";
permission java.util.PropertyPermission "java.home", "read";
permission java.util.PropertyPermission "java.class.path", "read";
permission java.util.PropertyPermission "user.name", "read"; 

//对线程和线程组的操作权限
permission java.lang.RuntimePermission "modifyThread";
permission java.lang.RuntimePermission "modifyThreadGroup";

//操作Socket端口的各种权限
permission java.net.SocketPermission "-", "listen";
permission java.net.SocketPermission "-", "accept";
permission java.net.SocketPermission "-", "connect";
permission java.net.SocketPermission "-", "read";
permission java.net.SocketPermission "-", "write";

//读写文件的权限
permission java.io.FilePermission "-", "read";
permission java.io.FilePermission "-", "write";

//退出系统的权限，例如System.exit(0)
permission java.lang.RuntimePermission "exitVM";
};
```