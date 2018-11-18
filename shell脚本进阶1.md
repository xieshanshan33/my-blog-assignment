# SHELL脚本编程进阶1

## 条件选择if语句

if语句可嵌套

* 单分支

&ensp;&ensp;&ensp;&ensp; if 判断条件;then

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 条件为真的分支代码

&ensp;&ensp;&ensp;&ensp; fi

* 双分支

&ensp;&ensp;&ensp;&ensp; if 判断条件; then

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 条件为真的分支代码

&ensp;&ensp;&ensp;&ensp; else

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 条件为假的分支代码

&ensp;&ensp;&ensp;&ensp; fi

* 多分支

&ensp;&ensp;&ensp;&ensp; if 判断条件1; then

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 条件1为真的分支代码

&ensp;&ensp;&ensp;&ensp; elif 判断条件2; then

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 条件2为真的分支代码

&ensp;&ensp;&ensp;&ensp; elif 判断条件3; then

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 条件3为真的分支代码

&ensp;&ensp;&ensp;&ensp; else

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 以上条件都为假的分支代码

&ensp;&ensp;&ensp;&ensp; fi

## 条件判断： case语句

&ensp;&ensp;&ensp;&ensp; case 变量引用 in

&ensp;&ensp;&ensp;&ensp; PAT1)

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 分支1

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; ;;

&ensp;&ensp;&ensp;&ensp; PAT2)

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 分支2

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; ;;

&ensp;&ensp;&ensp;&ensp; ...

&ensp;&ensp;&ensp;&ensp; *)

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; 默认分支

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; ;;

&ensp;&ensp;&ensp;&ensp; esac

> 例题：输入成绩看你的成绩好坏

read -p "Please input your score: " score

if [[ !  "$score" =~ ^[0-9]+$  ]];then                                          
        echo "please input digit"

                exit

                elif [  "$score" -gt 100 ];then

                        echo "Your score is false"

                                exit 10 //判断输入是否大于100，满足则打印成绩错误并退出

                                elif [ $score -lt 60 ];then
                                        echo "no pass" //判断成绩是否小于60，满足则打印不通过

                                        elif [ $score -lt 80 ];then

                                                echo "so so"//判断成绩是否小于80，满足则打印一般般

                                                else

                                                     echo "very good" //都不满足上述条件则打印非常好

                                                     fi

>练习题
> 
> 1、编写脚本/root/bin/createuser.sh，实现如下功能：使用一个用户名做为参数，如果指定参数的用户存在，就显示其存在，否则添加之；显示添加的用户的id号等信息

read -p "please input username：" username //输入你的用户名，你输入的用户名为变量username

     useradd $username  &> /dev/null       //不存在就创建账号且不显示

       if [ $? -eq 0 ];then

               echo "add $username user" && id $username //显示添加的用户的id

           else

                echo "the user already exits"     //打印用户退出        
                                      
            fi

> 2、编写脚本/root/bin/yesorno.sh，提示用户输入yes或no,并判断用户输入的
是yes还是no,或是其它信息

read -p "please input yes or no:" ans //ans为变量

ans=`echo $ans | tr 'A-Z' 'a-z'`//把所以输入的大写字母变成小写

case $ans in

y|yes)

        echo "yes"

        ;;

n|no)

        echo "no"

                ;;

*)

        echo "写错了"

;;

esac

> 3、编写脚本/root/bin/filetype.sh,判断用户输入文件路径，显示其文件类型
（普通，目录，链接，其它文件类型）

read -p "please enter a file name:" path

if [ -L $path ];then

    echo "该$path 文件是符号链接文件"

  elif

        [ -f $path ];then

        echo "该$path 文件是普通文件"

 elif
 
         [ -b $path ];then

                 echo "该$path 文件是块设备文件"

 elif

         [ -c $path ];then

                 echo "该$path 文件是字符设备文件"
                 
 elif

         [ -d $path ];then

                 echo "该$path 文件是目录文件"

 elif

         [ -p $path ];then

                 echo "该$path 文件是管道"

 elif

         [ -S $path ];then

                 echo "该$path 文件是套接字文件"   

