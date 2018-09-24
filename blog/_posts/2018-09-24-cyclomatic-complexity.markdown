---
layout: post
title:  "Python 圈复杂度分析"
date:   2018-09-24 12:03:40 +0800
categories: [blog, python] 
tags: [python, tools]
---
# 圈复杂度分析

![images]({{ site.url }}/assets/codercat.jpg){:.small_image}

Cyclomatic Complexity 是用于衡量代码结构复杂度的标准（不是数据结构）。适用于描述控制流路径的数量、同时也可做为测试程序分支的最小测试集数量的参考。一般情况下程序的圈复杂度越高，程序的控制流越复杂，也越难理解。圈复杂度尽管被广泛使用，但也因不能分析暗含的嵌套的控制结构而被诟病。

其计算过程是将程序控制流看做是有向图，并利用如下公式计算：

M = E - N + 2P

其中，E是边数，N是节点数，P是连通分支（也可从出口节点数来考虑：最后一条指令、return、exit等）

对于单一的程序（子程序、方法），P总是等于1，因此公式化简为：

M = E - N + 2

如果连通分支中的每一个出口，都和自身的入口相连，这样的图称为强连通图，此处的每一个循环中都不包括其他循环，则其公式为：

M = E - N + P

*举例*：

![images]({{ site.url }}/assets/250px-Control_flow_graph_of_function_with_loop_and_an_if_statement_without_loop_back.svg.png)

此图有9条边、8个节点、一个连通分支：

M =  9 - 8 + 2*1 = 3

![images]({{ site.url }}/assets/250px-Control_flow_graph_of_function_with_loop_and_an_if_statement.svg.png)

同样的函数、此图有10条边、8个节点、一个连通分支（每一个出口都指回了入口）：

M = 10 - 8 + 1 = 3


*程序举例*：

~~~python
x = 3
if x > 0:
    print('Positive')
else:
    print('Negative')
~~~

以上代码的对应的有向图如下：

![images]({{ site.url }}/assets/cc_demo_1.png)

故而：对应的圈复杂度为 5-5+2(1)=2

# Python 进行分析

## McCabe complexity checker

[Ned's script to check McCabe complexity.](https://github.com/pycqa/mccabe)

### installation

~~~bash
pip install mccabe
~~~

~~~python
# cc.py
def judge(x):
    if x > 5:
        print('Positive')
    else:
        if x == 0:
            print('Zero')
        else:
            print('Negative')


def main():
    judge(3)


if __name__ == "__main__":
    main()
~~~

使用（找出圈复杂度大等于2的部分）:

~~~bash
python -m mccabe --min 2 cc.py
~~~

得到结果：
~~~bash
1:0: 'judge' 3
If 15 2
~~~

<style type="text/css">
    .small_image{
        width: 60px;
    }
</style>


