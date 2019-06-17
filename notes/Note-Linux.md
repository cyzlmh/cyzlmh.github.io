---
layout: page
title: Linux
category: note
tags: Others
---

* content
{:toc}

# Command Line

| command                  | result                                   |
| ------------------------ | ---------------------------------------- |
| date                     | print date                               |
| cal                      | print calender                           |
| df                       | print hard disk usage                    |
| free                     | print memory usage                       |
| pwd                      | print working directory                  |
| cd                       | change directory                         |
|                          | dot (.): current directory, dot dot (..): parent directory |
|                          | dash (-):  the previous working directory |
|                          | ~*user_name*: home directory of the user |
| ls                       | list directory contents                  |
| file                     | print file information                   |
| less                     | show text file                           |
| cat /etc/os-release      | check linux version
| du -h --max-depth=1      | show volumes
|                          |                                          |
| cp                       | copy files and directories               |
| mv                       | move/rename files and directories        |
| mkdir                    | create directories                       |
| rm                       | remove files and directories             |
| ln                       | create hard and symbolic links           |
|                          |                                          |
| type                     | Indicate how a command name is interpreted |
| which                    | Display which executable program will be executed |
| help                     | Get help for shell builtins              |
| man                      | Display a command's manual page          |
| apropos                  | Display a list of appropriate commands   |
| info                     | Display a command's info entry           |
| whatis                   | Display a very brief description of a command |
| alias                    | Create an alias for a command            |
|                          |                                          |
| \> *file*                | redirect standard output to a file       |
| \>\> *file*              | append standard output to a file         |
| 2\> *file*               | redirect standard error to a file        |
| &> *file*                | redirect standard output and error to one file |
| &>> *file*               | append standard output and error to one file |
| 2> /dev/null             | throw it away                            |
|                          |                                          |
| cat                      | display files without paging             |
| cat [*file...*] > *file* | concatenate files                        |
| cat > *file*             | write new file                           |
| cat < *file*             | redirect input from a file               |
| sort                     | Sort lines of text                       |
| uniq                     | Report or omit repeated lines            |
| grep                     | Print lines matching a pattern           |
| wc                       | Print newline, word, and byte counts for each file |
| head                     | Output the first part of a file          |
| tail                     | Output the last part of a file           |
| tee                      | Read from standard input and write to standard output and files |
| *command1* \| *command2* | pipeline                                 |
|                          |                                          |
| echo                     | print                                    |
|                          |                                          |
| [[:upper:]]              |                                          |
| $((*expression*))        | Arithmetic Expansion                     |
| echo $(ls)               | Command Substitution                     |
| echo Number_{1..5}       | Brace Expansion                          |
| echo Front-{A,B,C}-Back  |                                          |
| echo $USER               | Parameter Expansion                      |
| " "                      | suppress expansions except parameter expansion, arithmetic expansion, and command substitution |
| ' '                      | suppress all expansions                  |
|                          |                                          |
| clear/Ctrl+L             | clear the screen                         |
| history                  | Display the contents of the history list |
| Ctrl+A                   | Move cursor to the beginning of the line |
| Ctrl+E                   | Move cursor to the end of the line       |
| Alt+F                    | Move cursor forward one word             |
| Alt+B                    | Move cursor backward one word            |
| Ctrl+K                   | Kill text from the cursor location to the end of line |
| Ctrl+U                   | Kill text from the cursor location to the beginning of the line |
| Ctrl+Y                   | Yank text from the kill-ring and insert it at the cursor location |
| \<tab\> \<tab\>          | Display list of possible completions     |
| Alt+*                    | Insert all possible completions          |
|                          |                                          |
| id                       | Display user identity                    |
| chmod                    | Change a file's mode                     |
| ps                       | Report a snapshot of current processes   |
| pstree                   | Outputs a process list arranged in a tree-like pattern showing the parent/child relationships between processes. |
| top                      | Display tasks                            |
| jobs                     | List active jobs                         |
| bg/fg                    | Place a job in the background/foreground |
| kill                     | Send a signal to a process               |
| killall                  | Kill processes by name                   |
| shutdown                 | Shutdown or reboot the system            |
| printenv                 | Print part or all of the environment     |
| /etc/profile             | A global configuration script that applies to all users for login shell sessions |
| ~/.bash_profile          | A user's personal startup file for login shell sessions |
| /etc/bash.bashrc         | A global configuration script that applies to all users for non-login shell sessions |
| ~/.bashrc                | A user's personal startup file for non-login shell sessions |
|                          |                                          |
| tar -czvf X.tar.gz dir   | Compress and pack up                     |
| tar -xzvf X.tar.gz dir   | Decompress and unpack                    |


# File system

![File system](/assets/notes/Note-Linux/FileSystem.jpg)

