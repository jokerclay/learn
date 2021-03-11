[TOC]



# bash script



## 1.查看你的系统上可用的 shell

```bash
pi@raspberrypi:~ $ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
```

## 2.查看你正在使用的bash

```bash
pi@raspberrypi:~ $ which bash
/bin/bash
```

## 3.第一个bash 脚本

```
#! /bin/bash
echo "Hello World !"
```

执行

```bash
pi@raspberrypi:~ $ ./helloscript.sh
-bash: ./helloscript.sh: Permission denied
```

需要赋予执行权限

```bash
pi@raspberrypi:~ $ chmod +x helloscript.sh
```

执行

```bash
pi@raspberrypi:~ $ ./helloscript.sh
Hello World !
```

## 4.重定向  redirect to file

* 将 屏幕上的输出 存到一个文件里  符号  `>` 

```bash
pi@raspberrypi:~ $ ls
helloscript.sh  MagPi
pi@raspberrypi:~ $ cat helloscript.sh
#! /bin/bash
echo "Hello World !" > file.txt
pi@raspberrypi:~ $ ./helloscript.sh
pi@raspberrypi:~ $ ls
file.txt  helloscript.sh  MagPi
pi@raspberrypi:~ $ cat file.txt
Hello World !
pi@raspberrypi:~ $
```

* 将屏幕上的输入存入文件  `>` `>>` 

  ```bash
  #! /bin/bash
  cat > test.txt
  ```

  1. 按`ctrl + d ` 结束脚本
  2. 原本没有 `test.txt ` 文件，脚本会创建

  ```bash
  pi@raspberrypi:~ $ ls
  file.txt  helloscript.sh  MagPi
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  cat > test.txt
  pi@raspberrypi:~ $ ./helloscript.sh
  hello this is a test
  pi@raspberrypi:~ $ ls
  file.txt  helloscript.sh  MagPi  test.txt
  pi@raspberrypi:~ $ cat test.txt
  hello this is a test
  pi@raspberrypi:~ $
  ```

  3. `>>`  不会抹去文件之前数据，会接着原有的数据添加数据

     `>` 会抹去原有的数据，存入新的数据

     ```bash
     pi@raspberrypi:~ $ ls
     file.txt  helloscript.sh  MagPi  test.txt
     pi@raspberrypi:~ $ cat helloscript.sh
     #! /bin/bash
     cat >> test.txt
     pi@raspberrypi:~ $ cat test.txt
     hello this is a test
     pi@raspberrypi:~ $ ./helloscript.sh
     hello this is a test again
     pi@raspberrypi:~ $ ls
     file.txt  helloscript.sh  MagPi  test.txt
     pi@raspberrypi:~ $ cat test.txt
     hello this is a test
     hello this is a test again
     pi@raspberrypi:~ $ vim helloscript.sh
     pi@raspberrypi:~ $ ./helloscript.sh
     hello this is a test again againpi@raspberrypi:~ $ pi@raspberrypi:~ $ ls
     file.txt  helloscript.sh  MagPi  test.txt
     pi@raspberrypi:~ $ cat test.txt
     hello this is a test again again
     pi@raspberrypi:~ $
     ```

* 注释

  ```bash
  #! /bin/bash
  # this is a comment 单行注释
  : '这
  是
  多
  行
  注
  释'
  echo "Hello"
  ```

  `#! /bin/bash`   与  `#this is a comment`

  后者是注释，前者 `#! /bin/bash`   \#! 是说明 hello 这个文件的类型的,类似于 Windows 系统下用不同文件后缀来表示不同文件类型的意思（但不相同）。Linux 系统根据 "#!" 及该字串后面的信息确定该文件的类型。

  `#!` 及后面的 "/bin/bash" 就表明该文件是一个 BASH 程序

  多行注释是 冒号 空格 一对单引号，注释内容放在单引号中

## 5.here Document 

格式

```bash
command << token

text

token

```

示例

```bash
pi@raspberrypi:~ $ cat helloscript.sh
#! /bin/bash

cat <<  heredoc
" this is a test"
'this is a test'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>this is a test</h1>
</body>
</html>

heredoc

pi@raspberrypi:~ $
```

```
pi@raspberrypi:~ $ ./helloscript.sh
" this is a test"
'this is a test'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>this is a test</h1>
</body>
</html>

pi@raspberrypi:~ $
```

* 为什么用 here Document ?
  1. 对单 双 引号 不敏感
  2. 对特殊符号不敏感，直接输出



## 6.条件声明 conditional statement

* ### if

  ```bash
  #! /bin/bash
  # count=10 要写在一起 count = 10 不行
  count=19
  
  if [ $count -eq 10 ]
          # not equal to   -ne  不等于
  then
          echo "count is 10"
  else
          echo " flase"
  fi
  ```

* ### if elif

  ```bash
  #! /bin/bash
  # count=10 要写在一起 count = 10 不行
  count=19
  
  if [ $count -ne 10 ]
          # not equal to   -ne  不等于
          # greater than   -gt  大于
  : '
  if (( $count > 9 ))
  
  '
  
  then
          echo "count is 10"
  elif (($count >= 9))
  then
          echo "count is greater than 9"
  else
          echo " flase"
  fi
  ```

