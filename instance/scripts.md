#####启动脚本
```
文件名： /data/tomcat-instance/www.123.com/start.sh
内容如下：
#!/bin/bash
export CATALINA_HOME=/usr/local/tomcat
export CATALINA_BASE=/data/tomcat-instance/www.123.com
TOMCAT_ID=`ps aux |grep "java"|grep "Dcatalina.base=$CATALINA_BASE "|grep -v "grep"|awk '{ print $2}'`
if [ -n "$TOMCAT_ID" ] 
then 
    echo "tomcat(${TOMCAT_ID}) still running now , please shutdown it first"; 
    exit 2; 
else
       $CATALINA_HOME/bin/startup.sh
    if [ "$?" = "0" ]; then 
        echo "start succeed" 
    else 
        echo "sart failed"
    fi
fi
```

#####关闭脚本
```
文件名：/data/tomcat-instance/www.123.com/shutdown.sh
内容如下：
#!/bin/bash
export CATALINA_HOME=/usr/local/tomcat
export CATALINA_BASE=/data/tomcat-instance/www.123.com
TOMCAT_ID=`ps aux |grep "java"|grep "Dcatalina.base=$CATALINA_BASE "|grep -v "grep"|awk '{ print $2}'`
if [ -n "$TOMCAT_ID" ] ; then 
    TOMCAT_STOP_LOG=`$CATALINA_HOME/bin/shutdown.sh` 
       if [ "$?" = "0" ]; then 
        echo "stop succeed" 
    else 
        echo "stop failed"
    fi
else 
    echo "Tomcat instance not found" 
    exit 
fi

```