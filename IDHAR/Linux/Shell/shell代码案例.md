#自定义函数 
#!/bin/bash

function add(){
        s=$[$1+$2]
        echo $s
}

read -p "di yi ge shu zi:" a
read -p "di er ge shu zi:" b

add $a $b