* 且  运算符  and operator `-a` 

  ```bash
  #! /bin/bash
  
  age=10
  #if [ "$age" -gt 18 ] && [ "$age" -lt 40 ]
  if [ "$age" -gt 18 -a "$age" -lt 40  ]
  #   ^--> 这里要有空格，不然会错误
  then
          echo "age is 18~40"
  else
          echo "age is not in 18~40"
  fi
  
  ```

* 或  运算符  or    `-o`    `||` 

  ```bash
  #! /bin/bash
  
  age=10
  #if [ "$age" -gt 18 -o "$age" -lt 40  ]
  if [ "$age" -gt 18 ] || [ "$age" -lt 40 ]
  #if [[ "$age" -gt 18 || "$age" -lt 40 ]]
  then
          echo "age is 18~40"
  else
          echo "age is not in 18~40"
  fi
  
  ```

  两者都为假  表达式为假



* case statement





```
# 输入预设的发送邮箱和接受邮箱
        echo "To: 2079209547@qq.com" > mailText.txt
        echo "From: 2079209547@qq.com" >> mailText.txt
        echo "Cc: 2079209547@qq.com" >> mailText.txt
        echo "Subject: 系统邮件 " >> mailText.txt
        echo -e "\n" >> mailText.txt
```













## 7.Loops

* ### while loop   一直循环，直到 条件 变为假

  ```
  #! /bin/bash
  
  number=1
  
  while [ $number -lt 10  ]
  #while [ $number -le 10  ]
  do
          echo "$number"
          number=$(( number+1 ))
  done
  ```

  output

  ```
  pi@raspberrypi:~ $ ./helloscript.sh
  1
  2
  3
  4
  5
  6
  7
  8
  9
  ```

* ### until loop     一直循环，直到条件变为真, 停止

  ```bash
  #! /bin/bash
  
  number=1
  
  until [ $number -ge 10  ]
  do
          echo "$number"
          number=$(( number+1 ))
  done
  ```

* ### for  loop

  ```bash
  #! /bin/bash
  # 打印出 0 到 100
  #for i in 1 2 3 4 5
  for i in {0..100}
  do
          echo $i
  done
  ```

  每个 10  个数 打印一次, 

  格式

  ```bash
  for i in {StartPoint..EndPoint..increment}
  do
  		# your code
  done
  ```

  

  ```bash
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  
  #for i in 1 2 3 4 5
  for i in {0..100..10}
  do
          echo $i
  done
  pi@raspberrypi:~ $ ./helloscript.sh
  0
  10
  20
  30
  40
  50
  60
  70
  80
  90
  100
  ```

  另一种写法

  ```bash
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  
  #for i in 1 2 3 4 5
  for (( i=0; i<5; i++))
  do
          echo $i
  done
  pi@raspberrypi:~ $ ./helloscript.sh
  0
  1
  2
  3
  4
  pi@raspberrypi:~ $
  ```

* break

  ```bash
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  
  #for i in 1 2 3 4 5
  for (( i=0; i<=10; i++))
  do
          if [ $i -gt 5  ]
          then
                  break
          fi
          echo $i
  done
  pi@raspberrypi:~ $ ./helloscript.sh
  0
  1
  2
  3
  4
  5
  ```

* continue

  ```
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  
  #for i in 1 2 3 4 5
  for (( i=0; i<=10; i++))
  do
          if [ $i -gt 5  ]
          then
                  break
          fi
          echo $i
  done
  pi@raspberrypi:~ $ vim helloscript.sh
  pi@raspberrypi:~ $
  pi@raspberrypi:~ $ ./helloscript.sh
  0
  1
  2
  3
  4
  6
  8
  9
  10
  
  ```



## 8.script input

 数组

```bash
pi@raspberrypi:~ $ cat helloscript.sh
#! /bin/bash

# h数组
args=("$@")
# 打印出数组前三个
# echo ${args[0]} ${args[1]} ${args[2]}


# 打印出所有数组
echo $@
# 打印出数组的长度
echo $#
pi@raspberrypi:~ $
```

* 从一个文件中读取数据

  ```bash
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  
  while read line
  do
          echo "$line"
  done < "${1:-/dev/stdin}"
  pi@raspberrypi:~ $ ./helloscript.sh file.txt
  hello this is a test
  pi@raspberrypi:~ $
  ```

  

## 9.script output  脚本输出

1. 标准输出 STDOUT
2. 错误输出  STDERR

* 标准输出

  ```
  pi@raspberrypi:~ $ ls -al
  total 3912
  drwxr-xr-x 8 pi   pi      4096 Feb  4 15:44  .
  drwxr-xr-x 3 root root    4096 Feb 13  2020  ..
  -rw------- 1 pi   pi      2873 Feb  4 15:42  .bash_history
  -rw-r--r-- 1 pi   pi       220 Feb 13  2020  .bash_logout
  -rw-r--r-- 1 pi   pi      3523 Feb 13  2020  .bashrc
  drwxr-xr-x 3 pi   pi      4096 Feb  4 12:56  .cache
  drwx------ 5 pi   pi      4096 Feb  4 13:06  .config
  -rw-r--r-- 1 pi   pi        21 Feb  3 19:37  file.txt
  drwx------ 3 pi   pi      4096 Feb  2 19:58  .gnupg
  -rwxr-xr-x 1 pi   pi        73 Feb  4 15:44  helloscript.sh
  -rw-r--r-- 1 pi   pi   3929355 Feb  4 13:29 'Illenium&Nevve-Fractures.mp3'
  drwxr-xr-x 5 pi   pi      4096 Feb  4 13:24  .local
  drwxr-xr-x 2 pi   pi      4096 Feb 14  2020  MagPi
  -rw-r--r-- 1 pi   pi       807 Feb 13  2020  .profile
  drwx------ 2 pi   pi      4096 Feb  2 21:43  .ssh
  -rw-r--r-- 1 pi   pi        32 Feb  3 19:50  test.txt
  -rw------- 1 pi   pi     11199 Feb  4 15:44  .viminfo
  ```

