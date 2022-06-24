## 安装
`README.md`

## 进程管理

### 写入supervisord.conf 配置文件（文件为我自己总结的，根据需要修改）
```
cat <<'EOF' > /etc/supervisord.conf
 
[unix_http_server]
file=/var/log/supervisor/supervisor.sock ; the path to the socket file
;chmod=0700 ; socket file mode (default 0700)
;chown=nobody:nogroup ; socket file uid:gid owner
;username=user ; default is no username (open server)
;password=123 ; default is no password (open server)
 
;[inet_http_server] ; inet (TCP) server disabled by default
;port=127.0.0.1:9001 ; ip_address:port specifier, *:port for all iface
;username=user ; default is no username (open server)
;password=123 ; default is no password (open server)
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
[supervisord]
logfile=/var/log/supervisor/supervisord.log ; main log file; default $CWD/supervisord.log
logfile_maxbytes=50MB ; max main logfile bytes b4 rotation; default 50MB
logfile_backups=10 ; # of main logfile backups; 0 means none, default 10
loglevel=info ; log level; default info; others: debug,warn,trace
pidfile=/var/log/supervisor/supervisord.pid ; supervisord pidfile; default supervisord.pid
nodaemon=false ; start in foreground if true; default false
silent=false ; no logs to stdout if true; default false
minfds=65535 ; min. avail startup file descriptors; default 1024
minprocs=65535 ; min. avail process descriptors;default 200
;umask=022 ; process file creation umask; default 022
;user=supervisord ; setuid to this UNIX account at startup; recommended if root
;identifier=supervisor ; supervisord identifier, default is 'supervisor'
;directory=/tmp ; default is not to cd during start
;nocleanup=true ; don't clean up tempfiles at start; default false
;childlogdir=/tmp ; 'AUTO' child log dir, default $TEMP
;environment=KEY="value" ; key value pairs to add to environment
;strip_ansi=false ; strip ansi escape codes in logs; def. false
 
[supervisorctl]
serverurl=unix:///var/log/supervisor/supervisor.sock ; use a unix:// URL for a unix socket
;serverurl=http://127.0.0.1:9001 ; use an http:// url to specify an inet socket
;username=chris ; should be same as in [*_http_server] if set
;password=123 ; should be same as in [*_http_server] if set
;prompt=mysupervisor ; cmd line prompt (default "supervisor")
;history_file=~/.sc_history ; use readline history if available
 
 
;[program:theprogramname]
;command=/bin/cat ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1 ; number of processes copies to start (def 1)
;directory=/tmp ; directory to cwd to before exec (def no cwd)
;umask=022 ; umask for process (default None)
;priority=999 ; the relative start priority (default 999)
;autostart=true ; start at supervisord start (default: true)
;startsecs=1 ; # of secs prog must stay up to be running (def. 1)
;startretries=3 ; max # of serial start failures when starting (default 3)
;autorestart=unexpected ; when to restart if exited after running (def: unexpected)
;exitcodes=0 ; 'expected' exit codes used with autorestart (default 0)
;stopsignal=QUIT ; signal used to kill process (default TERM)
;stopwaitsecs=10 ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false ; send stop signal to the UNIX process group (default false)
;killasgroup=false ; SIGKILL the UNIX process group (def false)
;user=chrism ; setuid to this UNIX account to run the program
;redirect_stderr=true ; redirect proc stderr to stdout (default false)
;stdout_logfile=/a/path ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10 ; # of stdout logfile backups (0 means none, default 10)
;stdout_capture_maxbytes=1MB ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false ; emit events on stdout writes (default false)
;stdout_syslog=false ; send stdout to syslog with process name (default false)
;stderr_logfile=/a/path ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10 ; # of stderr logfile backups (0 means none, default 10)
;stderr_capture_maxbytes=1MB ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false ; emit events on stderr writes (default false)
;stderr_syslog=false ; send stderr to syslog with process name (default false)
;environment=A="1",B="2" ; process environment additions (def no adds)
;serverurl=AUTO ; override serverurl computation (childutils)
; The sample eventlistener section below shows all possible eventlistener
; subsection values. Create one or more 'real' eventlistener: sections to be
; able to handle event notifications sent by supervisord.
;[eventlistener:theeventlistenername]
;command=/bin/eventlistener ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1 ; number of processes copies to start (def 1)
;events=EVENT ; event notif. types to subscribe to (req'd)
;buffer_size=10 ; event buffer queue size (default 10)
;directory=/tmp ; directory to cwd to before exec (def no cwd)
;umask=022 ; umask for process (default None)
;priority=-1 ; the relative start priority (default -1)
;autostart=true ; start at supervisord start (default: true)
;startsecs=1 ; # of secs prog must stay up to be running (def. 1)
;startretries=3 ; max # of serial start failures when starting (default 3)
;autorestart=unexpected ; autorestart if exited after running (def: unexpected)
;exitcodes=0 ; 'expected' exit codes used with autorestart (default 0)
;stopsignal=QUIT ; signal used to kill process (default TERM)
;stopwaitsecs=10 ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false ; send stop signal to the UNIX process group (default false)
;killasgroup=false ; SIGKILL the UNIX process group (def false)
;user=chrism ; setuid to this UNIX account to run the program
;redirect_stderr=false ; redirect_stderr=true is not allowed for eventlisteners
;stdout_logfile=/a/path ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10 ; # of stdout logfile backups (0 means none, default 10)
;stdout_events_enabled=false ; emit events on stdout writes (default false)
;stdout_syslog=false ; send stdout to syslog with process name (default false)
;stderr_logfile=/a/path ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10 ; # of stderr logfile backups (0 means none, default 10)
;stderr_events_enabled=false ; emit events on stderr writes (default false)
;stderr_syslog=false ; send stderr to syslog with process name (default false)
;environment=A="1",B="2" ; process environment additions
;serverurl=AUTO ; override serverurl computation (childutils)
; The sample group section below shows all possible group values. Create one
; or more 'real' group: sections to create "heterogeneous" process groups.
;[group:thegroupname]
;programs=progname1,progname2 ; each refers to 'x' in [program:x] definitions
;priority=999 ; the relative start priority (default 999)
; The [include] section can just contain the "files" setting. This
; setting can list multiple files (separated by whitespace or
; newlines). It can also contain wildcards. The filenames are
; interpreted as relative to this file. Included files *cannot*
; include files themselves.
[include]
files = supervisord.d/*.conf
EOF
```


