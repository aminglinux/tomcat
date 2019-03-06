####Tomcat中的realm
Realm其实就是一个存放用户名，密码及角色的一个“数据库”。

Tomcat中的Realm有下面几种，你也可以使用自己的Realm，只要实现org.apache.catalina.Realm就可以了。

##### JDBCRealm
授权信息存在关系数据库中, 通过JDBC驱动访问;

数据库中必须至少有两张表，表示用户及角色;

用户表必须至少有两个字段，用户名及密码;

角色表必须至少有两个字段，用户名及角色

建表语句如下：
```
CREATE TABLE users (
  user_name         VARCHAR(15) NOT NULL PRIMARY KEY,
  user_pass         VARCHAR(15) NOT NULL
);
 
CREATE TABLE user_roles (
  user_name         VARCHAR(15) NOT NULL,
  role_name         VARCHAR(15) NOT NULL,
  PRIMARY KEY (user_name, role_name)
);
```
Tomcat配置如下：
```

<Realm className="org.apache.catalina.realm.JDBCRealm"
  driverName="org.gjt.mm.mysql.Driver"
  connectionURL="jdbc:mysql://localhost/authority?user=dbuser&password=dbpass"
  userTable="users" userNameCol="user_name" userCredCol="user_pass"
  userRoleTable="user_roles" roleNameCol="role_name"/>
```

#####DataSourceRealm
授权信息存在关系数据库中, 通过JNDI JDBC数据源访问

数据库中必须至少有两张表，表示用户及角色

用户表必须至少有两个字段，用户名及密码

角色表必须至少有两个字段，用户名及角色

数据库建表语句：
```

CREATE TABLE users (
  user_name         VARCHAR(15) NOT NULL PRIMARY KEY,
  user_pass         VARCHAR(15) NOT NULL
);
 
CREATE TABLE user_roles (
  user_name         VARCHAR(15) NOT NULL,
  role_name         VARCHAR(15) NOT NULL,
  PRIMARY KEY (user_name, role_name)

```

Tomcat配置：
```
<Realm className="org.apache.catalina.realm.DataSourceRealm"
  dataSourceName="jdbc/authority"
  userTable="users" userNameCol="user_name" userCredCol="user_pass"
  userRoleTable="user_roles" roleNameCol="role_name"/>
```

#####JNDIRealm
授权信息存在LDAP目录服务器中，通过JNDI提供者访问

Tomcat配置：
```
<Realm className="org.apache.catalina.realm.JNDIRealm"
  connectionName="cn=Manager,dc=mycompany,dc=com"
  connectionPassword="secret"
  connectionURL="ldap://localhost:389"
  userPassword="userPassword"
  userPattern="uid={0},ou=people,dc=mycompany,dc=com"
  roleBase="ou=groups,dc=mycompany,dc=com"
  roleName="cn"
  roleSearch="(uniqueMember={0})"
/>
```

#####UserDatabaseRealm
默认配置，只是用于少量用户

授权信息存在用户数据JNDI资源中，该资源通常是一个XML文档 (conf/tomcat-users.xml)

Tomcat配置：
```
<tomcat-users>
  <user name="tomcat" password="tomcat" roles="tomcat" />
  <user name="role1"  password="tomcat" roles="role1"  />
  <user name="both"   password="tomcat" roles="tomcat,role1" />
</tomcat-users>
```

#####MemoryRealm
授权信息存在内存中的对象集合中，该对象集合来自XML文档 (conf/tomcat-users.xml).

仅用于测试。

#####JAASRealm
通过JAAS框架访问授权信息，最灵活最开放的一种授权方式。

如果前面几种方式满足不了你的需求，可以先试试这种方式。

Tomcat配置：
```
<Realm className="org.apache.catalina.realm.JAASRealm"
  appName="MyFooRealm"
  userClassNames="org.foobar.realm.FooUser"
  roleClassNames="org.foobar.realm.FooRole"/>
```

#####CombinedRealm
采用多种方式授权，只要任何一个认证通过则表示认证成功。

要保证用户名在所有realm中唯一。

Tomcat配置：
```

<Realm className="org.apache.catalina.realm.CombinedRealm" >
  <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
    resourceName="UserDatabase"/>
  <Realm className="org.apache.catalina.realm.DataSourceRealm"
    dataSourceName="jdbc/authority"
    userTable="users" userNameCol="user_name" userCredCol="user_pass"
    userRoleTable="user_roles" roleNameCol="role_name"/>
</Realm>
```

#####LockOutRealm
该Realm继承自CombinedRealm，在此基础上提供了用户锁定机制，多次登录失败后，锁定用户

Tomcat配置：
```
<Realm className="org.apache.catalina.realm.LockOutRealm" >
  <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
    resourceName="UserDatabase"/>
</Realm>
```
Realm可以被添加到任意级别的Container（Context、Host、Engine），并对所有下级Container生效。

默认情况下，Tomcat在Engine下添加了一个LockOutRealm，且包含了一个UserDatabaseRealm。