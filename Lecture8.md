# Metaprogramming

## build systems

### Make

makefile

```makefile
paper.pdf: paper.tex plot-data.png
		pdflatex paper.tex
		
plot-%.png: %.dat plot.py
		./plot.py -i $* -o $@
```

`%`是任意字符的意思，`-i`会帮助在python文件中标记输入，`$*`是一个特殊的变量，不管%是什么，makefile都能成功匹配，比如查找foo.dat，`$*`将会拓展为foo

```shell
make
```