* 错误输出

  ```
  pi@raspberrypi:~ $ ls +al
  ls: cannot access '+al': No such file or directory
  pi@raspberrypi:~ $
  ```

* `1`   表示标准输出    `2`  表示 错误输出

  ```
  pi@raspberrypi:~ $ cat helloscript.sh
  #! /bin/bash
  
  ls -al 1>out_1.txt  2>out_2.txt
  ```

  运行

  ```
  pi@raspberrypi:~ $ ./helloscript.sh
  pi@raspberrypi:~ $ ls
   file.txt                        MagPi       test.txt
   helloscript.sh                  out_1.txt
  'Illenium&Nevve-Fractures.mp3'   out_2.txt
  pi@raspberrypi:~ $ cat out_1.txt
  total 3912
  drwxr-xr-x 8 pi   pi      4096 Feb  4 16:29 .
  drwxr-xr-x 3 root root    4096 Feb 13  2020 ..
  -rw------- 1 pi   pi      2933 Feb  4 16:15 .bash_history
  -rw-r--r-- 1 pi   pi       220 Feb 13  2020 .bash_logout
  -rw-r--r-- 1 pi   pi      3523 Feb 13  2020 .bashrc
  drwxr-xr-x 3 pi   pi      4096 Feb  4 12:56 .cache
  drwx------ 5 pi   pi      4096 Feb  4 13:06 .config
  -rw-r--r-- 1 pi   pi        21 Feb  3 19:37 file.txt
  drwx------ 3 pi   pi      4096 Feb  2 19:58 .gnupg
  -rwxr-xr-x 1 pi   pi        50 Feb  4 16:29 helloscript.sh
  -rw-r--r-- 1 pi   pi   3929355 Feb  4 13:29 Illenium&Nevve-Fractures.mp3
  drwxr-xr-x 5 pi   pi      4096 Feb  4 13:24 .local
  drwxr-xr-x 2 pi   pi      4096 Feb 14  2020 MagPi
  -rw-r--r-- 1 pi   pi         0 Feb  4 16:29 out_1.txt
  -rw-r--r-- 1 pi   pi         0 Feb  4 16:29 out_2.txt
  -rw-r--r-- 1 pi   pi       807 Feb 13  2020 .profile
  drwx------ 2 pi   pi      4096 Feb  2 21:43 .ssh
  -rw-r--r-- 1 pi   pi        32 Feb  3 19:50 test.txt
  -rw------- 1 pi   pi     11277 Feb  4 16:29 .viminfo
  pi@raspberrypi:~ $ cat out_2.
  cat: out_2.: No such file or directory
  pi@raspberrypi:~ $ cat out_2.txt
  pi@raspberrypi:~ $
  ```

* 将标准输出与错误输出放到同一个文件里

  ```bash
  #! /bin/bash
  
  ls -al>out.txt 2>&1
  ```

  output

  ```
  pi@raspberrypi:~ $ ls
   file.txt         out.txt
   helloscript.sh   MagPi                           test.txt
  pi@raspberrypi:~ $ cat out.txt
  total 3912
  drwxr-xr-x 8 pi   pi      4096 Feb  4 16:35 .
  drwxr-xr-x 3 root root    4096 Feb 13  2020 ..
  -rw------- 1 pi   pi      2933 Feb  4 16:15 .bash_history
  -rw-r--r-- 1 pi   pi       220 Feb 13  2020 .bash_logout
  -rw-r--r-- 1 pi   pi      3523 Feb 13  2020 .bashrc
  drwxr-xr-x 3 pi   pi      4096 Feb  4 12:56 .cache
  drwx------ 5 pi   pi      4096 Feb  4 13:06 .config
  -rw-r--r-- 1 pi   pi        21 Feb  3 19:37 file.txt
  drwx------ 3 pi   pi      4096 Feb  2 19:58 .gnupg
  -rwxr-xr-x 1 pi   pi        40 Feb  4 16:35 helloscript.sh
  -rw-r--r-- 1 pi   pi   3929355 Feb  4 13:29 Illenium&Nevve-Fractures.mp3
  drwxr-xr-x 5 pi   pi      4096 Feb  4 13:24 .local
  drwxr-xr-x 2 pi   pi      4096 Feb 14  2020 MagPi
  -rw-r--r-- 1 pi   pi         0 Feb  4 16:35 out.txt
  -rw-r--r-- 1 pi   pi       807 Feb 13  2020 .profile
  drwx------ 2 pi   pi      4096 Feb  2 21:43 .ssh
  -rw-r--r-- 1 pi   pi        32 Feb  3 19:50 test.txt
  -rw------- 1 pi   pi     11320 Feb  4 16:35 .viminfo
  pi@raspberrypi:~ $ cat helloscript.sh
  ```

  * 将标准输出与错误输出的另一种写法

    ```bash
    #! /bin/bash
    
    ls -al>& out.txt
    ```



