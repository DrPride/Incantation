# 布局

不妨查看materialize-css的[grid](http://materializecss.com/grid.html).

## Container

首先有一个对象叫做container, 它的作用是这样的。

- `No Container`
[![No Container](./noContainer.PNG)](./noContainer.PNG)

- `Has Container`
[![Has Container](./hasContainer.PNG)](./hasContainer.PNG)

代码如下:

```python
from incantation.template import Page 
from incantation.Module.CSS.Grid import container, row
from incantation.Module.CSS.Color import color
from flask import Flask
app = Flask(__name__)
app.debug = True
def my_page(hasContainer:bool):
    a_row = row("This is a row.")
    a_row.cons_class(color('red').gen())

    if hasContainer:
        main = container()
        main.contains(a_row)
        res = main
    else:
        res = a_row
    return Page(res).gen()
@app.route('/<hasContainer>', methods=['GET'])
def index(hasContainer):
    return my_page(hasContainer == 'hasContainer')
app.run('localhost')
```

## 一行12列

- `4-8 Split`

[![4-8 Split](./Grid48.PNG)](./Grid48.PNG)


- `6-6 Split`

[![6-6 Split](./Grid66.PNG)](./Grid66.PNG)

代码如下

```python
from incantation.template import Page 
from incantation.Module.abst import Seq
from incantation.Module.CSS.Grid import  row, grid, col
from incantation.Module.CSS.Color import color
from incantation.Module.CSS.Table import table
from flask import Flask
app = Flask(__name__)
app.debug = True

def my_page(gridNum):
    gridNum = int(gridNum)
    table_example = table(["A","B","C"],
                          [[1,2,3],
                           [2,3,4],
                           [5,6,7]
                          ]).cons_class('striped')
    a_row = row(
                Seq(
                    col(table_example, grid(l = gridNum)),
                    col(table_example, grid(l = 12-gridNum)),
                    )
                )
    return Page(a_row).gen()
@app.route('/<gridNum>', methods=['GET'])
def index(gridNum):
    return my_page(gridNum)
app.run('localhost')
```

**P.S** `grid`的初始化可以选用两种方法

- 声明大中小三种屏幕下的占位。
    ```
    grid(s=1, l=2, m=3)
    ```
- 声明大中小三种屏幕之一的占位, 这会按照`s:m:l=12:4:3`的比例换算。
    ```
    grid(s=10)
    grid(l=5)
    grid(m=12)
    ```



## Divider和Section

divider能够提供分割线。
要使用divider对象，直接使用

```python
split = divider()
```
即可初始化一个分割线对象。

关于section对象，具体请查看materialize-css。

以下是一个section和divider使用的例子(divider颜色较浅，请仔细查看...

[![divsec](./divsec.PNG)](./divsec.PNG)

```python
from incantation.template import Page 
from incantation.Module.abst import Seq
from incantation.Module.CSS.Grid import divider, section, container
from flask import Flask
app = Flask(__name__)
app.debug = True

@app.route('/', methods=['GET'])
def index():
    main = container()
    main.contains(
                Seq(section('<h5>A</h5><p>a</p>'),
                    divider(),
                    section('<h>B</h><p>b</p>')
                    )
                )
    return Page(main).gen()

app.run('localhost')
```

 