## Find files

### find

find <指定目录> <指定条件> <指定动作>
find . -name 'filename'

### locate

locate dir/filename

如果查找不到最新变动的文件
updatedb

### whereis

whereis grep

### which

which grep

## 文件内查找

grep -E match_pattern file_name

## 后台运行

``` shell
nohup command >/dev/null 2>&1 &
```

## 删除进程

``` shell
ps aux | grep [process_name]
kill -9 [pid]
```

# vim

| Command          | Result     |
| ---------------- | ---------- |
| i                | insert     |
| :q               | 退出       |
| :q!              | 不保存退出 |
| :w               | 保存       |
| :wq              | 保存退出   |
| w                | 后跳一词   |
| b                | 前跳一词   |
| $                | 跳到行尾   |
| 0                | 跳到行首   |
| G                | 跳到尾行   |
| 5G               | 跳到第5行  |
| x                | 删除       |
| d                | 剪切       |
| dd               | 删除整行   |
| y                | 复制       |
| p                | 粘贴       |
| /+*word*+<ENTER> | 查找       |
| n                | 查找下一个 |



# Shell Script

``` shell
# 文件头
#!/bin/bash

# 定义变量
var="a string" #等号两边不能有空格

#调用变量
echo $var
echo ${var}

#执行命令
`cat some.txt`
$(cat some.txt)

# here document
command <<- _EOF_
    text
_EOF_
```

## Function

``` shell
function name {
    commands
    return
}
name () {
    commands
    return
}

# local variable
function name {
    local var
    var=2
    return
}
```

## Branching and Looping

### IF

``` shell
x=5
if [ $x -eq 5 ]; then
    echo "x equals 5."
elif [ $x -eq 4 ]; then
    echo "x equals 4."
else
    echo "x does not equal 5 or 4."
fi
```

#### File test

| Expression          | True                                                     |
| ------------------- | -------------------------------------------------------- |
| [ -e file ]         | file exists                                              |
| [ -d file ]         | file exists and is a directory                           |
| [ -f file ]         | file exists and is a regular file                        |
| [ file1 -ef file2 ] | the two filenames refer to the same file by hard linking |

#### String test

| Expression             | True                              |
| ---------------------- | --------------------------------- |
| [ string ]             | not null                          |
| [ -n string ]          | The length is greater than zero   |
| [ -z string ]          | The length is zero                |
| [ string1 == string2 ] | equal                             |
| [ string1 != string2 ] | not equal                         |
| [ string1 > string2 ]  | string1 sorts after string2       |
| [[ string =~ regex ]]  | matched by the regular expression |
| [[ string == *.txt ]]  | matched by the expansion          |

#### Integer test

``` shell
if ((INT < 0)); then
    echo "here"
elif (( ((INT % 2)) == 0)); then
    echo "here"
elif (( ((INT % 2)) == 0 && ((INT % 3)) == 0 )); then
    echo "here"
else
    echo "here"
fi
```

#### Logit

| Expression           | Logit |
| -------------------- | ----- |
| [[ exp1 && exp2 ]]   | and   |
| [[ exp1 \|\| exp2 ]] | or    |
| [[ ! exp1 ]]         | not   |

``` shell
# do command2 if command1 is sucessful
command1 && command2
# do command2 if command1 is unsucessful
command1 || command2
```

#### Positional Paramaters

- \$0：路径
  - dirname \$0：路径名
  - basename \$0：文件名
- \$1~\$9：输入参数
- \$?：上一次退出状态，0为正常，其他为错误

### CASE

``` shell
read -p
case $REPLY in
    0)    echo "zero"
        ;;
    1)    echo "one"
        ;;
    2)    echo "two"
        ;;
    *)    echo "other"
        ;;
esac
```

### WHILE

``` shell
count=1
while [[ $count -le 5 ]]; do 
    echo $count 
    count=$((count + 1)) 
done
```

### FOR

``` shell
for (( i=0; i<5; i=i+1 )); do 
    echo $i
done
```

## String Processing

### replace

``` shell
tr 'old' 'new' file.txt
```

### delete

``` shell
tr -d 'to_delete'
```

### reverse

``` shell
rev "string"
```

### cut

``` shell
cut -c1- "string"
```

# Network

``` shell
# 查看网络配置
ifconfig
# 查看公网ip
curl ifconfig.me
# 远程登陆
ssh username@ip_adress:port
# 传文件
scp file username@ip_adress:path
rsync -avz --remove-source-files -e "ssh -p $portNumber" user@remoteip:/path/to/files/ /local/path/
```

## Samba

``` shell
# show file shares
smbclient -L //host -U username%password
# connecting to a file shares
smbclient //host/share_dir -U username%password

```

# Tools

- tldr: too long didn't read