## 10.Send  output from one to another

多个脚本互动

```
pi@raspberrypi:~ $ cat helloscript.sh
#! /bin/bash

message="你好哇"
export message
./secondscript.sh
pi@raspberrypi:~ $ cat secondscript.sh
#! /bin/bash
echo "收到的消息是：$message "
pi@raspberrypi:~ $ ./helloscript.sh
收到的消息是：你好哇
pi@raspberrypi:~ $
```

## 11 大小写

```
-->~/Documents/bash_toturial$
-->~/Documents/bash_toturial$ cat test.sh
#!/bin/bash
#! /bin/bash

echo "enter 1st string"
read st1
echo "enter 2st string"
read st2

echo ${st1^}
echo ${st2^^}
-->~/Documents/bash_toturial$ ./test.sh
enter 1st string
hi
enter 2st string
hello
Hi
HELLO
-->~/Documents/bash_toturial$
```



## 12  数字与运算符 Numbers And Arithmetic

```bash
#!/bin/bash

# 不会打印出33 只会打印出 12+21
echo 12+21

num1=12
num2=21
echo -e "============================"
# 打印出 33
echo $((num1+num2))
echo $((num1-num2))
echo $((num1*num2))
echo $((num1/num2))
#           ^--> 除法结果为整数不是浮点数


echo -e "============================"


echo $(expr $num1 + $num2)
echo $(expr $num1 - $num2)
echo $(expr $num1 \* $num2)
echo $(expr $num1 / $num2)

```

* 十进制，十六进制 转换

  十六进制转为 十进制

  ```
  #!/bin/bash
  
  echo "请输入你要转换的十六进制数"
  read Hex
  
  echo -n "该数的十进制为: "
  
  echo " obase=10; ibase=16; $Hex" | bc
  ```

  run

  ```
  -->~/Documents/bash/bash_toturial$ ./test.sh
  请输入你要转换的十六进制数
  FFFF
  该数的十进制为: 65535
  ```

  

## 13.declear command

```
-->~/Documents/bash/bash_toturial$ declare -p
declare -- BASH="/bin/bash"
declare -r BASHOPTS="checkwinsize:cmdhist:complete_fullquote:expand_aliases:extglob:extquote:force_fignore:globasciiranges:histappend:interactive_comments:login_shell:progcomp:promptvars:sourcepath"
declare -i BASHPID
...........
...........
...........
...........
tf|txt|htm|html|?(f)odt|ott|odm|pdf)" [loimpress]="!*.@(sxi|sti|pps?(x)|ppt?([mx])|pot?([mx])|?(f)odp|otp)" [epiphany]="!*.@(?([xX]|[sS])[hH][tT][mM]?([lL]))" [modplugplay]="!*.@(669|abc|am[fs]|d[bs]m|dmf|far|it|mdl|m[eo]d|mid?(i)|mt[2m]|oct|okt?(a)|p[st]m|s[3t]m|ult|umx|wav|xm)" [dvipdf]="!*.dvi" [dillo]="!*.@(?([xX]|[sS])[hH][tT][mM]?([lL]))" [fbxine]="!*@(.@(mp?(e)g|MP?(E)G|wm[av]|WM[AV]|avi|AVI|asf|vob|VOB|bin|dat|divx|DIVX|vcd|ps|pes|fli|flv|FLV|fxm|FXM|viv|rm|ram|yuv|mov|MOV|qt|QT|web[am]|WEB[AM]|mp[234]|MP[234]|m?(p)4[av]|M?(P)4[AV]|mkv|MKV|og[agmvx]|OG[AGMVX]|t[ps]|T[PS]|m2t?(s)|M2T?(S)|mts|MTS|wav|WAV|flac|FLAC|asx|ASX|mng|MNG|srt|m[eo]d|M[EO]D|s[3t]m|S[3T]M|it|IT|xm|XM)|+([0-9]).@(vdr|VDR))?(.@(crdownload|part))" )
declare -- snap_bin_path="/snap/bin"
declare -- snap_xdg_path="/var/lib/snapd/desktop"
```



## 14. 数组

* 打印出一组数组

  ```bash
  #!/bin/bash
  
  car=('bmw' 'Toyota' 'Hondo')
  echo "${car[@]}"
  # 选择特定数组中的值
  echo "${car[0]}"
  # 打印当前数组的值位置信息
  echo "${！car[@]}"
  ```

  output

  ```
  bmw Toyota Hondo
  bmw
  0 1 2
  ```

* 计算数组的长度

  ```
  #!/bin/bash
  
  car=('bmw' 'Toyota' 'Hondo')
  echo "${#car[@]}"
  ```

  output

  ```
  3
  ```

* 删除数组中某一值

  ```bash
  #!/bin/bash
  
  car=('bmw' 'Toyota' 'Hondo')
  echo "${car[@]}"
  unset car[1]
  echo "${car[@]}"
  ```

  output

  ```
  bmw Toyota Hondo
  bmw Hondo
  ```

