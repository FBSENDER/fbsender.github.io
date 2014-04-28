---
layout: post
title:  "linux redirect"
date:   2014-04-28
categories: linux
---

标准输入,输出和错误
---------------------------------
文件文件                描述符
---------------------------------
输入文件—标准输入       0
输出文件—标准输出       1
错误输出文件—标准错误   2
---------------------------------
 1.重定向
COMMAND_OUTPUT >
  2       # 将stdout重定向到一个文件. 
  3       # 如果这个文件不存在, 那就创建, 否则就覆盖. 
  4 
  5       ls -lR > dir-tree.list
  6       # 创建一个包含目录树列表的文件. 
  7 
  8    : > filename
  9       # >操作, 将会把文件"filename"变为一个空文件(就是size为0). 
 10       # 如果文件不存在, 那么就创建一个0长度的文件(与'touch'的效果相同). 
 11       # :是一个占位符, 不产生任何输出. 
 		  # zsh下：后可以加或不加空格
 12 
 13    > filename    
 14       # >操作, 将会把文件"filename"变为一个空文件(就是size为0). 
 15       # 如果文件不存在, 那么就创建一个0长度的文件(与'touch'的效果相同). 
 16       # (与上边的": >"效果相同, 但是某些shell可能不支持这种形式.)
 		  # zsh 这样操作后清空了文件内容，回车后标准输入内容会输入该文件，ctrl + d 结束输入
 		  # 回车后ctrl + d 一下，否则两下
 		  # 1>filename &>filename 效果一样
 		  # 1>>filename 为追加输入
 		  # 2>filename 无法输入，其他数字也不可以
 17 
 18    COMMAND_OUTPUT >>
 19       # 将stdout重定向到一个文件. 
 20       # 如果文件不存在, 那么就创建它, 如果存在, 那么就追加到文件后边. 
 21 
 22 
 23       # 单行重定向命令(只会影响它们所在的行): 
 24       # --------------------------------------------------------------------
 25 
 26    1>filename
 27       # 重定向stdout到文件"filename". 
 28    1>>filename
 29       # 重定向并追加stdout到文件"filename". 
 30    2>filename
 31       # 重定向stderr到文件"filename". 
 32    2>>filename
 33       # 重定向并追加stderr到文件"filename". 
 34    &>filename
 35       # 将stdout和stderr都重定向到文件"filename". 
 36 
 37    M>N
 38      # "M"是一个文件描述符, 如果没有明确指定的话默认为1. 
 39      # "N"是一个文件名. 
 40      # 文件描述符"M"被重定向到文件"N". 
 41    M>&N
 42      # "M"是一个文件描述符, 如果没有明确指定的话默认为1. 
 43      # "N"是另一个文件描述符. 
 44 
 45       #==============================================================================
 46 
 47       # 重定向stdout, 一次一行. 
 48       LOGFILE=script.log
 49 
 50       echo "This statement is sent to the log file, \"$LOGFILE\"." 1>$LOGFILE
 51       echo "This statement is appended to \"$LOGFILE\"." 1>>$LOGFILE
 52       echo "This statement is also appended to \"$LOGFILE\"." 1>>$LOGFILE
 53       echo "This statement is echoed to stdout, and will not appear in \"$LOGFILE\"."
 54       # 每行过后, 这些重定向命令会自动"reset". 
 
 58       # 重定向stderr, 一次一行. 
 59       ERRORFILE=script.errors
 61       bad_command1 2>$ERRORFILE       #  Error message sent to $ERRORFILE.
 62       bad_command2 2>>$ERRORFILE      #  Error message appended to $ERRORFILE.
 63       bad_command3                    #  Error message echoed to stderr,
 64                                       #+ and does not appear in $ERRORFILE.
 65       # 每行过后, 这些重定向命令也会自动"reset". 
 66       #==============================================================================
 67 
 70    2>&1
 71       # 重定向stderr到stdout. 
 72       # 将错误消息的输出, 发送到与标准输出所指向的地方. 
 73 
 74    i>&j
 75       # 重定向文件描述符i到j. 
 76       # 指向i文件的所有输出都发送到j. 
 77 
 78    >&j
 79       # 默认的, 重定向文件描述符1(stdout)到j. 
 80       # 所有传递到stdout的输出都送到j中去. 
 81 
 82    0< FILENAME
 83     < FILENAME
 84       # 从文件中接受输入. 
 85       # 与">"是成对命令, 并且通常都是结合使用. 
 86       #
 87       # grep search-word <filename
 88 
 90    [j]<>filename
 91       # 为了读写"filename", 把文件"filename"打开, 并且将文件描述符"j"分配给它. 
 92       # 如果文件"filename"不存在, 那么就创建它. 
 93       # 如果文件描述符"j"没指定, 那默认是fd 0, stdin. 
 94       #
 95       # 这种应用通常是为了写到一个文件中指定的地方. 
 96       echo 1234567890 > File    # 写字符串到"File". 
 97       exec 3<> File             # 打开"File"并且将fd 3分配给它. 
 98       read -n 4 <&3             # 只读取4个字符. 
 99       echo -n . >&3             # 写一个小数点. 
100       exec 3>&-                 # 关闭fd 3.
101       cat File                  # ==> 1234.67890
102       # 随机访问. 
106    |
107       # 管道. 
108       # 通用目的处理和命令链工具. 
109       # 与">", 很相似, 但是实际上更通用. 
110       # 对于想将命令, 脚本, 文件和程序串连起来的时候很有用. 
111       cat *.txt | sort | uniq > result-file
112       # 对所有.txt文件的输出进行排序, 并且删除重复行. 
113       # 最后将结果保存到"result-file"中. 
command > filename 　　　　　把标准输出重定向到一个新文件中
command >> filename 　　　　　把标准输出重定向到一个文件中(追加)
command 1 > fielname 　　　　　把标准输出重定向到一个文件中
command > filename 2>&1 　　　把标准输出和标准错误一起重定向到一个文件中
command 2 > filename 　　　　把标准错误重定向到一个文件中
command 2 >> filename 　　　　把标准输出重定向到一个文件中(追加)
command >> filename 2>&1 　　把标准输出和标准错误一起重定向到一个文件中(追加)
command < filename >filename2 　　把command命令以filename文件作为标准输入，以filename2文件作为标准输出
command < filename 　　　把command命令以filename文件作为标准输入
command << delimiter 　　把从标准输入中读入，直至遇到delimiter分界符
command <&m 　　　把文件描述符m作为标准输入
command >&m 　　　把标准输出重定向到文件描述符m中
command <&- 　　　把关闭标准输入
2.双向重定向
　　即在重定向数据到目标文件的同时，还要保证数据能够正常处理，使用tee命令。
　　tee [-a]  file
　　　　-a 往文件尾添加内容　　
　　last | tee last_backup | cut -d " " -f 1  #tee相当于对last的结果备份了一次