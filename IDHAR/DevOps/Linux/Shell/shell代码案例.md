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

#发送消息

#!/bin/bash

查看用户是否登录
login_user=$(who | grep -i -m 1 $1 | awk '{print $1}')

if [ -z $login_user ]
then 
	echo "$1 不在线！"
	echo "脚本退出..."
	exit
fi

查看用户是否开启消息功能
is_allowed=$(who -T | grep -i -m 1 $1 | awk '{print $2}')

if [ $is_allowed != "+" ]
then 
	echo "$1 没有开启消息功能！"
	echo "脚本退出..."
	exit
fi

确认是否有消息发送
if [ -z $2 ]
then 
	echo "没有消息发送！"
	echo "脚本退出..."
	exit
fi 

从参数中获取要发送的消息
whole_msg=$(echo $* | cut -d " " -f 2-)

获取用户登录的终端
user_terminal=$(who | grep -i -m 1 $1 | awk '{print $2}')

写入要发送的消息
echo $whole_msg | write $login_user $user_terminal

if [ $? != 0 ]
then 
	echo "发送失败"
else 
	echo "发送成功"
fi

exit