* 增加数组中某一值

  ```bash
  #!/bin/bash
  
  car=('bmw' 'Toyota' 'Hondo')
  echo "${car[@]}"
  unset car[1]
  echo "${car[@]}"
  car[1]="Toyota"
  echo "${car[@]}"
  ```

  output

  ```
  bmw Toyota Hondo
  bmw Hondo
  bmw Toyota Hondo	
  ```





## 15. 函数

* 函数的格式与调用

  ```bash
  #!/bin/bash
  
  function functionName()
  {
          echo "this is a new function"
  
  }
  functionName
  
  # 可以给函数一个参数
  # 打印函数
  
  function funcPrint()
  {
          echo $1 
          echi $1  $2  $3  $4
  } 
  
  funcPrint thisIsTheFunctionArgument
  ```

  output

  ```
  this is a new function
  this Is The Function Argument
  ```

* 检查 函数 是否 可用

  ```bash
  #!/bin/bash
  
  function functionCheck()
  {
          returningvalue="using this function right now"
          echo "$returningvalue"
  }
  functionCheck
  ```

  output

  ```
  using this function right now
  ```

* bash 脚本从上到下， 逐条执行，语句的执行优先于函数语句

  ```bash
  #!/bin/bash
  
  function functionCheck()
  {
          returningvalue="i love linux"
  }
          returningvalue="i love mac"
          echo "$returningvalue"
  
  functionCheck
          echo "$returningvalue"
  ```

  output

  ```
  i love mac
  i love linux
  ```

  

## 16. 文件和目录

* 创建目录

  ```bash
  #!/bin/bash
  
  mkdir -p Directory2
  ```

  output

  ```
  -->~/Documents/bash/bash_toturial$ ./test.sh
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  Directory2  test.sh
  ```

* 检查目录是否存在

  ```bash
  #!/bin/bash
  
  echo "输入目录的名称"
  read direct
  
  if [ -d "$direct" ]
  then
          echo "$direct 存在"
  else
          echo "$direct 不存在"
  fi
  ```

  output

  ```
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  Directory2  test.sh
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入目录的名称
  hifrhvs
  hifrhvs 不存在
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入目录的名称
  Directory2
  Directory2 存在
  -->~/Documents/bash/bash_toturial$
  ```

* 创建文件

  ```bash
  #!/bin/bash
  
  echo "输入要创建的文件名称"
  read fileName
  
  touch $fileName
  
  ```

  output

  ```
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要创建的文件名称
  dic 
  -->~/Documents/bash/bash_toturial$
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  dic  Directory2  test.sh
  ```

* 检查文件是否存在

  ```bash
  #!/bin/bash
  
  echo "输入要检查的文件名称"
  read fileName
  
  if [ -f "$fileName" ]
  then
          echo "$fileName 已创建"
  else
          echo "$fileName 未创建"
  fi
  ```

  output

  ```
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  dic  Directory2  test.sh
  -->~/Documents/bash/bash_toturial$
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要检查的文件名称
  dic
  dic 已创建
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要检查的文件名称
  dcsw
  dcsw 未创建
  ```

* 将文本内容输入文件

  ```bash
  #!/bin/bash
  
  echo "输入要输入的文件的名称"
  read fileName
  
  if [ -f "$fileName" ]
  then
          echo "$fileName 已创建"
          echo "输入需要输入的文本"
          read fileText
          echo "$fileText" >> $fileName
  
  else
          echo "$fileName 未创建"
  fi
  ```

  output

  ```
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  dic  Directory2  test.sh
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要输入的文件的名称
  dic
  dic 已创建
  输入需要输入的文本
  this is a test that I want to put in this file
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  dic  Directory2  test.sh
  ```

  *`>>`*  不会抹去之前的数据  *`>`*  会抹去之前的数据

  ```
  -->~/Documents/bash/bash_toturial$ cat dic
  this is a test that I want to put in this file
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要输入的文件的名称
  dic
  dic 已创建
  输入需要输入的文本
  this is the second test that i put it in the dic file
  -->~/Documents/bash/bash_toturial$ cat dic
  this is a test that I want to put in this file
  this is the second test that i put it in the
  -->~/Documents/bash/bash_toturial$
  
  ```

* 读取文件内容

  ```bash
  #!/bin/bash
  
  echo "输入要读取的文件的名称"
  read fileName
  
  if [ -f "$fileName" ]
  then
          while IFS= read -r line
          do
                  echo "$line"
          done < $fileName
  
  else
          echo "$fileName 未创建"
  fi 
  ```

* 删除文件

  script

  ```bash
  #!/bin/bash
  
  echo "输入要删除的文件的名称"
  read fileName
  
  if [ -f "$fileName" ]
  then
  
          rm $fileName
          echo "$fileName has been removed successfully"
  else
          echo "can not find $fileName  file"
  fi
  ```

  output

  ```
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  dic  Directory2  test.sh
  -->~/Documents/bash/bash_toturial$ 
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要删除的文件的名称
  dic
  dic has been removed successfully
  -->~/Documents/bash/bash_toturial$
  -->~/Documents/bash/bash_toturial$ ls
  bc.txt  declare.sh  Directory2  test.sh
  -->~/Documents/bash/bash_toturial$ ./test.sh
  输入要删除的文件的名称
  dic
  can not find dic  file
  -->~/Documents/bash/bash_toturial$
  ```

  