else

        echo "您的输入无效" && exit 1

        fi 

> 4、编写脚本/root/bin/checkint.sh,判断用户输入的参数是否为正整数

read -p "please input a digit：" int

  if [[ $int =~ (^[0-9]*[1-9][0-9]*$) ]];then

      echo "输入为整数"

      else
      
          echo "输入的不是整数"                           
                                                          
           fi

## for循环

for 变量名 in 列表;do

&ensp;&ensp;&ensp;&ensp; 循环体

done

依次将列表中的元素赋值给“变量名” ; 每次赋值后即执行一次循环体; 直
到列表中的元素耗尽，循环结束

列表生成方式：

&ensp;&ensp;&ensp;&ensp; (1) 直接给出列表

&ensp;&ensp;&ensp;&ensp; (2) 整数列表：

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; (a) {start..end}

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; (b) $(seq [start [step]] end)

&ensp;&ensp;&ensp;&ensp; (3) 返回列表的命令

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; $(COMMAND)

&ensp;&ensp;&ensp;&ensp; (4) 使用glob， 如： *.sh

&ensp;&ensp;&ensp;&ensp; (5) 变量引用；

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; $@, $*

## for特殊格式

双小括号方法，即((…))格式，也可以用于算术运算

双小括号方法也可以使bash Shell实现C语言风格的变量操作

&ensp;&ensp;&ensp;&ensp; I=10

&ensp;&ensp;&ensp;&ensp; ((I++))

for循环的特殊格式：

for ((控制变量初始化;条件判断表达式;控制变量的修正表达式))

do

&ensp;&ensp;&ensp;&ensp; 循环体

done

> for num in 1 3 5 7;do
> 
> echo num is $num;
> 
> done

控制变量初始化：仅在运行到循环代码段时执行一次

控制变量的修正表达式：每轮循环结束会先进行控制变量修正运算，而后再做条件判断

> 练习:

> 1、判断/var/目录下所有文件的类型