### 服务进程管理

```

cat <<'EOF' > /etc/supervisord.d/kibana.conf
[program:kibana]
directory = /usr/local/elk/kibana-7.7.1/bin ; 程序的启动目录
command = sh kibana ; 启动命令，与命令行启动的命令是一样的
autostart = true     ; 在 supervisord 启动的时候也自动启动
startsecs = 5        ; 启动 5 秒后没有异常退出，就当作已经正常启动了
autorestart = true   ; 程序异常退出后自动重启
startretries = 3     ; 启动失败自动重试次数，默认是 3
user = elastic          ; 用哪个用户启动
redirect_stderr = true  ; 把 stderr 重定向到 stdout，默认 false
stdout_logfile_maxbytes = 20MB  ; stdout 日志文件大小，默认 50MB
stdout_logfile_backups = 10     ; stdout 日志文件备份数
; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
stdout_logfile = /var/log/kibana.log  ;日志统一放在log目录下
EOF
```



### 常用操作
```
启动
supervisord -c /etc/supervisord.conf
修改了配置文件重载
supervisorctl update
supervisorctl stop programxxx，停止某一个进程(programxxx)，programxxx 为 [program:beepkg] 里配置的值，这个示例就是 beepkg。
supervisorctl start programxxx，启动某个进程。
supervisorctl restart programxxx，重启某个进程。
supervisorctl status，查看进程状态。
supervisorctl stop groupworker ，重启所有属于名为 groupworker 这个分组的进程(start,restart 同理)。
supervisorctl stop all，停止全部进程，注：start、restart、stop 都不会载入最新的配置文件。
supervisorctl reload，载入最新的配置文件，停止原有进程并按新的配置启动、管理所有进程。
supervisorctl update，根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启。
```