## 17.  使用脚本发送邮件   send e-mail via script

* 安装	`ssmtp`   `sudo apt-get install ssmtp` 

* 设置配置文件   `/ect/ssmtp/ssmtp.conf` 

  ssmpt.config

  ```c
  root=xxxxxxxxxx@qq.com
  mailhub=smtp.qq.com:587
  AuthUser=xxxxxxxxxx@qq.com
  AuthPass=xxxxxxxxxxxxxx
  UseSTARTTLS=yes
  UseTLS=Yes
  FromLineOverride=YES
  ```

  **NOTE** : AuthPass 为 QQ 认证的授权码  具体如何获得授权码 可参考  https://service.mail.qq.com/cgi-bin/help?subtype=1&id=28&no=1001256 

  ​			其他邮箱可自行实验

  

  

  创建 文本文件 ` touch mailText.txt`

  脚本

  ```bash
  #! /bin/bash
  
  if [ -f "mailText.txt" ]
  then
          read mailtxt
          # 输入预设的发送邮箱和接受邮箱
          echo "To: xxxxxxxxxx@qq.com" > mailText.txt
          echo "From: xxxxxxxxxx@qq.com" >> mailText.txt
          echo "Cc: xxxxxxxxxx@qq.com" >> mailText.txt
          echo "Subject: 系统邮件 " >> mailText.txt
          echo -e "\n" >> mailText.txt
  
          # 将 邮件内容追加到 预设后
          echo "$mailtxt">> mailText.txt
  else
          echo "未找到 mailText.txt "
  fi
  # 发送邮件
  sendmail -t < mailText.txt
  ```

  

##  18. curl 

**下载文件**

```bash
#! /bin/bash
url="http://ovh.net/files/1Mio.dat"
curl ${url} -o
# 保存为原文件名称
```

**output**

```
-->~/Documents/bash/bash_toturial$ ./curl.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1024k  100 1024k    0     0   6736      0  0:02:35  0:02:35 --:--:-- 18321
-->~/Documents/bash/bash_toturial$
-->~/Documents/bash/bash_toturial$ ls
1Mio.dat
```

```bash
#! /bin/bash
url="http://ovh.net/files/1Mio.dat"
curl ${url} -o NewFileDownload   
# 保存 文件名为 NewFileDownload 

# 另一种写法 
curl ${url} > NewFileDownload 
```

ouput

```
NewFileDownload
```

* 检查下载的文件 ( the header of the file )

  ```bash
  #! /bin/bash
  url="http://ovh.net/files/1Mio.dat"
  curl -I ${url}
  ```

  output

```
-->~/Documents/bash/bash_toturial$ ./curl.sh
HTTP/1.1 200 OK
Date: Mon, 22 Feb 2021 02:08:51 GMT
Server: Apache
Last-Modified: Thu, 16 Sep 2010 11:42:20 GMT
ETag: "362c002-100000-4905ef050df00"
Accept-Ranges: bytes
Content-Length: 1048576
Connection: close
Content-Type: application/octet-stream
```







## 19  专业的菜单

* 等待输入

```bash
#! /bin/bash

select name in 小明 小李 小芳 小张 小刘
do
        echo "you have slected $car "
done
```

output

```
-->~/Documents/bash/bash_toturial$ ./menus.sh
1) 小明
2) 小李
3) 小芳
4) 小张
5) 小刘
#? 1
you have slected 小明
#? 2
you have slected 小李
#? 3
you have slected 小芳
#? 4
you have slected 小张
#? 5
you have slected 小刘
#? 6
you have slected
#? 7
you have slected
#? 8
you have slected
#? 9
you have slected
#? ^C
-->~/Documents/bash/bash_toturial$
```

脚本

```bash
#! /bin/bash

select name in 小明 小李 小芳 小张 小刘
do
        case $name in
               小明)
                echo "小明 已选择";;
               小李)
                echo "小李 已选择";;
               小芳)
                echo "小芳 已选择";;
               小张)
                echo "小张 已选择";;
               小刘)
                echo "小刘 已选择";;
               *)
                echo "错误! 请在 1~5 之间选择"
        esac
done
```

output

```
-->~/Documents/bash/bash_toturial$ ./menus.sh
1) 小明
2) 小李
3) 小芳
4) 小张
5) 小刘
#? 1
小明 已选择
#? 2
小李 已选择
#? 3
小芳 已选择
#? 4
小张 已选择
#? 5
小刘 已选择
#? 6
错误! 请在 1~5 之间选择
#? 7
错误! 请在 1~5 之间选择
#? 8
错误! 请在 1~5 之间选择
#? 9
错误! 请在 1~5 之间选择
#? 0
错误! 请在 1~5 之间选择
#? ^C
```



脚本

```bash
#! /bin/bash

echo -e "按任意键继续..."

while [ true ]
do
        read -t 3 -n 1
if [ $? = 0  ]
then
        echo "你已经终止脚本"
        exit;
else
        echo "等待按下任意键..."
fi

done
```

output

**等待用户按键 **

