---
layout: post
title: "Linux Kill 多个进程"
date: 2014-06-12 17:17
categories: linux
---

kill 包含keyword_script 关键字的进程:
ps aux | grep keyword_script | grep -v grep | cut -c 10-15 | xargs kill

ps aux 为展示进程信息，具体参数需自行查看ps -h
| 表示管道，左侧的输出作为右侧的输入
grep xxx 为查询关键词
grep -v xxx 排除关键词 
cut 截取字符串 
xargs kill 结束所输入进程号的进程

kill -9 为强制结束
kill: kill [-s sigspec | -n signum | -sigspec] pid | jobspec ... or kill -l [sigspec]
Send a signal to a job.

Send the processes identified by PID or JOBSPEC the signal named by
SIGSPEC or SIGNUM.  If neither SIGSPEC nor SIGNUM is present, then
SIGTERM is assumed.

Options:
-s sig    SIG is a signal name
-n sig    SIG is a signal number
-l        list the signal names; if arguments follow `-l' they are
assumed to be signal numbers for which names should be listed

Kill is a shell builtin for two reasons: it allows job IDs to be used
instead of process IDs, and allows processes to be killed if the limit
on processes that you can create is reached.

Exit Status:
Returns success unless an invalid option is given or an error occurs.

ps --help
  -A all processes                      -C by command name
  -N negate selection                   -G by real group ID (supports names)
-a all w/ tty except session leaders  -U by real user ID (supports names)
  -d all except session leaders         -g by session OR by effective group name
  -e all processes                      -p by process ID
  T  all processes on this terminal     -s processes in the sessions given
  a  all w/ tty, including other users  -t by tty
g  OBSOLETE -- DO NOT USE             -u by effective user ID (supports names)
  r  only running processes             U  processes for specified users
  x  processes w/o controlling ttys     t  by tty
  *********** output format **********  *********** long options ***********
  -o,o user-defined  -f full            --Group --User --pid --cols --ppid
  -j,j job control   s  signal          --group --user --sid --rows --info
  -O,O preloaded -o  v  virtual memory  --cumulative --format --deselect
  -l,l long          u  user-oriented   --sort --tty --forest --version
  -F   extra full    X  registers       --heading --no-heading --context
  ********* misc options *********
  -V,V  show version      L  list format codes  f  ASCII art forest
  -m,m,-L,-T,H  threads   S  children in sum    -y change -l format
  -M,Z  security data     c  true command name  -c scheduling class
  -w,w  wide output       n  numeric WCHAN,UID  -H process hierarchy


