# Spring Boot项目启动脚本与停止脚本



#### 启动脚本`start.sh`

``` shell
#!/bin/sh

APPDIR=`pwd`
PIDFILE=$APPDIR/project_name.pid
if [ -f "$PIDFILE" ] && kill -0 $(cat "$PIDFILE"); then
echo "project_name is already running..."
exit 1
fi
nohup java -jar project_name.jar >> running.log 2>&1 &
echo $! > $PIDFILE
echo "start project_name success..."
```

#### 停止脚本`stop.sh`

``` shell
#!/bin/sh

APPDIR=`pwd`
PIDFILE=$APPDIR/project_name.pid
if [ ! -f "$PIDFILE" ] || ! kill -0 "$(cat "$PIDFILE")"; then
echo "project_name not running..."
else
echo "stopping project_name..."
PID="$(cat "$PIDFILE")"
kill -9 $PID
rm "$PIDFILE"
echo "...project_name stopped"
fi
```

#### 查看日志脚本`log.sh`

``` shell
tail -f running.log
```

