---
title: shell脚本
date: 2021-05-04 19:55:39
tags: linux
categories: 
  - Linux
  - Sehll
cover: /images/banners/VCG41N1169192820.jpg
---



###  清屏
```shell
clear
```
### 输出换行，两个
```shell
echo -e "\n\n"
```
### 休息一秒
```shell
sleep 1
```
### 输出用户数
```shell
who | awk '{print $1}' | sort | uniq | wc -l
```
### 让计算机发出蜂鸣声
```shell
echo -en "\007"
```

### 判断文件夹是否存在
```shell
if [ ! -d "./QQ/" ];
then
  mkdir QQ
else
  echo "文件夹存在"
fi
```
### 文件目录追加到文件中
```shell
ls >> 文件名
```

### 输出白底黑字的文字
```shell
echo -e "\033[47;30m Now at your service,*Zhu-Zhi-Wu* \033[0m"
```

### 监听键盘输入任意键
```shell
read -n 1 -p "the End…"
```



### 输入三个数，输出最大数

```shell
printf "请输入三个书：\n"
read first_num
read secend_num
read thd_num
if [ $first_num -gt $secend_num ]
then
  max=$first_num
else
  max=$secend_num
fi

if [ $thd_num -gt $max ]
then
  max=$thd_num
fi

echo "$max"
```