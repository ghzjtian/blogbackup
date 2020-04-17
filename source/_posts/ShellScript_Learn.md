---
title: 学习 Shell Script 
tags: [Linux]
categories: [Linux]
date: 2020-02-02 17:46:19
---

> 学习 Shell script .

<!-- more -->

## 1.参考
## 2.导入其它的 Script 文件.
## 3.Demo

***
***
***

## 1.参考
* [1.Linux Tutorial](https://ryanstutorials.net/linuxtutorial/)
* [2.Bash Scripting Tutorial](https://ryanstutorials.net/bash-scripting-tutorial/)
* [3.A shell script which turns your Mac into an awesome web development machine](https://github.com/monfresh/laptop)

## 2.导入其它的 Script 文件.

```
#!/bin/sh

#[How best to include other scripts?](https://stackoverflow.com/questions/192292/how-best-to-include-other-scripts)
my_dir="$(dirname "$0")"

"$my_dir/other_script.sh"
```



## 3.Demo

* 1.打印 Bash 的版本号

```
$ echo $BASH_VERSION
3.2.57(1)-release
```

* 2.只读属性 

```
readonly var
```

* 1.Demo 1检查 file 是否存在

```
usage() {
    echo "You need to provide an argumennt:"
    echo "usage: $0 file_name"
}
is_file_exist() {
    local file="$1"
    [[ -f "$file" ]] && return 0 || return 1
}

## 如果是 0， 即没有参数输入的情况，就执行后面的 usage.
[[ $# -eq 0 ]] && usage

## 如果返回 0 ， 就代表找到了 file.
if ( is_file_exist "$1" )
then
    echo "File found"
else
    echo "File not found"
fi

```

* 2.方法的返回值

> 方法的返回值会保存在 `$?` 中，而且 `$?` 一旦获取过一次后，就会自动变为 0.

* 0.参考: [Returning from a function](https://bash.cyberciti.biz/guide/Returning_from_a_function)

* 1.用 `$?`

```
function test_return() {
    sum=$(($1 + $2))
    return $sum
}

echo "(before)test_return:$?"
test_return 1 2
echo "test_return:$?"

test_return 3 4
echo "test_return:$?"
```

* 2.返回 string

```
domain="CyberCiti.BIz"
out=""

##################################################################
# Purpose: Converts a string to lower case
# Arguments:
#   $@ -> String to convert to lower case
##################################################################
function to_lower()
{
    local str="$@"
    local output
    output=$(tr '[A-Z]' '[a-z]'<<<"${str}")
    echo $output
}

# invoke the to_lower()
to_lower "This Is a TEST"

# invoke to_lower() and store its result to $out variable
out=$(to_lower ${domain})

# Display  back the result from $out
echo "Domain name : $out"
```

### 1.PID 相关.
##### 1.打印 pid

```
#echo "pid is $$"
```

###### 2.kill pid 所在的程序.

```
$ kill -9 pid
```

### 2.Debug 的方法
##### * 1.在运行的时候添加 bash -x 命令, 如

```
$ bash -x ./file_name
```

##### * 2.在文件的头部添加 -x, 如

```
#!/bin/bash -x
```

##### * 3.或者在文件的中部位置添加 set -x , set +x 如:

```
echo "1"
set -x
echo "2"
echo "3"
echo "4"
set +x
echo "5"
```

### 3.与或操作命令符

* [What is the purpose of “&&” in a shell command?](https://stackoverflow.com/questions/4510640/what-is-the-purpose-of-in-a-shell-command)


```
# command_1 && command_2: execute command_2 only when command_1 is executed successfully.
# command_1 || command_2: execute command_2 only when command_1 is not successful executed.

$ false || echo "Oops, fail"
Oops, fail

$ true || echo "Will not be printed"
$  

$ true && echo "Things went well"
Things went well

$ false && echo "Will not be printed"
$

$ false ; echo "This will always run"
This will always run
```

### 4.判断用户是否是 root

```
# Purpose: Determine if current user is root or not
is_root_user(){
 # root user has user id (UID) zero.
 [ $(id -u) -eq 0 ] && return $TRUE || return $FALSE
}

is_root_user && echo "You can run this script." || echo "You need to run this script as a root user."

```

### 5.判断 True of False

* 1.参考:[How does bash test 'false'?](https://superuser.com/questions/1400335/how-does-bash-test-false)

```
declare -r TRUE=true
declare -r FALSE=false

if [ $TRUE = true ]
then
    echo "TRUE"
else
    echo "FALSE"
fi
```

* 1.`-eq` 是判断 数字类型的, `=` 判断 string 类型

```
declare -r five=05
declare -r siz=06
if [[ $five -eq 5 ]]; then echo "equal"; else echo "nope"; fi
# print: equal
if [[ $five -eq 6 ]]; then echo "equal"; else echo "nope"; fi
# print: nope
if [[ $five = 5 ]]; then echo "equal"; else echo "nope"; fi
# print: nope
if [[ $five = 6 ]]; then echo "equal"; else echo "nope"; fi
# print: nope
```

* 2.`while` 判断

```
flag=1
while ((flag))
do
   read x
   [ "$x" == "true" ] && flag=0
   echo "${x} : ${flag}"
done

```

* 3.数字

```
declare -r TRUE=0
declare -r FALSE=1

condition=$TRUE

if [[ $condition -eq $TRUE ]]
then
    echo "true"
else
    echo "false"
fi


```