```
-->~/Documents/bash/bash_toturial$ ./waiting.sh
按任意键继续...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
^C
-->~/Documents/bash/bash_toturial$
-->~/Documents/bash/bash_toturial$ ./waiting.sh
按任意键继续...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...
等待按下任意键...

你已经终止脚本
-->~/Documents/bash/bash_toturial$
```



## 20.  监控文件系统    inotify

* ​	监控文件系统中文件的创建，写入，等等.....

1. 安装 `inotify`  `sudo apt-get install inotify-tools` 
2. 脚本

```bash
#! /bin/bash

inotifywait -m /home/clay/Documents/bash/bash_toturial/Directory2
```



```
root@clay:/home/clay/Documents/bash/bash_toturial#
root@clay:/home/clay/Documents/bash/bash_toturial# ./inotiyf.sh
Setting up watches.
Watches established.
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR test
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ATTRIB testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE .testfile.txt.swx
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN .testfile.txt.swx
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE .testfile.txt.swx
/home/clay/Documents/bash/bash_toturial/Directory2/ DELETE .testfile.txt.swx
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ DELETE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ MODIFY .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ ATTRIB .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ MODIFY .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE 4913
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN 4913
/home/clay/Documents/bash/bash_toturial/Directory2/ ATTRIB 4913
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE 4913
/home/clay/Documents/bash/bash_toturial/Directory2/ DELETE 4913
/home/clay/Documents/bash/bash_toturial/Directory2/ MOVED_FROM testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ MOVED_TO testfile.txt~
/home/clay/Documents/bash/bash_toturial/Directory2/ CREATE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ MODIFY testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ATTRIB testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ATTRIB testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ MODIFY .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ DELETE testfile.txt~
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ DELETE .testfile.txt.swp
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ MODIFY testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_WRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE testfile.txt
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ OPEN,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ ACCESS,ISDIR
/home/clay/Documents/bash/bash_toturial/Directory2/ CLOSE_NOWRITE,CLOSE,ISDIR
```





## 21 .    抓取命令   grep

脚本

```bash
#! /bin/bash
echo "输入搜索文件:"
read fileName

if [[ -f $fileName ]]
then
        echo "输入搜索文本"
        read grepvar
         grep --colour=always -i -n $grepvar $fileName
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$ ./grep.sh 
输入搜索文件:
grepfile.txt
输入搜索文本
1
2:this is grep test file 1
10:this is grep test file 1
```

更多关于 `grep` 参数 `man grep`







## 22 . awk

脚本

```bash
#! /bin/bash

echo "输入要从AWK打印的文件名 "
read fileName

if [[ -f $fileName  ]]
then
        awk '{print}' $fileName
        # awk /1/'{print}' $fileName    筛选出 有 1 的打印出来
        # awk /1/'{print $3}' $fileName    筛选出 有 1 的将第三个打印出来
        # awk /1/'{print $3，$4}' $fileName    筛选出 有 1 的将第三个打印出来
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$ ./awk.sh
输入要从AWK打印的文件名
 awkfile.txt
this is a awk test file
this is awk test file 1
this is awk test file 2
this is awk test file 3
this is awk test file 5
this is awk test file 7
this is awk test file 8
this is awk test file 9
this is awk test file 0
this is awk test file 1

-->~/Documents/bash/bash_toturial$
```

```
-->~/Documents/bash/bash_toturial$ ./awk.sh
输入要从AWK打印的文件名
awkfile.txt
this is awk test file 1
this is awk test file 1
-->~/Documents/bash/bash_toturial$

```

```
-->~/Documents/bash/bash_toturial$ ./awk.sh
输入要从AWK打印的文件名
awkfile.txt
awk
awk
```

```
-->~/Documents/bash/bash_toturial$ ./awk.sh
输入要从AWK打印的文件名
awkfile.txt
awk test
awk test
-->~/Documents/bash/bash_toturial$
```



## 22. sed   批量文本替换

**sed --> screen editor** 

* 替换文本

```bash
#! /bin/bash

echo "输入要使用sed替换内容文件的文件名"
read fileName

if [[ -f $fileName ]]
then
		# 将文件中单个字母且是第一个字母替换 （i 替换为 I）
         sed 's/i/I/' $fileName		# s  代表 substitution (替换)
        # 将文件中单个字母全部替换 （i 替换为 I）
         sed 's/i/I/g' $fileName     #  g  代表 global
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$ ./sed.sh
输入要使用sed替换内容文件的文件名
sedfile.txt
thIs is a sed test file
thIs is sed test file 1
thIs is sed test file 2
thIs is sed test file 3
thIs is sed test file 5
thIs is sed test file 7
thIs is sed test file 8
thIs is sed test file 9
thIs is sed test file 0
thIs is sed test file 1

```

全部替换 `i --> I` 

```
-->~/Documents/bash/bash_toturial$ ./sed.sh
输入要使用sed替换内容文件的文件名
sedfile.txt
thIs Is a sed test fIle
thIs Is sed test fIle 1
thIs Is sed test fIle 2
thIs Is sed test fIle 3
thIs Is sed test fIle 5
thIs Is sed test fIle 7
thIs Is sed test fIle 8
thIs Is sed test fIle 9
thIs Is sed test fIle 0
thIs Is sed test fIle 1

```