for i in /var/* ;do 
        
        if [ -b $i ];then
        
                echo "$i是块设备"
        
        elif [ -c $i ] ;then       

        echo "$i是字符设备"
        
        elif [ -f $i ];then    

                echo "$i是普通文件"
        
        elif [ -h $i ];then
                
                echo "$i是符号链接文件" 
        
        elif [ -p $i ];then        
                
                echo "$i是管道文件"
        
        elif [ -S $i ];then       
                
                echo "$i是套接字文件"

        elif [ -d $i ];then
                
                echo "$i是目录文件"
        
        else
        
                echo "文件或目录不存在" 
        fi
                done    

> 2、添加10个用户user1-user10，密码为8位随机字符

for i in `seq 1 10`

do

useradd user$i

   password=`head -c 500 /dev/urandom | tr -dc '[:alnum:]' | head
 -c 8`                   

      echo $password|passwd --stdin user$i

      done

> 3、 /etc/rc.d/rc3.d目录下分别有多个以K开头和以S开头的文件；分别读取每个文件，以K开头的输出为文件加stop，以S开头的输出为文件名加start，如K34filename stopS66filename start

K=`ls /etc/rc.d/rc3.d/ | egrep -i "^k.*"`

S=`ls /etc/rc.d/rc3.d/ | egrep -i "^s.*"`

for FK in $K;do

echo -e "$FK\tstop"

done

for FS in $S;do

echo -e "$FS\tstart"

done  

> 4、编写脚本，提示输入正整数n的值，计算1+2+…+n的总和

read -p "give me a number:" n

  sum=0

    for((i=0;i<=$n;i++))

      do

           sum=$[$sum+$i]

             done

               echo $sum

> 5、计算100以内所有能被3整除的整数之和

let SUM=0

 for I in {1..100}; do #取余运算

   if [ $[$I%3] -eq 0 ]; then

       SUM=$[$SUM+$I]

         fi

         done

          echo "SUM=$SUM"

> 6、编写脚本，提示请输入网络地址，如192.168.0.0，判断输入的网段中主机在线状态

 file=`mktemp /tmp/pingip.XXXXXX`
 
  read -p "请输入网络地址 :" ip i=`echo $ip | cut -d'.' -f1-3` 
  
  for z in {1..255};do

   { 

     ip=$i.$z 
        
     ping -c1 -W1 $ip &> /dev/null && (echo "$ip is up" ) && echo ip >> $file 
     
}&
     
done
     
      sleep 1 
      
      echo "`cat $file | wc -l`在线" rm -f $file &> /dev/null

> 7、打印九九乘法表

for i in `seq 9`

do

    for j in `seq 9`

            do

                    [ $j -le $i ] && echo -n "$i x $j = `echo $(($i*$j))`  "   # 如果 j 小与等于 i 才会打印式子
                        
                        done
                        
                        echo ""
                        
                        done

> 8、在/testdir目录下创建10个html文件,文件名格式为数字N（从1到10）加随机8个字母，如： 1AbCdeFgH.html

for i in {1..10};do  

         kk=$(openssl rand -base64 20|tr -dc [[:alpha:]]|cut -c1-8)
                
                 touch /data/testdir/"$i""$kk".html
                 
                 done

> 9、打印等腰三角形

read -p "请输入一个数字:" Line                                   

for ((i=1; i<=Line; i++))

do
    
    for ((j=$Line-$i; j>0; j--));
       
        do
                
                echo -n ' '
                   
                    done
                       
                        for ((h=1; h<=$(2*$i-1)); h++))
                            
                            do
                                   
                                    echo -n '*'
                                        
                                        done
                                            echo
                               
                                done


> 10.猴子第一天摘下若干个桃子，当即吃了一半，不不过瘾，又多吃了一个，第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天剩下的一半加一个。到第十天早上再吃时，只剩下一个桃子了。求第一天一共摘了多少？

read -p "请输入天数：" day

read -p "请输入最后一天剩余个数：" sum

        let day=day-1

for i in `seq 1 $day`
        
        do
        
        let sum=(sum+1)*2
        
done

        echo "桃子的个数是$sum："

unset

## while循环

while CONDITION; do

        循环体

done

CONDITION：循环控制条件；进入循环之前，先做一次判断；每一次循环之后会再次做判断；条件为“true” ，则执行一次循环；直到条件测试状态为“false”终止循环

因此： CONDTION一般应该有循环控制变量；而此变量的值会在循环体不断地被修正

进入条件： CONDITION为true

退出条件： CONDITION为false

## 练习：用while实现

> 1、 编写脚本，求100以内所有正奇数之和

i=1

sum=0

while [ $i -le 100 ];do

    if [ $[$i%2] -ne 0 ];then

           let sum+=i

               fi

                      let i++

                      done

                      echo "sum is $sum"

> 2、 编写脚本， 提示请输入网络地址，如192.168.0.0，判断输入的网段中主机在线状态，并统计在线和离线主机各多少

net=192.168.213

     let num=1

        count1=0

        count2=0

while [ ${num} -le 254 ];do

        ping -c1 -w1 "${net}.${num} is up"

if [ $? -eq 0 ];then

        echo "${net}.${num} is up"

        count1=$(($count1+1))
else

        echo "${net}.${num} is down"

        count2=$(($count2+1))
fi

        num=$(($num+1))
done

        echo "total ${count1} hosts are up"

        echo "total ${count2} hosts are down"
unset net

unset num

unset count1

unset count2

> 3、 编写脚本，打印九九乘法表

for i in  `seq 9`

do

         for j in `seq 9`

         do 

       [ $j -le $i ] &&  echo  -n  "$i*$j= `echo $(($i*$j))` "

         done 

        echo "  "

done

> 4、 编写脚本，利用变量RANDOM生成10个随机数字，输出这个10数字，并显示其中的最大值和最小值

i=1                                                              
 a=$RANDOM

    max=$a

      min=$a

        while [ $i -le 10 ];do

                [ $max -lt $a ] && max=$a

                  [ $min -gt $a ] && min=$a

               a=$RANDOM

                   echo "$a"

                     let i++

                 done

                   echo "max is $max"

                    echo "min is $min"

> 5、编写脚本，实现打印国际象棋棋盘

red="\e[41m  \e[0m"

white="\e[47m  \e[0m"

  for i in `seq 1 8`;do    

             if [ $[i%2] -eq 0 ];then   

                   for j in {1..4};do    

                        echo -en "$red$white"
                       
                       done
                      else

                           
                           for j in {1..4};do         
                           echo -en "$white$red"
                           
        done
                           
                fi
                          
                          echo
                 
                  done

6、后续六个字符串： efbaf275cd、 4be9c40b8b、 44b2395c46、f8c8873ce0、 b902c16c8b、 ad865d2f63是通过对随机数变量RANDOM随机执行命令： echo $RANDOM|md5sum|cut –c1-10 后的结果，请破解这些字符串对应的RANDOM值

key_num_array=(efbaf275cd 4be9c40b8b 44b2395c46 f8c8873ce0 b902c16c8b ad865d2f63) 

for true_num in `seq 0 65535`;do   #RANDOM值为0~65535                     
       
       key_num=`echo $true_num | md5sum | cut -c 1-10`
          
          for num in  ${key_num_array[*]}; do
              
                if [ "$key_num" == "$num" ];
                
                then
                    
                      echo "$num --> $true_num" 
                     fi

                   done
                done

## until循环

until CONDITION; do

        循环体

done

* 进入条件： CONDITION 为false

* 退出条件： CONDITION 为true

## 循环控制语句continue

用于循环体中

continue [N]：提前结束第N层的本轮循环，而直接进入下一轮判断；最内层为第1层

while CONDTIITON1; do

        CMD1

        ...

        if CONDITION2; then

                continue

        fi

        CMDn

        ...

done

用于循环体中

break [N]：提前结束第N层循环， 最内层为第1层

while CONDTIITON1; do

        CMD1

        ...

        if CONDITION2; then

                break

        fi

        CMDn

        ...
done

## 循环控制shift命令

shift [n]

用于将参量列表 list 左移指定次数，缺省为左移一次。

参量列表 list 一旦被移动，最左端的那个参数就从列表中删除。 while 循环遍历位置参量列表时，常用到 shift

./doit.sh a b c d e f g h

./shfit.sh a b c d e f g h

## 创建无限循环

while true; do

        循环体

done

until false; do

        循环体

Done

> 练习

> 1、每隔3秒钟到系统上获取已经登录的用户的信息；如果发现用户hacker登录，则将登录时间和主机记录于日志/var/log/login.log中,并退出脚本

until false;do

    if   w -h|grep hacker &>/dev/null

        then

        w -h|grep hacker|tr -s ' ' |cut -d ' ' -f3,4 >> /var/log/login.log
                        
                echo "You are forbidden to login,please logout at once!"|write hacker &> /dev/null
                                
                        echo "Message to hacker is sent"
                                    
                fi

                echo "hacker doesnot login"
                                
                                sleep 3
                                        
                                         done

> 2、随机生成10以内的数字，实现猜字游戏，提示比较大或小，相等则退出

until false;do

    rannum=$[$RANDOM%11]

        read -p "please enter your num[0-10]: " num
            
            if [ $rannum -eq $num ];then
                    
                    echo "You are very luchy!"
                            
                            break
        
        else

                echo "you are wrong,guess again!"
                
                fi
                  
                  done 
