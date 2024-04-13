#  Lecture 4 Data Wrangling

```bash
journalctl | grep -i intel
```

这是一个简单的数据整理

```bash
ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less
#过滤非必要信息
#''的作用：大文件流直接传输非常浪费，先在远端机器上进行过滤再传输更好
#less可以通过翻页的方式浏览较长的文本
```

## Sed

```bash
ssh myserver journaltcl 
| grep sshd 
| grep "Disconnected from" 
| sed 's/.*Disconnected from //'
#sed的s用于替换 语法为：s/REGEX/SUBSTITUTION
#REGEX是正则表达式，SUBSTITUTION是替换文本
```

```bash
echo 'aba' | sed 's/[ab]//'
ba #每行只会匹配一次替换一次
echo 'abac' | sed 's/[ab]//g'
c  #修饰符g可以让sed匹配尽可能多次
echo 'abcaba' | sed -E 's/(ab)*//g'
ca #sed无法识别较新的正则表达式 需要使用-E 或者使用转义字符“\”
```

```bash
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/'
#^代表句子的开始，$代表句子的结束
#在正则表达式中，任何带圆括号的表达式都是捕获组
#\2代表第二个捕获组 再替换中放入这一组的值
```

## Sort & Unique

```bash
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | wc -l
#sort用于排序，unique可以去除重复行，修饰符-c将计算重复行的重复次数并消除它们
#实例： 9 zz
#      2 zzz
#wc -l 用于统计行数
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | sort -nk1,1 | tail -n10
#修饰符-n用于数字排序，修饰符-k可以让你选择输出中的以空格为间隔符的列来执行排序，“1,1”从第一行开始并在第一行停止排序，选取最后十行（最大十行）
```

```bash
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | sort -nk1,1 | tail -n20 | awk '{print $2}' | paste -sd,
#awk是一个基于列的流处理器，将输入解析为以空格为分隔符的列，再单独操作这些列
#paste是一个命令，可以将多行合并在一起，使其成为一行，修饰符-s可以分割输入，使用-sd,获得一个以,分隔的用户排名列表
```

```bash
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | sort '$1 == 1 && $2 ~ /^c.*e$/ {print $0}'
#显示以c为开头，e为结尾且在日志中只出现一次的用户名
```

## bc(Berkeley Calculator)

```bash
echo "1 + 2" | bc -l
#-l防止报错
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | awk '$1 != 1 {print $1}' | paste -sd+ | bc -l
#计算总登入次数
```

## Misc

```bash
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | R --slave -e 'x <- scan(file-"stdin", quiet=TRUE); sumary(x)'
#使用R语言来计算
```

```bash
cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | sort | unique -c | sort -nk1,1 | tail -n5 | gnuplot -p -e 'set boxwidth 0.5;plot "-" using 1:xtic(2) with boxes'
#使用gnuplot画图
```

````bash
rsutup toolchain list | grep nightly | grep -v 'nightly-x86' | grep 2019
nightly-2019-03-06-x86_64-unknown-linux-gnu
nightly-2019-04-23-x86_64-unknown-linux-gnu
nightly-2019-05-22-x86_64-unknown-linux-gnu
nightly-2019-06-19-x86_64-unknown-linux-gnu
nightly-2019-08-28-x86_64-unknown-linux-gnu
rsutup toolchain list | grep nightly | grep -v 'nightly-x86' | grep 2019 | sed 's/-x86.*//' | xargs rustup toolchain uninstall
#xargs按列分隔
````

```bash
ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 - | convert - -colorspace gray - | gzip | ssh tsp 'gzip -d | tee copy.png' | feh -
#ffmpeg是视频/图像处理工具，convert是图像处理工具
#-用于告诉程序使用标准输入/输出而不用文件
```

