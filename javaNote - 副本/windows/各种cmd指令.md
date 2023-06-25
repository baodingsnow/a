# 关闭指定端口

1. netstat -nao | findstr “8080” 查询8080端口的PID

2. taskkill /pid 3017 /F 关闭pid为3017的进程

关闭进程需要以管理员方式打开cmd

