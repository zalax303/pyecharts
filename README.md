# 目录

* [项目概况](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#项目概况)
* [如何安装](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#如何安装)
* [开始使用](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#开始使用)
* [通用配置项](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#通用配置项)
    * xyAxis：直角坐标系中的 x、y 轴(Line、Bar、Scatter、EffectScatter)
    * legend：图例组件。图例组件展现了不同系列的标记(symbol)，颜色和名字。可以通过点击图例控制哪些系列不显示。
    * label：图形上的文本标签，可用于说明图形的一些数据信息，比如值，名称等。
    * lineStyle：带线图形的线的风格选项(Line、Polar、Radar、Graph、Parallel)
* [图表详细](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#图表详细)
    * Bar（柱状图/条形图）
    * EffectScatter（带有涟漪特效动画的散点图）
    * Funnel（漏斗图）
    * Gauge（仪表盘）
    * Geo（地理坐标系）
    * Graph（关系图）
    * Line（折线/面积图）
    * Liquid（水球图）
    * Map（地图）
    * Parallel（平行坐标系）
    * Pie（饼图）
    * Polar（极坐标系）
    * Radar（雷达图）
    * Scatter（散点图）
    * WordCloud（词云图）
* [用户自定义](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#用户自定义)
* [更多示例](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#更多示例)
* [关于项目](https://github.com/chenjiandongx/pyecharts/blob/master/README.md#关于项目)


# 项目概况  
pyecharts 是一个用于生成 Echarts 图表的类库。 

[Echarts](https://github.com/ecomfe/echarts) 是百度开源的一个数据可视化 JS 库。看了官方的介绍文档，觉得很不错，就想看看有没有人实现了 Python 库可以直接调用的。Github 上找到了一个 [echarts-python](https://github.com/yufeiminds/echarts-python) 不过这个项目已经很久没更新且也没什么介绍文档。借鉴了该项目，就自己动手实现一个，于是就有了 pyecharts。API 接口是从另外一个图表库 [pygal](https://github.com/Kozea/pygal) 中模仿的。


# 如何安装
pyecharts 兼容 Python2 和 Python3。当前版本为 0.1.4

```python
pip install pyecharts
```

# 开始使用
首先开始来绘制你的第一个图表
```python
from pyecharts import Bar

bar = Bar("我的第一个图表", "这里是副标题")
bar.add("服装", ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"], [5, 20, 36, 10, 75, 90])
bar.show_config()
bar.render()
```
![guide-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/guide-0.png)

**Tip：** 可以按右边的下载按钮将图片下载到本地  

* ```add()```  
    主要方法，用于添加图表的数据和设置各种配置项  
* ```show_config()```  
    打印输出图表的所有配置项
* ```render()```  
    默认将会在根目录下生成一个 render.html 的文件，支持 path 参数，设置文件保存位置，如 render(r"e:\my_first_chart.html")，文件用浏览器打开。  
    默认的编码类型为 UTF-8，在 Python3 中是没什么问题的，Python3 对中文的支持好很多。但是在 Python2 中，编码的处理是个很头疼的问题，暂时没能找到完美的解决方法，目前只能通过文本编辑器自己进行二次编码，我用的是 Visual Studio Code，先通过 Gbk 编码重新打开，然后再用 UTF-8 重新保存，这样用浏览器打开的话就不会出现中文乱码问题了。  

基本上所有的图表类型都是这样绘制的：
1. ```chart_name = Type()``` 初始化具体类型图表。
2. ```add()``` 添加数据及配置项。
3. ```render()``` 生成 .html 文件。  

```add()``` 数据一般为两个列表（长度一致），如果你的数据是字典或者是带元组的字典。可利用 ```cast()``` 方法转换。

```python
@staticmethod
cast(seq)
``` 转换数据序列，将带字典和元组类型的序列转换为 k_lst,v_lst 两个列表 ``` 
```
1. 元组列表  
    [(A1, B1), (A2, B2), (A3, B3), (A4, B4)] --> k_lst[ A[i1, i2...] ], v_lst[ B[i1, i2...] ]
2. 字典列表  
    [{A1: B1}, {A2: B2}, {A3: B3}, {A4: B4}] --> k_lst[ A[i1, i2...] ], v_lst[ B[i1, i2...] ]
3. 字典  
    {A1: B1, A2: B2, A3: B3, A4: B4} -- > k_lst[ A[i1, i2...] ], v_lst[ B[i1, i2...] ]


图表类初始化所接受的参数（所有类型的图表都一样）。

* title -> str   
    主标题文本，支持 \n 换行，默认为 ""
* subtitle -> str  
    副标题文本，支持 \n 换行，默认为 ""
* width -> int  
    画布宽度，默认为 800（px）
* height -> int  
    画布高度，默认为 400（px）
* title_pos -> str   
    标题位置，默认为 auto，有'auto', 'left', 'right', 'center'可选
* title_color -> str  
    主标题文本颜色，默认为 '#000'
* subtitle_color -> str  
    副标题文本颜色，默认为 '#aaa'
* title_text_size -> int  
    主标题文本字体大小，默认为 18
* subtitle_text_size -> int  
    副标题文本字体大小，默认为 12
* background_color -> str  
    画布背景颜色，默认为 '#fff'
    
# 通用配置项
**通用配置项均在 ```add()``` 中设置**

xyAxis：直角坐标系中的 x、y 轴(Line、Bar、Scatter、EffectScatter)

* is_convert -> bool  
    是否交换 x 轴与 y 轴
* xy_text_size -> int  
    x 轴和 y 轴字体大小
* namegap -> int  
    坐标轴名称与轴线之间的距离
* x_axis -> list  
    x 轴数据项
* xaxis_name -> str  
    x 轴名称
* xaxis_name_pos -> str  
    x 轴名称位置，有'start'，'middle'，'end'可选
* y_axis -> list  
    y 坐标轴数据
* yaxis_formatter -> str  
    y 轴标签格式器，如 '天'，则 y 轴的标签为数据加'天'(3 天，4 天),默认为 ""
* yaxis_name -> str  
    y 轴名称
* yaxis_name_pos -> str  
    y 轴名称位置，有'start', 'middle'，'end'可选
* interval -> int  
    坐标轴刻度标签的显示间隔，在类目轴中有效。默认会采用标签不重叠的策略间隔显示标签  
    设置成 0 强制显示所有标签  
    设置为 1，表示『隔一个标签显示一个标签』，如果值为 2，表示隔两个标签显示一个标签，以此推

legend：图例组件。图例组件展现了不同系列的标记(symbol)，颜色和名字。可以通过点击图例控制哪些系列不显示。

* is_legend_show -> bool  
    是否显示顶端图例，默认为 True
* legend_orient -> str  
    图例列表的布局朝向，默认为'horizontal'，有'horizontal', 'vertical'可选
* legend_pos -> str  
    图例组件离容器左侧的距离，默认为'center'，有'left', 'center', 'right'可选
* legend_top -> str  
    图例组件离容器上侧的距离，默认为'top'，有'top', 'center', 'bottom'可选
    

label：图形上的文本标签，可用于说明图形的一些数据信息，比如值，名称等。

* is_label_show -> bool  
    是否正常显示标签，默认不显示。标签即各点的数据项信息  
* is_emphasis -> bool  
    是否高亮显示标签，默认显示。高亮标签即选中数据时显示的信息项。
* label_pos -> str  
    标签的位置，Bar 图默认为'top'。有'top', 'left', 'right', 'bottom', 'inside','outside'可选
* label_text_color -> str  
    标签字体颜色，默认为 "#000"
* label_text_size -> int  
    标签字体大小，默认为 12
* is_random -> bool  
    是否随机排列颜色列表，默认为 False
* label_color -> list  
    自定义标签颜色。
* formatter -> list  
    标签内容格式器，有'series', 'name', 'value', 'percent'可选。如 ["name", "value"]
    * series：图例名称
    * name：数据项名称
    * value：数据项值
    * percent：数据的百分比（主要用于饼图）

lineStyle：带线图形的线的风格选项(Line、Polar、Radar、Graph、Parallel)
* line_width -> int    
    线的宽度，默认为 1
* line_opacity -> float    
    线的透明度，0 为完全透明，1 为完全不透明。默认为 1
* line_curve -> float    
    线的弯曲程度，0 为完全不弯曲，1 为最弯曲。默认为 0
* line_type -> str  
    线的类型，有'solid', 'dashed', 'dotted'可选。默认为'solid'


# 图表详细  

## Bar（柱状图/条形图）
Bar.add() 方法签名
```python
add(name, x_axis, y_axis, is_stack=False, **kwargs)
```
* name -> str  
    图例名称
* x_axis -> list  
    x 坐标轴数据
* y_axis -> list  
    y 坐标轴数据  
* is_stack -> bool  
    数据堆叠，同个类目轴上系列配置相同的 stack 值可以堆叠放置  

```python
from pyecharts import Bar

attr = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
v1 = [5, 20, 36, 10, 75, 90]
v2 = [10, 25, 8, 60, 20, 80]
bar = Bar("柱状图数据堆叠示例")
bar.add("商家A", attr, v1, is_stack=True)
bar.add("商家B", attr, v2, is_stack=True)
bar.render()
```
![bar-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/bar-0.gif)  

```python
from pyecharts import Bar

bar = Bar("标记线和标记点示例")
bar.add("商家A", attr, v1, mark_point=["average"])
bar.add("商家B", attr, v2, mark_line=["min", "max"])
bar.render()
```
![bar-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/bar-1.gif)

* mark_point  -> list  
    标记点，有'min', 'max', 'average'可选
* mark_line  -> list  
    标记线，有'min', 'max', 'average'可选

```python
from pyecharts import Bar

bar = Bar("x 轴和 y 轴交换")
bar.add("商家A", attr, v1)
bar.add("商家B", attr, v2, is_convert=True)
bar.render()
```
![bar-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/bar-2.png)


## EffectScatter（带有涟漪特效动画的散点图）
EffectScatter.add() 方法签名
```python
add(name, x_value, y_value, symbol_size=10, **kwargs)
```
* name -> str  
    图例名称
* x_axis -> list  
    x 坐标轴数据
* y_axis -> list  
    y 坐标轴数据
* symbol_size -> int  
    标记图形大小，默认为 10
    
```python
from pyecharts import EffectScatter

v1 = [10, 20, 30, 40, 50, 60]
v2 = [25, 20, 15, 10, 60, 33]
es = EffectScatter("动态散点图示例")
es.add("effectScatter", v1, v2)
es.render()
```
![effectscatter-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/effectscatter-0.gif)

```python
es = EffectScatter("动态散点图各种图形示例")
es.add("", [10], [10], symbol_size=20, effect_scale=3.5, effect_period=3, symbol="pin")
es.add("", [20], [20], symbol_size=12, effect_scale=4.5, effect_period=4,symbol="rect")
es.add("", [30], [30], symbol_size=30, effect_scale=5.5, effect_period=5,symbol="roundRect")
es.add("", [40], [40], symbol_size=10, effect_scale=6.5, effect_brushtype='fill',symbol="diamond")
es.add("", [50], [50], symbol_size=16, effect_scale=5.5, effect_period=3,symbol="arrow")
es.add("", [60], [60], symbol_size=6, effect_scale=2.5, effect_period=3,symbol="triangle")
es.render()
```
![effectscatter-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/effectscatter-1.gif)

* symbol -> str  
    标记图形，有'rect', 'roundRect', 'triangle', 'diamond', 'pin', 'arrow'可选
* effect_brushtype -> str  
    波纹绘制方式，有'stroke', 'fill'可选。默认为'stroke'
* effect_scale -> float  
    动画中波纹的最大缩放比例。默认为 2.5
* effect_period -> float  
    动画持续的时间。默认为 4（s）


## Funnel（漏斗图）
Funnel.add() 方法签名
```python
add(self, name, attr, value, **kwargs)
```
* name -> str  
    图例名称
* attr -> list  
    属性名称
* value -> list  
    属性所对应的值

```python
from pyecharts import Funnel

attr = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
value = [20, 40, 60, 80, 100, 120]
funnel = Funnel("漏斗图示例")
funnel.add("商品", attr, value, is_label_show=True, label_pos="inside", label_text_color="#fff")
funnel.render()
```
![funnel-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/funnel-0.gif)

```python
funnel = Funnel("漏斗图示例", width=600, height=400, title_pos='center')
funnel.add("商品", attr, value, is_label_show=True, label_pos="outside", legend_orient='vertical',
           legend_pos='left')
funnel.show_config()
funnel.render()
```
![funnel-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/funnel-1.png)


## Gauge（仪表盘）
Gauge.add() 方法签名
```python
add(name, attr, value, scale_range=None, angle_range=None, **kwargs)
```
* name -> str  
    图例名称
* attr -> list  
    属性名称
* value -> list
    属性所对应的值
* scale_range -> list  
    仪表盘数据范围。默认为 [0, 100]
* angle_range -> list  
    仪表盘角度范围。默认为 [225, -45]
    
```python
from pyecharts import Gauge

gauge = Gauge("仪表盘示例")
gauge.add("业务指标", "完成率", 66.66)
gauge.show_config()
gauge.render()
```
![gauge-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/gauge-0.png)

```python
gauge = Gauge("仪表盘示例")
gauge.add("业务指标", "完成率", 166.66, angle_range=[180, 0], scale_range=[0, 200], is_legend_show=False)
gauge.show_config()
gauge.render()
```
![gauge-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/gauge-1.png)


## Geo（地理坐标系）
Geo.add() 方法签名
```python
add(name, attr, value, type="scatter", maptype='china', symbol_size=12, border_color="#111",
    geo_normal_color="#323c48", geo_emphasis_color="#2a333d", **kwargs)
```
* name -> str  
    图例名称
* attr -> list  
    属性名称
* value -> list   
    属性所对应的值
* type -> str  
    图例类型，有'scatter', 'effectscatter'可选。默认为 'scatter'
* maptype -> str  
    地图类型，目前只有 'china' 可选
* symbol_size -> int  
    标记图形大小。默认为 12
* border_color -> str  
    地图边界颜色。默认为 '#111'
* geo_normal_color -> str  
    正常状态下地图区域的颜色。默认为 '#323c48'
* geo_emphasis_color -> str  
    高亮状态下地图区域的颜色。默认为 '#2a333d'

```python
from pyecharts import Geo

data = [
    ("海门", 9),("鄂尔多斯", 12),("招远", 12),("舟山", 12),("齐齐哈尔", 14),("盐城", 15),
    ("赤峰", 16),("青岛", 18),("乳山", 18),("金昌", 19),("泉州", 21),("莱西", 21),
    ("日照", 21),("胶南", 22),("南通", 23),("拉萨", 24),("云浮", 24),("梅州", 25),
    ("文登", 25),("上海", 25),("攀枝花", 25),("威海", 25),("承德", 25),("厦门", 26),
    ("汕尾", 26),("潮州", 26),("丹东", 27),("太仓", 27),("曲靖", 27),("烟台", 28),
    ("福州", 29),("瓦房店", 30),("即墨", 30),("抚顺", 31),("玉溪", 31),("张家口", 31),
    ("阳泉", 31),("莱州", 32),("湖州", 32),("汕头", 32),("昆山", 33),("宁波", 33),
    ("湛江", 33),("揭阳", 34),("荣成", 34),("连云港", 35),("葫芦岛", 35),("常熟", 36),
    ("东莞", 36),("河源", 36),("淮安", 36),("泰州", 36),("南宁", 37),("营口", 37),
    ("惠州", 37),("江阴", 37),("蓬莱", 37),("韶关", 38),("嘉峪关", 38),("广州", 38),
    ("延安", 38),("太原", 39),("清远", 39),("中山", 39),("昆明", 39),("寿光", 40),
    ("盘锦", 40),("长治", 41),("深圳", 41),("珠海", 42),("宿迁", 43),("咸阳", 43),
    ("铜川", 44),("平度", 44),("佛山", 44),("海口", 44),("江门", 45),("章丘", 45),
    ("肇庆", 46),("大连", 47),("临汾", 47),("吴江", 47),("石嘴山", 49),("沈阳", 50),
    ("苏州", 50),("茂名", 50),("嘉兴", 51),("长春", 51),("胶州", 52),("银川", 52),
    ("张家港", 52),("三门峡", 53),("锦州", 54),("南昌", 54),("柳州", 54),("三亚", 54),
    ("自贡", 56),("吉林", 56),("阳江", 57),("泸州", 57),("西宁", 57),("宜宾", 58),
    ("呼和浩特", 58),("成都", 58),("大同", 58),("镇江", 59),("桂林", 59),("张家界", 59),
    ("宜兴", 59),("北海", 60),("西安", 61),("金坛", 62),("东营", 62),("牡丹江", 63),
    ("遵义", 63),("绍兴", 63),("扬州", 64),("常州", 64),("潍坊", 65),("重庆", 66),
    ("台州", 67),("南京", 67),("滨州", 70),("贵阳", 71),("无锡", 71),("本溪", 71),
    ("克拉玛依", 72),("渭南", 72),("马鞍山", 72),("宝鸡", 72),("焦作", 75),("句容", 75),
    ("北京", 79),("徐州", 79),("衡水", 80),("包头", 80),("绵阳", 80),("乌鲁木齐", 84),
    ("枣庄", 84),("杭州", 84),("淄博", 85),("鞍山", 86),("溧阳", 86),("库尔勒", 86),
    ("安阳", 90),("开封", 90),("济南", 92),("德阳", 93),("温州", 95),("九江", 96),
    ("邯郸", 98),("临安", 99),("兰州", 99),("沧州", 100),("临沂", 103),("南充", 104),
    ("天津", 105),("富阳", 106),("泰安", 112),("诸暨", 112),("郑州", 113),("哈尔滨", 114),
    ("聊城", 116),("芜湖", 117),("唐山", 119),("平顶山", 119),("邢台", 119),("德州", 120),
    ("济宁", 120),("荆州", 127),("宜昌", 130),("义乌", 132),("丽水", 133),("洛阳", 134),
    ("秦皇岛", 136),("株洲", 143),("石家庄", 147),("莱芜", 148),("常德", 152),("保定", 153),
    ("湘潭", 154),("金华", 157),("岳阳", 169),("长沙", 175),("衢州", 177),("廊坊", 193),
    ("菏泽", 194),("合肥", 229),("武汉", 273),("大庆", 279)]

geo = Geo("全国主要城市空气质量", "data from pm2.5", title_color="#fff", title_pos="center",
width=1200, height=600, background_color='#404a59')
attr, value = geo.cast(data)
geo.add("", attr, value, visual_range=[0, 200], visual_text_color="#fff", symbol_size=15, is_visualmap=True)
geo.show_config()
geo.render()
```
![geo-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/geo-0.gif)

visualmap：是视觉映射组件，用于进行『视觉编码』，也就是将数据映射到视觉元素（视觉通道）
* is_visualmap -> bool  
    是否使用视觉映射组件
* visual_range -> list  
    指定组件的允许的最小值与最大值。默认为 [0, 100]
* visual_text_color -> list  
    两端文本颜色。
* visual_range_text -> list  
    两端文本。默认为 ['low', 'hight']
* visual_range_color -> list  
    过渡颜色。默认为 ['#50a3ba', '#eac763', '#d94e5d']
* is_calculable -> bool  
    是否显示拖拽用的手柄（手柄能拖拽调整选中范围）。默认为 True

```python
from pyecharts import Geo

data = [("海门", 9), ("鄂尔多斯", 12), ("招远", 12), ("舟山", 12), ("齐齐哈尔", 14), ("盐城", 15)]
geo = Geo("全国主要城市空气质量", "data from pm2.5", title_color="#fff", title_pos="center",
          width=1200, height=600, background_color='#404a59')
attr, value = geo.cast(data)
geo.add("", attr, value, type="effectScatter", is_random=True, effect_scale=5)
geo.show_config()
geo.render()
```
![geo-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/geo-1.gif)


## Graph（关系图）
Graph.add() 方法签名
```python
add(name, nodes, links, categories=None, is_focusnode=True, is_roam=True, is_rotatelabel=False,
    layout="force", edge_length=50, gravity=0.2, repulsion=50, **kwargs)
```
* name -> str  
    图例名称
* nodes -> dict  
    关系图结点，包含的数据项有  
    * name：结点名称（必须有！）
    * x：节点的初始 x 值
    * y：节点的初始 y 值
    * value：结点数值 
    * category：结点类目
    * symbol：标记图形
    * symbolSize：标记图形大小
* links -> dict  
    结点间的关系数据，包含的数据项有  
    * source：边的源节点名称的字符串，也支持使用数字表示源节点的索引（必须有！）
    * target：边的目标节点名称的字符串，也支持使用数字表示源节点的索引（必须有！）
    * vaule：边的数值，可以在力引导布局中用于映射到边的长度
* categories -> list  
    结点分类的类目，结点可以指定分类，也可以不指定。  
    如果节点有分类的话可以通过 nodes[i].category 指定每个节点的类目，类目的样式会被应用到节点样式上
* is_focusnode -> bool  
    是否在鼠标移到节点上的时候突出显示节点以及节点的边和邻接节点。默认为 True
* is_roam -> bool/str  
    是否开启鼠标缩放和平移漫游。默认为 True  
    如果只想要开启缩放或者平移，可以设置成'scale'或者'move'。设置成 True 为都开启
* is_rotatelabel -> bool  
    是否旋转标签，默认为 False
* layout -> str  
    关系图布局，默认为 'force'
    * none：不采用任何布局，使用节点中必须提供的 x， y 作为节点的位置。
    * circular：采用环形布局
    * force：采用力引导布局
* edge_length -> int  
    力布局下边的两个节点之间的距离，这个距离也会受 repulsion 影响。默认为 50  
    支持设置成数组表达边长的范围，此时不同大小的值会线性映射到不同的长度。值越小则长度越长
* gravity -> int  
    节点受到的向中心的引力因子。该值越大节点越往中心点靠拢。默认为 0.2
* repulsion -> int  
    节点之间的斥力因子。默认为 50  
    支持设置成数组表达斥力的范围，此时不同大小的值会线性映射到不同的斥力。值越大则斥力越大

```python
from pyecharts import Graph

nodes = [{"name": "结点1", "symbolSize": 10},
         {"name": "结点2", "symbolSize": 20},
         {"name": "结点3", "symbolSize": 30},
         {"name": "结点4", "symbolSize": 40},
         {"name": "结点5", "symbolSize": 50},
         {"name": "结点6", "symbolSize": 40},
         {"name": "结点7", "symbolSize": 30},
         {"name": "结点8", "symbolSize": 20}]
links = []
for i in nodes:
    for j in nodes:
        links.append({"source": i.get('name'), "target": j.get('name')})
graph = Graph("关系图-力引导布局示例")
graph.add("", nodes, links, repulsion=8000)
graph.show_config()
graph.render()

```
![graph-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/graph-0.png)

```python
graph = Graph("关系图-环形布局示例")
graph.add("", nodes, links, is_label_show=True, repulsion=8000, layout='circular', label_text_color=None)
graph.show_config()
graph.render()
```
![graph-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/graph-1.png)

```python
from pyecharts import Graph

import json
with open("..\json\weibo.json", "r", encoding="utf-8") as f:
    j = json.load(f)
    nodes, links, categories, cont, mid, userl = j
graph = Graph("微博转发关系图", width=1200, height=600)
graph.add("", nodes, links, categories, label_pos="right", repulsion=50, is_legend_show=False,
          line_curve=0.2, label_text_color=None)
graph.show_config()
graph.render()
```
![graph-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/graph-2.gif)

**Tip：** 可配置 **lineStyle** 参数


## Line（折线/面积图）
Line.add() 方法签名
```python
add(name, x_axis, y_axis, is_symbol_show=True, is_smooth=False, is_stack=False,
    is_step=False, is_fill=False, **kwargs)
```
* name -> str  
    图例名称
* x_axis -> list  
    x 坐标轴数据
* y_axis -> list  
    y 坐标轴数据
* is_symbol_show -> bool  
    是否显示标记图形，默认为 True
* is_smooth -> bool  
    是否平滑曲线显示，默认为 False
* is_stack -> bool  
    数据堆叠，同个类目轴上系列配置相同的 stack 值可以堆叠放置。默认为 False
* is_step -> bool/str  
    是否是阶梯线图。可以设置为 True 显示成阶梯线图。默认为 False  
    也支持设置成'start', 'middle', 'end'分别配置在当前点，当前点与下个点的中间下个点拐弯。
* is_fill -> bool  
    是否填充曲线所绘制面积，默认为 False

```python
from pyecharts import Line

attr = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
v1 = [5, 20, 36, 10, 10, 100]
v2 = [55, 60, 16, 20, 15, 80]
line = Line("折线图示例")
line.add("商家A", attr, v1, mark_point=["average"])
line.add("商家B", attr, v2, is_smooth=True, mark_line=["max", "average"])
line.show_config()
line.render()
```
![line-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/line-0.gif)

* mark_point  -> list  
    标记点，有'min', 'max', 'average'可选
* mark_line  -> list  
    标记线，有'min', 'max', 'average'可选

```python
line = Line("折线图-数据堆叠示例")
line.add("商家A", attr, v1, is_stack=True, is_label_show=True)
line.add("商家B", attr, v2, is_stack=True, is_label_show=True)
line.show_config()
line.render()
```
![line-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/line-1.gif)

```python
line = Line("折线图-阶梯图示例")
line.add("商家A", attr, v1, is_step=True, is_label_show=True)
line.show_config()
line.render()
```
![line-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/line-2.png)

```python
line = Line("折线图-面积图示例")
line.add("商家A", attr, v1, is_fill=True, line_opacity=0.2, area_opacity=0.4, symbol=None)
line.add("商家B", attr, v2, is_fill=True, area_color='#000', area_opacity=0.3, is_smooth=True)
line.show_config()
line.render()
```
![line-3](https://github.com/chenjiandongx/pyecharts/blob/master/images/line-3.png)

* area_opacity -> float  
    填充区域透明度
* area_color -> str  
    填充区域颜色

**Tip：** 可配置 **lineStyle** 参数


## Liquid（水球图）
Liquid.add() 方法签名
```python
add(name, data, shape='circle', liquid_color=None, is_liquid_animation=True,
    is_liquid_outline_show=True, **kwargs)
```
* name -> str  
    图例名称
* data -> list  
    数据项
* shape -> str  
    水球外形，有'circle', 'rect', 'roundRect', 'triangle', 'diamond', 'pin', 'arrow'可选。默认'circle'
* liquid_color -> list  
    波浪颜色，默认的颜色列表为['#294D99', '#156ACF', '#1598ED', '#45BDFF']。
* is_liquid_animation -> bool  
    是否显示波浪动画，默认为 True。
* is_liquid_outline_show -> bool  
    是否显示边框，默认为 True。

```python
from pyecharts import Liquid

liquid = Liquid("水球图示例")
liquid.add("Liquid", [0.6])
liquid.show_config()
liquid.render()
```
![liquid-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/liquid-0.gif)

```python
from pyecharts import Liquid

liquid = Liquid("水球图示例")
liquid.add("Liquid", [0.6, 0.5, 0.4, 0.3], is_liquid_outline_show=False)
liquid.show_config()
liquid.render()
```
![liquid-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/liquid-1.gif)

```python
from pyecharts import Liquid

liquid = Liquid("水球图示例")
liquid.add("Liquid", [0.6, 0.5, 0.4, 0.3], is_liquid_animation=False, shape='diamond')
liquid.show_config()
liquid.render()
```
![liquid-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/liquid-2.png)

## Map（地图）
Map.add() 方法签名
```python
add(name, attr, value, is_roam=True, maptype='china', **kwargs)
```
* name -> str  
    图例名称
* attr -> list  
   属性名称
* value -> list  
   属性所对应的值
* is_roam -> bool/str
   是否开启鼠标缩放和平移漫游。默认为 True  
   如果只想要开启缩放或者平移，可以设置成'scale'或者'move'。设置成 True 为都开启
* maptype -> str  
   地图类型。
   支持 china、world、安徽、澳门、北京、重庆、福建、福建、甘肃、广东，广西、广州、海南、河北、黑龙江、河南、湖北、湖南、江苏、江西、吉林、辽宁、内蒙古、宁夏、青海、山东、上海、陕西、四川、台湾、天津、香港、新疆、西藏、云南、浙江

```python
from pyecharts import Map

value = [155, 10, 66, 78]
attr = ["福建", "山东", "北京", "上海"]
map = Map("全国地图示例", width=1200, height=600)
map.add("", attr, value, maptype='china')
map.show_config()
map.render()
```
![map-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/map-0.gif)

```python
from pyecharts import Map

value = [155, 10, 66, 78, 33, 80, 190, 53, 49.6]
attr = ["福建", "山东", "北京", "上海", "甘肃", "新疆", "河南", "广西", "西藏"]
map = Map("Map 结合 VisualMap 示例", width=1200, height=600)
map.add("", attr, value, maptype='china', is_visualmap=True, visual_text_color='#000')
map.show_config()
map.render()
```
![map-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/map-1.gif)

**Tip：** 可结合 visualmap 组件进行设置

```python
from pyecharts import Map

value = [20, 190, 253, 77, 65]
attr = ['汕头市', '汕尾市', '揭阳市', '阳江市', '肇庆市']
map = Map("广东地图示例", width=1200, height=600)
map.add("", attr, value, maptype='广东', is_visualmap=True, visual_text_color='#000')
map.show_config()
map.render()
```
![map-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/map-2.gif)


## Parallel（平行坐标系）
Parallel.add() 方法签名
```python
add(name, data, **kwargs)
```
* name -> str
    图例名称
* data -> [list],包含列表的列表  
    数据项。数据中，每一行是一个『数据项』，每一列属于一个『维度』

Parallel.config() 方法签名
```python
config(schema=None, c_schema=None)
```
* schema  
    默认平行坐标系的坐标轴信息，如 ["dim_name1", "dim_name2", "dim_name3"]。
* c_schema  
    用户自定义平行坐标系的坐标轴信息。
    * dim -> int   
        维度索引
    * name > str  
        维度名称
    * type -> str   
        维度类型，有'value', 'category'可选  
        value：数值轴，适用于连续数据。  
        category： 类目轴，适用于离散的类目数据。
    * min -> int  
        坐标轴刻度最小值。
    * max -> int  
        坐标轴刻度最大值。
    * inverse - bool  
        是否是反向坐标轴。默认为 False
    * nameLocation -> str  
        坐标轴名称显示位置。有'start', 'middle', 'end'可选

```python
from pyecharts import Parallel

schema = ["data", "AQI", "PM2.5", "PM10", "CO", "NO2"]
data = [
        [1, 91, 45, 125, 0.82, 34],
        [2, 65, 27, 78, 0.86, 45,],
        [3, 83, 60, 84, 1.09, 73],
        [4, 109, 81, 121, 1.28, 68],
        [5, 106, 77, 114, 1.07, 55],
        [6, 109, 81, 121, 1.28, 68],
        [7, 106, 77, 114, 1.07, 55],
        [8, 89, 65, 78, 0.86, 51, 26],
        [9, 53, 33, 47, 0.64, 50, 17],
        [10, 80, 55, 80, 1.01, 75, 24],
        [11, 117, 81, 124, 1.03, 45]
]
parallel = Parallel("平行坐标系-默认指示器")
parallel.config(schema) 
parallel.add("parallel", data, is_random=True)
parallel.show_config()
parallel.render()
```
![parallel-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/parallel-0.png)

```python
from pyecharts import Parallel

c_schema = [
    {"dim": 0, "name": "data"},
    {"dim": 1, "name": "AQI"},
    {"dim": 2, "name": "PM2.5"},
    {"dim": 3, "name": "PM10"},
    {"dim": 4, "name": "CO"},
    {"dim": 5, "name": "NO2"},
    {"dim": 6, "name": "CO2"},
    {"dim": 7, "name": "等级",
    "type": "category", "data": ['优', '良', '轻度污染', '中度污染', '重度污染', '严重污染']}
]
data = [
    [1, 91, 45, 125, 0.82, 34, 23, "良"],
    [2, 65, 27, 78, 0.86, 45, 29, "良"],
    [3, 83, 60, 84, 1.09, 73, 27, "良"],
    [4, 109, 81, 121, 1.28, 68, 51, "轻度污染"],
    [5, 106, 77, 114, 1.07, 55, 51, "轻度污染"],
    [6, 109, 81, 121, 1.28, 68, 51, "轻度污染"],
    [7, 106, 77, 114, 1.07, 55, 51, "轻度污染"],
    [8, 89, 65, 78, 0.86, 51, 26, "良"],
    [9, 53, 33, 47, 0.64, 50, 17, "良"],
    [10, 80, 55, 80, 1.01, 75, 24, "良"],
    [11, 117, 81, 124, 1.03, 45, 24, "轻度污染"],
    [12, 99, 71, 142, 1.1, 62, 42, "良"],
    [13, 95, 69, 130, 1.28, 74, 50, "良"],
    [14, 116, 87, 131, 1.47, 84, 40, "轻度污染"]
]
parallel = Parallel("平行坐标系-用户自定义指示器")
parallel.config(c_schema=c_schema)
parallel.add("parallel", data)
parallel.show_config()
parallel.render()
```
![parallel-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/parallel-1.png)

**Tip：** 可配置 **lineStyle** 参数


## Pie（饼图）
Pie.add() 方法签名
```python
add(name, attr, value, radius=None, center=None, rosetype=None, **kwargs)
```
* name -> str   
    图例名称
* attr -> list  
    属性名称
* value -> list  
    属性所对应的值
* radius -> list  
    饼图的半径，数组的第一项是内半径，第二项是外半径，默认为 [0, 75]  
    默认设置成百分比，相对于容器高宽中较小的一项的一半
* center -> list  
    饼图的中心（圆心）坐标，数组的第一项是横坐标，第二项是纵坐标，默认为 [50, 50]  
    默认设置成百分比，设置成百分比时第一项是相对于容器宽度，第二项是相对于容器高度
* rosetype -> str  
    是否展示成南丁格尔图，通过半径区分数据大小，有'radius'和'area'两种模式。默认为'radius'
    * radius：扇区圆心角展现数据的百分比，半径展现数据的大小
    * area：所有扇区圆心角相同，仅通过半径展现数据大小

```python
from pyecharts import Pie

attr = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
v1 = [11, 12, 13, 10, 10, 10]
pie = Pie("饼图示例")
pie.add("", attr, v1, is_label_show=True)
pie.show_config()
pie.render()
```
![pie-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/pie-0.gif)

```python
from pyecharts import Pie

attr = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
v1 = [11, 12, 13, 10, 10, 10]
pie = Pie("饼图-圆环图示例", title_pos='center')
pie.add("", attr, v1, radius=[40, 75], label_text_color=None, is_label_show=True,
        legend_orient='vertical', legend_pos='left')
pie.show_config()
pie.render()
```
![pie-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/pie-1.png)

```python
from pyecharts import Pie

attr = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
v1 = [11, 12, 13, 10, 10, 10]
v2 = [19, 21, 32, 20, 20, 33]
pie = Pie("饼图-玫瑰图示例", title_pos='center', width=900)
pie.add("商品A", attr, v1, center=[25, 50], is_random=True, radius=[30, 75], rosetype='radius')
pie.add("商品B", attr, v2, center=[75, 50], is_random=True, radius=[30, 75], rosetype='area',
        is_legend_show=False, is_label_show=True)
pie.show_config() 
pie.render()
```
![pie-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/pie-2.png)


## Polar（极坐标系）
Polar.add() 方法签名
```python
add(name, data, angle_data=None, radius_data=None, type='line', symbol_size=4, start_angle=90,
    rotate_step=0, boundary_gap=True, clockwise=True, **kwargs)
```
* name -> str  
    图例名称
* data -> [list],包含列表的列表  
    数据项 [极径，极角 [数据值]]
* angle_data -> list  
    角度类目数据
* radius_data -> list  
    半径类目数据
* type -> str  
    图例类型，有'line', 'scatter', 'effectScatter', 'barAngle', 'barRadius'可选。默认为 'line'
* symbol_size -> int  
    标记图形大小，默认为 4。
* start_angle -> int  
    起始刻度的角度，默认为 90 度，即圆心的正上方。0 度为圆心的正右方
* rotate_step -> int  
    刻度标签旋转的角度，在类目轴的类目标签显示不下的时候可以通过旋转防止标签之间重叠  
    旋转的角度从 -90 度到 90 度。默认为 0
* boundary_gap -> bool  
    坐标轴两边留白策略  
    默认为 True，这时候刻度只是作为分隔线，标签和数据点都会在两个刻度之间的带(band)中间
* clockwise -> bool  
    刻度增长是否按顺时针，默认 True
* is_stack -> bool  
    数据堆叠，同个类目轴上系列配置相同的 stack 值可以堆叠放置
* axis_range -> list  
    坐标轴刻度范围。默认值为 [None, None]。
* is_angleaxis_show -> bool  
    是否显示极坐标系的角度轴，默认为 True
* is_radiusaxis_show -> bool  
    是否显示极坐标系的径向轴，默认为 True

```python
from pyecharts import Polar

import random
data = [(i, random.randint(1, 100)) for i in range(101)]
polar = Polar("极坐标系-散点图示例")
polar.add("", data, boundary_gap=False, type='scatter', is_splitline_show=False,
          area_color=None, is_axisline_show=True)
polar.show_config()
polar.render()
```
![polar-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/polar-0.png)

* is_splitline_show  -> bool  
    是否显示分割线，默认为 True
* is_axisline_show -> bool  
    是否显示坐标轴线，默认为 True
* area_opacity -> float  
    填充区域透明度
* area_color -> str  
    填充区域颜色

**Tip：** 可配置 **lineStyle** 参数

```python
from pyecharts import Polar

import random
data_1 = [(10, random.randint(1, 100)) for i in range(300)]
data_2 = [(11, random.randint(1, 100)) for i in range(300)]
polar = Polar("极坐标系-散点图示例", width=1200, height=600)
polar.add("", data_1, type='scatter')
polar.add("", data_2, type='scatter')
polar.show_config()
polar.render()
```
![polar-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/polar-1.png)

```python
from pyecharts import Polar

import random
data = [(i, random.randint(1, 100)) for i in range(10)]
polar = Polar("极坐标系-动态散点图示例", width=1200, height=600)
polar.add("", data, type='effectScatter', effect_scale=10, effect_period=5)
polar.show_config()
polar.render()
```
![polar-2](https://github.com/chenjiandongx/pyecharts/blob/master/images/polar-2.gif)

```python
from pyecharts import Polar

radius = ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
polar = Polar("极坐标系-堆叠柱状图示例", width=1200, height=600)
polar.add("A", [1, 2, 3, 4, 3, 5, 1], radius_data=radius, type='barRadius', is_stack=True)
polar.add("B", [2, 4, 6, 1, 2, 3, 1], radius_data=radius, type='barRadius', is_stack=True)
polar.add("C", [1, 2, 3, 4, 1, 2, 5], radius_data=radius, type='barRadius', is_stack=True)
polar.show_config()
polar.render()
```
![polar-3](https://github.com/chenjiandongx/pyecharts/blob/master/images/polar-3.gif)

```python
from pyecharts import Polar

radius = ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
polar = Polar("极坐标系-堆叠柱状图示例", width=1200, height=600)
polar.add("", [1, 2, 3, 4, 3, 5, 1], radius_data=radius, type='barAngle', is_stack=True)
polar.add("", [2, 4, 6, 1, 2, 3, 1], radius_data=radius, type='barAngle', is_stack=True)
polar.add("", [1, 2, 3, 4, 1, 2, 5], radius_data=radius, type='barAngle', is_stack=True)
polar.show_config()
polar.render()
```
![polar-4](https://github.com/chenjiandongx/pyecharts/blob/master/images/polar-4.png)


## Radar（雷达图）
Radar.add() 方法签名
```python
add(name, value, item_color=None, **kwargs)
```
* name -> list  
    图例名称
* value -> [list],包含列表的列表 
    数据项。数据中，每一行是一个『数据项』，每一列属于一个『维度』
* item_color -> str  
    指定单图例颜色

Radar.config() 方法签名
```python
config(schema=None, c_schema=None, shape="", rader_text_color="#000", **kwargs):
```
* schema -> list  
    默认雷达图的指示器，用来指定雷达图中的多个维度，会对数据处理成 {name:xx, value:xx} 的字典
* c_schema -> dict  
    用户自定义雷达图的指示器，用来指定雷达图中的多个维度
    * name: 指示器名称
    * min: 指示器最小值
    * max: 指示器最大值
* shape -> str  
    雷达图绘制类型，有'polygon'（多边形）和'circle'可选
* rader_text_color -> str  
    雷达图数据项字体颜色，默认为'#000'

```python
from pyecharts import Radar

schema = [ 
    ("销售", 6500), ("管理", 16000), ("信息技术", 30000), ("客服", 38000), ("研发", 52000), ("市场", 25000)]
v1 = [[4300, 10000, 28000, 35000, 50000, 19000]]
v2 = [[5000, 14000, 28000, 31000, 42000, 21000]]
radar = Radar()
radar.config(schema)
radar.add("预算分配", v1, is_splitline=True, is_axisline_show=True)
radar.add("实际开销", v2, label_color=["#4e79a7"], is_area_show=False)
radar.show_config()
radar.render()
```
![radar-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/radar-0.gif)

* is_area_show -> bool  
    是否显示填充区域
* area_opacity -> float  
    填充区域透明度
* area_color -> str  
    填充区域颜色
* is_splitline_show  -> bool  
    是否显示分割线，默认为 True
* is_axisline_show -> bool  
    是否显示坐标轴线，默认为 True

**Tip：** 可配置 **lineStyle** 参数

```python
value_bj = [
    [55, 9, 56, 0.46, 18, 6, 1], [25, 11, 21, 0.65, 34, 9, 2],
    [56, 7, 63, 0.3, 14, 5, 3], [33, 7, 29, 0.33, 16, 6, 4],
    [42, 24, 44, 0.76, 40, 16, 5], [82, 58, 90, 1.77, 68, 33, 6],
    [74, 49, 77, 1.46, 48, 27, 7], [78, 55, 80, 1.29, 59, 29, 8],
    [267, 216, 280, 4.8, 108, 64, 9], [185, 127, 216, 2.52, 61, 27, 10],
    [39, 19, 38, 0.57, 31, 15, 11], [41, 11, 40, 0.43, 21, 7, 12],
    [64, 38, 74, 1.04, 46, 22, 13], [108, 79, 120, 1.7, 75, 41, 14],
    [108, 63, 116, 1.48, 44, 26, 15], [33, 6, 29, 0.34, 13, 5, 16],
    [94, 66, 110, 1.54, 62, 31, 17], [186, 142, 192, 3.88, 93, 79, 18],
    [57, 31, 54, 0.96, 32, 14, 19], [22, 8, 17, 0.48, 23, 10, 20],
    [39, 15, 36, 0.61, 29, 13, 21], [94, 69, 114, 2.08, 73, 39, 22],
    [99, 73, 110, 2.43, 76, 48, 23], [31, 12, 30, 0.5, 32, 16, 24],
    [42, 27, 43, 1, 53, 22, 25], [154, 117, 157, 3.05, 92, 58, 26],
    [234, 185, 230, 4.09, 123, 69, 27],[160, 120, 186, 2.77, 91, 50, 28],
    [134, 96, 165, 2.76, 83, 41, 29], [52, 24, 60, 1.03, 50, 21, 30],
]
value_sh = [
    [91, 45, 125, 0.82, 34, 23, 1], [65, 27, 78, 0.86, 45, 29, 2],
    [83, 60, 84, 1.09, 73, 27, 3], [109, 81, 121, 1.28, 68, 51, 4],
    [106, 77, 114, 1.07, 55, 51, 5], [109, 81, 121, 1.28, 68, 51, 6],
    [106, 77, 114, 1.07, 55, 51, 7], [89, 65, 78, 0.86, 51, 26, 8],
    [53, 33, 47, 0.64, 50, 17, 9], [80, 55, 80, 1.01, 75, 24, 10],
    [117, 81, 124, 1.03, 45, 24, 11], [99, 71, 142, 1.1, 62, 42, 12],
    [95, 69, 130, 1.28, 74, 50, 13], [116, 87, 131, 1.47, 84, 40, 14],
    [108, 80, 121, 1.3, 85, 37, 15], [134, 83, 167, 1.16, 57, 43, 16],
    [79, 43, 107, 1.05, 59, 37, 17], [71, 46, 89, 0.86, 64, 25, 18],
    [97, 71, 113, 1.17, 88, 31, 19], [84, 57, 91, 0.85, 55, 31, 20],
    [87, 63, 101, 0.9, 56, 41, 21], [104, 77, 119, 1.09, 73, 48, 22],
    [87, 62, 100, 1, 72, 28, 23], [168, 128, 172, 1.49, 97, 56, 24],
    [65, 45, 51, 0.74, 39, 17, 25], [39, 24, 38, 0.61, 47, 17, 26],
    [39, 24, 39, 0.59, 50, 19, 27], [93, 68, 96, 1.05, 79, 29, 28],
    [188, 143, 197, 1.66, 99, 51, 29], [174, 131, 174, 1.55, 108, 50, 30],
]
c_schema= [{"name": "AQI", "max": 300, "min": 5},
           {"name": "PM2.5", "max": 250, "min": 20},
           {"name": "PM10", "max": 300, "min": 5},
           {"name": "CO", "max": 5},
           {"name": "NO2", "max": 200},
           {"name": "SO2", "max": 100}]
radar = Radar()
radar.config(c_schema=c_schema, shape='circle')
radar.add("北京", value_bj, item_color="#f9713c", symbol=None)
radar.add("上海", value_sh, item_color="#b3e4a1", symbol=None)
radar.show_config()
radar.render()
```
![radar-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/radar-1.gif)

**Tip：** symblo=None 可隐藏标记图形（小圆圈）


## Scatter（散点图）
Scatter.add() 方法签名
```python
add(name, x_value, y_value, symbol_size=10, **kwargs)
```
* name -> str  
    图例名称
* x_axis -> list  
    x 坐标轴数据
* y_axis -> list  
    y 坐标轴数据
* symbol_size -> int  
    标记图形大小，默认为 10

```python
from pyecharts import Scatter

v1 = [10, 20, 30, 40, 50, 60]
v2 = [10, 20, 30, 40, 50, 60]
scatter = Scatter("散点图示例")
scatter.add("A", v1, v2)
scatter.add("B", v1[::-1], v2)
scatter.show_config()
scatter.render()
```
![scatter-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/scatter-0.png)

Scatter 还内置了画画方法
```python
draw(path, color=None)
''' 
将图片上的像素点转换为数组，如 color 为（255,255,255）时只保留非白色像素点的坐标信息  
返回两个 k_lst, v_lst 两个列表刚好作为散点图的数据项
'''
```
* path -> str  
    转换图片的地址
* color -> str  
    所要排除的颜色

首先你需要准备一张图片，如

![pyecharts-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/pyecharts-0.png)

```python
from pyecharts import Scatter

scatter = Scatter("散点图示例")
v1, v2 = scatter.draw("../images/pyecharts-0.png")
scatter.add("pyecharts", v1, v2, is_random=True)
scatter.show_config()
scatter.render()
```
![pyecharts-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/pyecharts-1.png)


## WordCloud（词云图）
WordCloud.add() 方法签名
```python
add(name, attr, value, shape="circle", word_gap=20, word_size_range=None, rotate_step=45)
```
* name -> str  
    图例名称
* attr -> list  
   属性名称
* value -> list  
   属性所对应的值
* shape -> list  
   词云图轮廓，有'circle', 'cardioid', 'diamond', 'triangle-forward', 'triangle', 'pentagon', 'star'可选
* word_gap -> int  
   单词间隔，默认为 20。
* word_size_range -> list  
   单词字体大小范围，默认为 [12, 60]。
* rotate_step -> int  
   旋转单词角度，默认为 45

```python
from pyecharts import WordCloud

name = ['Sam S Club', 'Macys', 'Amy Schumer', 'Jurassic World', 'Charter Communications',
        'Chick Fil A', 'Planet Fitness', 'Pitch Perfect', 'Express', 'Home', 'Johnny Depp',
        'Lena Dunham', 'Lewis Hamilton', 'KXAN', 'Mary Ellen Mark', 'Farrah Abraham',
        'Rita Ora', 'Serena Williams', 'NCAA baseball tournament', 'Point Break']
value = [10000, 6181, 4386, 4055, 2467, 2244, 1898, 1484, 1112, 965, 847, 582, 555,
         550, 462, 366, 360, 282, 273, 265]
wordcloud = WordCloud(width=1300, height=620)
wordcloud.add("", name, value, word_size_range=[20, 100])
wordcloud.show_config()
wordcloud.render()
```
![wordcloud-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/wordcloud-0.png)

```python
wordcloud = WordCloud(width=1300, height=620)
wordcloud.add("", name, value, word_size_range=[30, 100], shape='diamond')
wordcloud.show_config()
wordcloud.render()
```
![wordcloud-1](https://github.com/chenjiandongx/pyecharts/blob/master/images/wordcloud-1.png)

**Tip：** 当且仅当 shape 为默认的'circle'时 rotate_step 参数才生效


# 用户自定义
用户还可以自定义结合 Line/Bar 图表  
需使用 ```get_series()``` 和 ```custom()``` 方法  

```python
get_series()
""" 获取图表的 series 数据 """
```
```python
custom(series)
''' 追加自定义图表类型 '''
```
* series -> dict  
    追加图表类型的 series 数据

先用 ```get_series()``` 获取数据，再使用 ```custom()``` 将图表结合在一起  

```python
from pyecharts import Bar, Line

attr = ['A', 'B', 'C', 'D', 'E', 'F']
v1 = [10, 20, 30, 40, 50, 60]
v2 = [15, 25, 35, 45, 55, 65]
v3 = [38, 28, 58, 48, 78, 68]
bar = Bar("Line - Bar 示例")
bar.add("bar", attr, v1)
line = Line()
line.add("line", v2, v3)
bar.custom(line.get_series())
bar.show_config()
bar.render()
```
![custom-0](https://github.com/chenjiandongx/pyecharts/blob/master/images/custom-0.gif)

# 更多示例

* 更多示例请参考 [example.md](https://github.com/chenjiandongx/pyecharts/blob/master/example.md)
* 欢迎大家补充

# 关于项目

* 欢迎大家使用及提出意见
* // Todo