**NOTE :** 此时 文件中的字母并没有被改变， 打印出来的是 `i 被替换为 Ｉ` 文件中还是小写

替换脚本

```bash
#! /bin/bash

echo "输入要使用sed替换内容文件的文件名"
read fileName

if [[ -f $fileName ]]
then
         sed 's/i/I/g' > newFileName
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$
-->~/Documents/bash/bash_toturial$ ./sed.sh
输入要使用sed替换内容文件的文件名
sedfile.txt
-->~/Documents/bash/bash_toturial$ ls
newFileName      sed.sh		sedfile.txt
-->~/Documents/bash/bash_toturial$ cat newFileName
thIs Is a sed test fIle
thIs Is sed test fIle 1
thIs Is sed test fIle 2
thIs Is sed test fIle 3
thIs Is sed test fIle 5
thIs Is sed test fIle 7
thIs Is sed test fIle 8
thIs Is sed test fIle 9
thIs Is sed test fIle 0
thIs Is sed test fIle 1

-->~/Documents/bash/bash_toturial$
```



改变文件脚本 **（NOTE 不可撤回）**

```bash
#！ /bin/bash

echo "输入要使用sed替换内容文件的文件名"
read fileName

if [[ -f $fileName ]]
then
         sed -i 's/i/I/g' $fileName
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$ cat sedfile.txt
this is a grep test file
this is grep test file 1
this is grep test file 2
this is grep test file 3
this is grep test file 5
this is grep test file 7
this is grep test file 8
this is grep test file 9
this is grep test file 0
this is grep test file 1

-->~/Documents/bash/bash_toturial$ ./sed.sh
输入要使用sed替换内容文件的文件名
sedfile.txt
-->~/Documents/bash/bash_toturial$ cat sedfile.txt
thIs Is a grep test fIle
thIs Is grep test fIle 1
thIs Is grep test fIle 2
thIs Is grep test fIle 3
thIs Is grep test fIle 5
thIs Is grep test fIle 7
thIs Is grep test fIle 8
thIs Is grep test fIle 9
thIs Is grep test fIle 0
thIs Is grep test fIle 1

```

* 也可用于 单字 的替换 

脚本

```bash
#! /bin/bash

echo "输入要使用sed替换内容文件的文件名"
read fileName

if [[ -f $fileName ]]
then
         sed -i 's/test/TEST/g' $fileName
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$ cat sedfile.txt
this is a sed test file
this is sed test file 1
this is sed test file 2
this is sed test file 3
this is sed test file 5
this is sed test file 7
this is sed test file 8
this is sed test file 9
this is sed test file 0
this is sed test file 1

-->~/Documents/bash/bash_toturial$ ./sed.sh
输入要使用sed替换内容文件的文件名
sedfile.txt
-->~/Documents/bash/bash_toturial$ cat sedfile.txt
this is a sed TEST file
this is sed TEST file 1
this is sed TEST file 2
this is sed TEST file 3
this is sed TEST file 5
this is sed TEST file 7
this is sed TEST file 8
this is sed TEST file 9
this is sed TEST file 0
this is sed TEST file 1

```



## 23. bash  脚本调试

`bash -x 调试脚本.sh`

脚本的调试实际上是 将脚本的执行 一步一步的展现出来

例

```bash
#! /bin/bash

echo "输入要使用sed替换内容文件的文件名"
read fileName

if [[-f $fileName ]]	# 此处有错误，-f 的左边应该有 空格	
then
         sed -i 's/test/TEST/g' $fileName
else
        echo "$fileName 不存在"
fi
```

调试

```
-->~/Documents/bash/bash_toturial$ bash -x sed.sh
+ echo 输入要使用sed替换内容文件的文件名
输入要使用sed替换内容文件的文件名
+ read fileName
sedfile.txt
+ '[[-f' sedfile.txt ']]'
sed.sh: line 6: [[-f: command not found
+ echo 'sedfile.txt 不存在'
sedfile.txt 不存在
```

也可以在脚本中直接 添加  `-x`

```bash
#! /bin/bash -x

echo "输入要使用sed替换内容文件的文件名"
read fileName

if [[-f $fileName ]]	# 此处有错误，-f 的左边应该有 空格	
then
         sed -i 's/test/TEST/g' $fileName
else
        echo "$fileName 不存在"
fi
```

output

```
+ echo 输入要使用sed替换内容文件的文件名
输入要使用sed替换内容文件的文件名
+ read fileName
sedfile.txt
+ '[[-f' sedfile.txt ']]'
./sed.sh: line 6: [[-f: command not found
+ echo 'sedfile.txt 不存在'
sedfile.txt 不存在
```



* 设置调试断点

```bash
-->~/Documents/bash/bash_toturial$ cat sed.sh
#! /bin/bash -x

# 调试断点

set -x
echo "输入要使用sed替换内容文件的文件名"
read fileName

set -x

if [[-f $fileName ]]
then
         sed -i 's/test/TEST/g' $fileName
else
        echo "$fileName 不存在"
fi
```

output

```
-->~/Documents/bash/bash_toturial$ ./sed.sh
+ set -x
+ echo 输入要使用sed替换内容文件的文件名
输入要使用sed替换内容文件的文件名
+ read fileName
sedfile.txt
+ set +x    // ==> 在这个点调试结束
```

