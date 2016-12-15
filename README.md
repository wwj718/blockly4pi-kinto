# blockly4pi_cloud
为blockly4pi构建的云端，用于存储和分享程序，基于kinto

# 此前工作
[kinto_server](https://github.com/wwj718/raspberrypi_lab/blob/master/doc/kinto_server.md)

# 搭建kinto server

```bash
mkdir ~/kinto_server && cd ~/kinto_server
virtualenv env
source env/bin/activate
pip install kinto  kinto_redis
kinto init
kinto migrate
kinto start #运行在  0.0.0.0:8888
```

# kinto配置
### redis作为存储后端
安装redis:`sudo apt-get install redis-server`

在`kinto init`时选择redis
```
# Backends.
#
# https://kinto.readthedocs.io/en/latest/configuration/settings.html#storage
#
kinto.storage_backend = kinto_redis.storage
kinto.storage_url = redis://localhost:6379/1
kinto.cache_backend = kinto_redis.cache
kinto.cache_url = redis://localhost:6379/2
kinto.permission_backend = kinto_redis.permission
kinto.permission_url = redis://localhost:6379/3
```

# 开机自启
使用supervisor

配置文件：kinto_server.conf

```bash
[program:kinto_server]
user = root     ; 用哪个用户启动
directory = /home/ubuntu/kinto_server
command =  /usr/local/bin/kinto start #全局安装
autostart = true     ; 在 supervisord 启动的时候也自动启动
startsecs = 5        ; 启动 5 秒后没有异常退出，就当作已经正常启动了
autorestart = false   ; 不需要自动重启
;startretries = 3     ; 启动失败自动重试次数，默认是 3
redirect_stderr = true  ; 把 stderr 重定向到 stdout，默认 false
stdout_logfile_maxbytes = 20MB  ; stdout 日志文件大小，默认 50MB
stdout_logfile_backups = 20     ; stdout 日志文件备份数
; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
stdout_logfile = /tmp/kinto_server.log
;stopsignal = QUIT

; 可以通过 environment 来添加需要的环境变量，一种常见的用法是修改 PYTHONPATH
; environment=PYTHONPATH=$PYTHONPATH:/path/to/somewhere
```



# 感谢
*  [blockly](https://github.com/google/blockly)
*  [kinto](https://github.com/Kinto/kinto)
*  [kinto.js](https://github.com/Kinto/kinto.js)

# todo

[ ]  test
