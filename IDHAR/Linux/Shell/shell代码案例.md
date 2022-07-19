#自定义函数 
#!/bin/bash

function add(){
        s=$[$1+$2]
        echo $s
}

read -p "di yi ge shu zi:" a
read -p "di er ge shu zi:" b

add $a $b

#自动归档 
#!/bin/bash

%%首先判断输入参数的个数是否为一%%
if [ $# -ne 1 ]
then
	echo "当前参数个数错误，应该输入一个目录参数"
	exit 1
fi

%%从参数中获取目录名称%%
if [ -d $1 ]
then
	echo 
else
	echo
	echo "目录不存在"
	echo
	exit
fi

DIR_NAME=$(basename $1)
DIR_PATH=$(cd $(dirname $1); pwd)

%%获取当前日期%%
DATE=$(date +%y%m%d)

%%定义生成的归档文件名称%%
FILE=archive_$DIR_NAME_$DATE.tar.gz
DEST=/root/backup/$FILE

%%归档目录文件%%
echo "开始归档..."
echo 

tar -czf $DEST $DIR_PATH/$DIR_NAME
%% tar 的目录必须存在，他不会新创建一个目录 %%

if [ $? -eq 0 ]
then 
	echo 
	echo "归档成功"
	echo "归档文件为：$DEST"
	echo
else
	echo "归档失败"
	echo
fi

exit

%%定时任务%%
crontab -e
0 18 * * * /root/Desktop/daily_archive /root/Documents
%%创建一个crontab每天晚上六点执行daily_archive脚本去备份Documents文件夹%%
crontab -l
%%查看这个crontab%%