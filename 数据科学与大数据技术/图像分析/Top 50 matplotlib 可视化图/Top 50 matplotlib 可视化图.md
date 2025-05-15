### 壹  相关性（Correlation）

###### · 相关图用于可视化两个或多个变量之间的关系，即一个变量如何相对于另一个变量变化

##### 1. 散点图（Scatter plot）：
~~~
散点图是用来研究两个变量之间关系的一种经典的、基本的图。如果您的数据中有多个组，您可能希望用不同的颜色来显示每个组。在 matplotlib 中，您可以使用 plt.scatterplot() 方便地实现这一点。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
  
# 从本地文件导入数据集  
midwest = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/midwest_filter.csv")  
  
# 准备数据  
# 创建与midwest['category']中唯一值数量相同的颜色  
categories = np.unique(midwest['category'])  
colors = [plt.cm.tab10(i/float(len(categories)-1)) for i in range(len(categories))]  
  
# 为每个类别绘制散点图  
plt.figure(figsize=(16, 10), dpi= 80, facecolor='w', edgecolor='k')  
  
for i, category in enumerate(categories):  
    plt.scatter('area', 'poptotal',  
                data=midwest.loc[midwest.category==category, :],  
                s=20, c=colors[i], label=str(category))  
  
# 图形修饰  
plt.gca().set(xlim=(0.0, 0.1), ylim=(0, 90000),  
              xlabel='Area', ylabel='population')  
  
plt.xticks(fontsize=12); plt.yticks(fontsize=12)  
plt.title("Scatter Map of Area and Population in Central and Western China", fontsize=22)  
plt.legend(fontsize=12)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图1.png)
                          （图一：散点图 Scatter plot 示例）

##### 2. 有边界气泡图（Bubble plot with Encircling）：
~~~
有时您想在一个边界内显示一组点来强调它们的重要性。在本例中，您从数据框中获取应该被包围的记录，并将其传递给下面代码中描述的 encircle()。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
from matplotlib import patches  
from scipy.spatial import ConvexHull  
import seaborn as sns  
import warnings  
  
# 忽略警告  
warnings.simplefilter('ignore')  
  
# 设置Seaborn风格为白色背景  
sns.set_style("white")  
  
# Step 1: Prepare Data  
midwest = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/midwest_filter.csv")  
  
# As many colors as there are unique midwest['category']  
categories = np.unique(midwest['category'])  
colors = [plt.cm.tab10(i/float(len(categories)-1)) for i in range(len(categories))]  
  
# Step 2: Draw Scatterplot with unique color for each category  
fig = plt.figure(figsize=(16, 10), dpi= 80, facecolor='w', edgecolor='k')  
  
for i, category in enumerate(categories):  
    plt.scatter('area', 'poptotal', data=midwest.loc[midwest.category==category, :], s='dot_size', c=colors[i], label=str(category), edgecolors='black', linewidths=.5)  
  
# Step 3: Encircling  
# https://stackoverflow.com/questions/44575681/how-do-i-encircle-different-data-sets-in-scatter-plot  
def encircle(x,y, ax=None, **kw):  
    if not ax: ax=plt.gca()  
    p = np.c_[x,y]  
    hull = ConvexHull(p)  
    poly = plt.Polygon(p[hull.vertices,:], **kw)  
    ax.add_patch(poly)  
  
# Select data to be encircled  
midwest_encircle_data = midwest.loc[midwest.state=='IN', :]  
  
# Draw polygon surrounding vertices  
encircle(midwest_encircle_data.area, midwest_encircle_data.poptotal, ec="k", fc="gold", alpha=0.1)  
encircle(midwest_encircle_data.area, midwest_encircle_data.poptotal, ec="firebrick", fc="none", linewidth=1.5)  
  
# Step 4: Decorations  
plt.gca().set(xlim=(0.0, 0.1), ylim=(0, 90000),  
              xlabel='Area', ylabel='Population')  
  
plt.xticks(fontsize=12); plt.yticks(fontsize=12)  
plt.title("Bubble Plot with Encircling", fontsize=22)  
plt.legend(fontsize=12)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图2.png)
              （图二：有边界气泡图（Bubble plot with Encircling）示例）

##### 3. 具有最佳拟合线性回归线的散点图（Scatter plot with linear regression line of best fit）：
~~~
如果您想了解两个变量是如何相互变化的，最佳拟合线就是最好的方法。下图显示了数据中不同组的最佳拟合线的差异。要禁用分组并仅为整个数据集绘制一条最佳拟合行，请从下面的 sn .lmplot() 调用中删除 hue='cyl' 参数。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/mpg_ggplot2.csv")  
df_select = df.loc[df.cyl.isin([4, 8]), :]  
  
# 绘图  
sns.set_style("white")  
gridobj = sns.lmplot(x="displ", y="hwy", hue="cyl", data=df_select,  
                     height=2, aspect=1.6, robust=True, palette='tab10',  
                     scatter_kws=dict(s=60, linewidths=.7, edgecolors='black'),  
                     truncate=False)  
# displ 为汽车排量  
# hwy 为公路里程  
# cyl 表示汽缸  
  
# 图形修饰  
gridobj.set(xlim=(0.5, 7.5), ylim=(0, 50))  
plt.title("Scatterplot with line of best fit grouped by number of cylinders", fontsize=20)  
# 不同汽缸数量分组的散点图与最佳拟合线  
  
# 调整底部边距  
plt.subplots_adjust(bottom=0.1)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图3.png)
（图三：具有最佳拟合线性回归线的散点图（Scatter plot with linear regression line of best fit）示例）
~~~
或者，您可以在各自的列中显示每个组的最佳拟合线。可以通过在 sn.lmplot() 中设置 col=groupingcolumn 参数来实现这一点。
~~~
###### · 分离图：
```Python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 导入数据
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/mpg_ggplot2.csv")
df_select = df.loc[df.cyl.isin([4,8]), :]

# 每个回归线都在自己的列中
sns.set_style("white")
gridobj = sns.lmplot(x="displ", y="hwy",
                     data=df_select,
                     height=7,
                     robust=True,
                     palette='Set1',
                     col="cyl",
                     hue_order=[4, 8],
                     scatter_kws=dict(s=60, linewidths=.7, edgecolors='black'),
                     truncate=False)  # 设置truncate参数为False

# 装饰
gridobj.set(xlim=(0.5, 7.5), ylim=(0, 50))

# 修改图例为英语
plt.legend(title='Cylinders', labels=['4 Cyl', '8 Cyl'])

plt.show()
```
![|700](“Top%2050”图/Top%2050图4.png)
                       （图四：每条回归线单独一个散点图的示例）

##### 4. 抖动图（Jittering with stripplot）：
~~~
通常多个数据点具有完全相同的 X 和 Y 值。结果多个点被绘制在彼此之上并隐藏覆盖起来。为了避免这种情况，稍微“抖动“点，这样您就可以直观地看到它们。使用 seaborn 的 stripplot() 可以很方便地做到这一点。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/mpg_ggplot2.csv")  
  
# 绘制 Stripplotfig, ax = plt.subplots(figsize=(16,10), dpi= 80)  
sns.stripplot(x=df['cty'], y=df['hwy'], jitter=0.25, size=8, ax=ax, linewidth=.5,  
              palette="rainbow")  # “彩虹”颜色映射
  
# 图形修饰  
plt.title('Use jittered plots to avoid overlapping of points', fontsize=22)  
# 使用抖动图避免数据重叠  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图5.png)
                   （图五：抖动图（Jittering with stripplot）的示例）

##### 5. 计数图（Counts plot）：
~~~
避免点重叠问题的另一个选择是根据点上有多少个点来增加点的大小。所以，点的大小越大，它周围的点的浓度就越大。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/mpg_ggplot2.csv")  
df_counts = df.groupby(['hwy', 'cty']).size().reset_index(name='counts')  
  
# 绘制 Counts Plotplt.figure(figsize=(16,10))  
sns.scatterplot(data=df_counts, x='cty', y='hwy', size='counts', hue='counts', palette='rainbow', alpha=0.6)  
  
# 图形修饰  
plt.title('Counts Plot - Size of circle is bigger as more points overlap', fontsize=22)  
plt.xlabel('City MPG')  
plt.ylabel('Highway MPG')  
plt.legend(title='Counts')  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图6.png)
                        （图六：计数图（Counts plot）的示例）

##### 6. 边缘直方图（Marginal Histogram）：
~~~
边缘直方图有沿 X 轴和 Y 轴变量的直方图。这用于可视化 X 和 Y 之间的关系以及 X 和 Y 各自的单变量分布。此图常用于探索性数据分析（EDA）。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/mpg_ggplot2.csv")  
  
# 创建图形和网格  
fig = plt.figure(figsize=(16, 10), dpi= 80)  
grid = plt.GridSpec(4, 4, hspace=0.5, wspace=0.2)  
  
# 定义子图  
ax_main = fig.add_subplot(grid[:-1, :-1])  
ax_right = fig.add_subplot(grid[:-1, -1], xticklabels=[], yticklabels=[])  
ax_bottom = fig.add_subplot(grid[-1, 0:-1], xticklabels=[], yticklabels=[])  
  
# 主图上的散点图  
ax_main.scatter('displ', 'hwy', s=df.cty*4, c=df.manufacturer.astype('category').cat.codes, alpha=.9, data=df, cmap="tab10", edgecolors='gray', linewidths=.5)  
  
# 右侧的直方图  
ax_bottom.hist(df.displ, 40, histtype='stepfilled', orientation='vertical', color='deeppink')  
ax_bottom.invert_yaxis()  
  
# 底部的直方图  
ax_right.hist(df.hwy, 40, histtype='stepfilled', orientation='horizontal', color='deeppink')  
  
# 图形修饰  
ax_main.set(title='Scatterplot with Histograms \n displ vs hwy', xlabel='displ', ylabel='hwy')  
ax_main.title.set_fontsize(20)  
for item in ([ax_main.xaxis.label, ax_main.yaxis.label] + ax_main.get_xticklabels() + ax_main.get_yticklabels()):  
    item.set_fontsize(14)  
  
xlabels = ax_main.get_xticks().tolist()  
ax_main.set_xticklabels(xlabels)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图7.png)
                   （图七：边缘直方图（Marginal Histogram）的示例）

##### 7. 边缘箱线图（Marginal Boxplot）：
~~~
边际箱线图的作用与边际直方图相似。然而，箱线图有助于确定 X 和 Y 的中位数、第 25 和第 75 百分位数。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/mpg_ggplot2.csv")  
  
# 创建图形和网格  
fig = plt.figure(figsize=(16, 10), dpi= 80)  
grid = plt.GridSpec(4, 4, hspace=0.5, wspace=0.2)  
  
# 定义子图  
ax_main = fig.add_subplot(grid[:-1, :-1])  
ax_right = fig.add_subplot(grid[:-1, -1], xticklabels=[], yticklabels=[])  
ax_bottom = fig.add_subplot(grid[-1, 0:-1], xticklabels=[], yticklabels=[])  
  
# 主图上的散点图  
ax_main.scatter('displ', 'hwy', s=df.cty*5, c=df.manufacturer.astype('category').cat.codes, alpha=.9, data=df, cmap="Set1", edgecolors='black', linewidths=.5)  
  
# 在每个部分添加一个图形  
sns.boxplot(df.hwy, ax=ax_right, orient="v")  
sns.boxplot(df.displ, ax=ax_bottom, orient="h")  
  
# 图形修饰  
# 删除箱线图的 x 轴名称  
ax_bottom.set(xlabel='')  
ax_right.set(ylabel='')  
  
# 主标题、X轴标签和Y轴标签  
ax_main.set(title='Scatterplot with Histograms \n displ vs hwy', xlabel='displ', ylabel='hwy')  
  
# 设置不同组件的字体大小  
ax_main.title.set_fontsize(20)  
for item in ([ax_main.xaxis.label, ax_main.yaxis.label] + ax_main.get_xticklabels() + ax_main.get_yticklabels()):  
    item.set_fontsize(14)  
  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图8.png)
                    （图八：边缘箱线图（Marginal Boxplot）的示例）

##### 8. 相关图（Correllogram）：
~~~
相关图用于直观地查看给定数据框（或2D数组）中所有可能的数值变量对之间的相关性度量。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据集  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mtcars.csv")  
  
# 选择数值类型的列  
numeric_cols = df.select_dtypes(include=['float64', 'int64'])  
  
# 绘制热力图  
plt.figure(figsize=(12,10), dpi= 80)  
sns.heatmap(numeric_cols.corr(), xticklabels=numeric_cols.corr().columns, yticklabels=numeric_cols.corr().columns, cmap='RdYlGn', center=0, annot=True)  
  
# 图形修饰  
plt.title('Correlogram of mtcars', fontsize=22)  
plt.xticks(fontsize=12)  
plt.yticks(fontsize=12)  
plt.show()
```
###### · 绘图：
![|450](“Top%2050”图/Top%2050图9.png)
                        （图九： 相关图（Correllogram）的示例）

##### 9. 成对图（Pairwise plot）：
~~~
成对图是探索性分析中最常用的方法，用于理解所有可能的数值变量对之间的关系。它是双变量分析的必备工具。
~~~
###### · 绘图代码：
```Python
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 加载数据集  
df = sns.load_dataset('iris')  
  
# 绘制成对关系图  
plt.figure(figsize=(10,8), dpi= 80)  
sns.pairplot(df, kind="scatter", hue="species", plot_kws=dict(s=80, edgecolor="white", linewidth=2.5))  
plt.show()
```
###### · 绘图：
![](“Top%2050”图/Top%2050图10.png)
（图十：成对图（Pairwise plot）的示例）


### 贰  偏差（Deviation）

###### · 如果您希望看到项目是如何基于单一指标变化的，并且希望可以可视化这种变化的顺序和数量，发散条形图是一个很好的工具
###### · 它有助于快速区分数据组的性能，并且非常直观，可以立即传达要点

##### 10. 发散型条形图（Diverging Bars）：
```
如果您希望看到项目是如何基于单一指标变化的，并可视化这种变化的顺序和数量，发散条形图是一个很好的工具。它有助于快速区分数据组的性能，并且非常直观，可以立即传达要点。
```
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 准备数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mtcars.csv")  
x = df.loc[:, ['mpg']]  
df['mpg_z'] = (x - x.mean())/x.std()  
df['colors'] = ['red' if x < 0 else 'green' for x in df['mpg_z']]  
df.sort_values('mpg_z', inplace=True)  
df.reset_index(inplace=True)  
  
# 绘制图表  
plt.figure(figsize=(14,10), dpi= 80)  
plt.hlines(y=df.index, xmin=0, xmax=df.mpg_z, color=df.colors, alpha=0.4, linewidth=5)  
  
# 图形修饰  
plt.gca().set(ylabel='$Model$', xlabel='$Mileage$')  
plt.yticks(df.index, df.cars, fontsize=12)  
plt.title('Diverging Bars of Car Mileage', fontdict={'size':20})  
plt.grid(linestyle='--', alpha=0.5)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图11.png)
                    （图十一：发散型条形图（Diverging Bars）的示例）

##### 11. 发散型条形图数值标签（Diverging texts）：
~~~
类似于发散条形图，如果您希望以美观的方式显示图表中每个项目的值，则首选 Diverging texts。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import requests  
from io import StringIO  
  
# 下载数据  
url = "https://github.com/selva86/datasets/raw/master/mtcars.csv"  
response = requests.get(url)  
data = StringIO(response.text)  
df = pd.read_csv(data)  
  
x = df.loc[:, ['mpg']]  
df['mpg_z'] = (x - x.mean()) / x.std()  
df['colors'] = ['red' if x < 0 else 'green' for x in df['mpg_z']]  
df.sort_values('mpg_z', inplace=True)  
df.reset_index(inplace=True)  
  
# 绘图  
plt.figure(figsize=(14, 14), dpi=80)  
plt.hlines(y=df.index, xmin=0, xmax=df.mpg_z)  
for x, y, tex in zip(df.mpg_z, df.index, df.mpg_z):  
    t = plt.text(x, y, round(tex, 2), horizontalalignment='right' if x < 0 else 'left',  
                 verticalalignment='center', fontdict={'color': 'red' if x < 0 else 'green', 'size': 14})  
  
# 图形装饰  
plt.yticks(df.index, df.cars, fontsize=12)  
plt.title('Diverging Text Bars of Car Mileage', fontdict={'size': 20})  
plt.grid(linestyle='--', alpha=0.5)  
plt.xlim(-2.5, 2.5)  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图12.png)
                （图十二：发散型条形图数值标签（Diverging texts）的示例）

##### 12. 发散点图（Diverging dot plot）：
~~~
散点图也类似于散条图。然而，与发散的条形图相比，没有条形图减少了组间的对比和差异。
~~~
###### · 绘图代码：
```Python
import pandas as pd
import matplotlib.pyplot as plt

# Prepare Data
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mtcars.csv")
x = df.loc[:, ['mpg']]
df['mpg_z'] = (x - x.mean())/x.std()
df['colors'] = ['red' if x < 0 else 'darkgreen' for x in df['mpg_z']]
df.sort_values('mpg_z', inplace=True)
df.reset_index(inplace=True)

# Draw plot
plt.figure(figsize=(14,16), dpi= 80)
plt.scatter(df.mpg_z, df.index, s=450, alpha=.6, color=df.colors)
for x, y, tex in zip(df.mpg_z, df.index, df.mpg_z):
    t = plt.text(x, y, round(tex, 1), horizontalalignment='center', 
                 verticalalignment='center', fontdict={'color':'white'})

# Decorations
# Lighten borders
plt.gca().spines["top"].set_alpha(.3)
plt.gca().spines["bottom"].set_alpha(.3)
plt.gca().spines["right"].set_alpha(.3)
plt.gca().spines["left"].set_alpha(.3)

plt.yticks(df.index, df.cars)
plt.title('Diverging Dotplot of Car Mileage', fontdict={'size':20})
plt.xlabel('$Mileage$')
plt.grid(linestyle='--', alpha=0.5)
plt.xlim(-2.5, 2.5)
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图13.png)
                    （图十三：发散点图（Diverging dot plot）的示例）

##### 13. 发散棒棒糖图（带标记）（Diverging lollipop chart with markers）：
~~~
带有标记的 Lollipop 提供了一种灵活的方式，通过强调您想要引起注意的任何重要数据点，并在图表中适当地给出推理，从而将分歧可视化。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import matplotlib.patches as patches  
  
# Prepare Data  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mtcars.csv")  
x = df.loc[:, ['mpg']]  
df['mpg_z'] = (x - x.mean())/x.std()  
df['colors'] = 'black'  
  
# Color Fiat differently  
df.loc[df.cars == 'Fiat X1-9', 'colors'] = 'darkorange'  
df.sort_values('mpg_z', inplace=True)  
df.reset_index(inplace=True)  
  
# Draw plot  
plt.figure(figsize=(14,16), dpi= 80)  
plt.hlines(y=df.index, xmin=0, xmax=df.mpg_z, color=df.colors, alpha=0.4, linewidth=1)  
plt.scatter(df.mpg_z, df.index, color=df.colors, s=[600 if x == 'Fiat X1-9' else 300 for x in df.cars], alpha=0.6)  
plt.yticks(df.index, df.cars)  
plt.xticks(fontsize=12)  
  
# Annotate  
plt.annotate('Mercedes Models', xy=(0.0, 11.0), xytext=(1.0, 11), xycoords='data',  
            fontsize=15, ha='center', va='center',  
            bbox=dict(boxstyle='square', fc='firebrick'),  
            arrowprops=dict(arrowstyle='-[, widthB=2.0, lengthB=1.5', lw=2.0, color='steelblue'), color='white')  
  
# Add Patches  
p1 = patches.Rectangle((-2.0, -1), width=.3, height=3, alpha=.2, facecolor='red')  
p2 = patches.Rectangle((1.5, 27), width=.8, height=5, alpha=.2, facecolor='green')  
plt.gca().add_patch(p1)  
plt.gca().add_patch(p2)  
  
# Decorate  
plt.title('Diverging Bars of Car Mileage', fontdict={'size':20})  
plt.grid(linestyle='--', alpha=0.5)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图14.png)
                （图十四：发散棒棒糖图（Diverging lollipop chart）的示例）

##### 14. 面积图（Area chart）：
~~~
通过给轴和线之间的区域上色，面积图不仅强调了峰值和低谷，还强调了高点和低点的持续时间。高点持续的时间越长，线下的面积就越大。
~~~
###### · 绘图代码：
```Python
import numpy as np  
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 准备数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/economics.csv", parse_dates=['date']).head(100)  
x = np.arange(df.shape[0])  
y_returns = (df.psavert.diff().fillna(0)/df.psavert.shift(1)).fillna(0) * 100  
  
# 绘图  
plt.figure(figsize=(16,10), dpi= 80)  
plt.fill_between(x[1:], y_returns[1:], 0, where=y_returns[1:] >= 0, facecolor='green', interpolate=True, alpha=0.7)  
plt.fill_between(x[1:], y_returns[1:], 0, where=y_returns[1:] <= 0, facecolor='red', interpolate=True, alpha=0.7)  
  
# 注释  
plt.annotate('Peak \n1975', xy=(94.0, 21.0), xytext=(88.0, 28),  
             bbox=dict(boxstyle='square', fc='firebrick'),  
             arrowprops=dict(facecolor='steelblue', shrink=0.05), fontsize=15, color='white')  
  
  
# 装饰  
xtickvals = [str(m)[:3].upper()+"-"+str(y) for y,m in zip(df.date.dt.year, df.date.dt.month_name())]  
plt.gca().set_xticks(x[::6])  
plt.gca().set_xticklabels(xtickvals[::6], rotation=90, fontdict={'horizontalalignment': 'center', 'verticalalignment': 'center_baseline'})  
plt.ylim(-35,35)  
plt.xlim(1,100)  
plt.title("Month Economics Return %", fontsize=22)  
plt.ylabel('Monthly returns %')  
plt.grid(alpha=0.5)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图15.png)
                       （图十五：面积图（Area chart）的示例）


### 叁  排序（Ranking）

##### 15. 有序条形图（Ordered bar chart）：
~~~
有序柱状图有效地传达了项目的等级顺序。但是在图表上方添加度量值，用户就可以从图表本身获得精确的信息。这是一种基于计数或任何给定度量来可视化项目的经典方法。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import matplotlib.patches as patches  
  
# Prepare Data  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
df = df_raw[['cty', 'manufacturer']].groupby('manufacturer').apply(lambda x: x.mean())  
df.sort_values('cty', inplace=True)  
df.reset_index(inplace=True)  
  
# Draw plot  
fig, ax = plt.subplots(figsize=(16,10), facecolor='white', dpi= 80)  
ax.vlines(x=df.index, ymin=0, ymax=df.cty, color='firebrick', alpha=0.7, linewidth=20)  
  
# Annotate Text  
for i, cty in enumerate(df.cty):  
    ax.text(i, cty+0.5, round(cty, 1), horizontalalignment='center')  
  
  
# Title, Label, Ticks and Ylim  
ax.set_title('Bar Chart for Highway Mileage', fontdict={'size':22})  
ax.set(ylabel='Miles Per Gallon', ylim=(0, 30))  
plt.xticks(df.index, df.manufacturer, rotation=60, horizontalalignment='right', fontsize=12)  
  
# Add patches to color the X axis labels  
p1 = patches.Rectangle((.57, -0.005), width=.33, height=.13, alpha=.1, facecolor='green', transform=fig.transFigure)  
p2 = patches.Rectangle((.124, -0.005), width=.446, height=.13, alpha=.1, facecolor='red', transform=fig.transFigure)  
fig.add_artist(p1)  
fig.add_artist(p2)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图16.png)
                   （图十六：有序条形图（Ordered bar chart）的示例）

##### 16. 棒棒糖图（Lollipop chart）：
~~~
棒棒糖图的作用与有序条形图相似，具有视觉上的愉悦性。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 准备数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
df = df_raw[['cty', 'manufacturer']].groupby('manufacturer').apply(lambda x: x.mean())  
df.sort_values('cty', inplace=True)  
df.reset_index(inplace=True)  
  
# 绘制图表  
fig, ax = plt.subplots(figsize=(16,10), dpi= 80)  
ax.vlines(x=df.index, ymin=0, ymax=df.cty, color='firebrick', alpha=0.7, linewidth=2)  
ax.scatter(x=df.index, y=df.cty, s=75, color='firebrick', alpha=0.7)  
  
# 标题、标签、刻度和Y轴范围  
ax.set_title('Lollipop Chart for Highway Mileage', fontdict={'size':22})  
ax.set_ylabel('Miles Per Gallon')  
ax.set_xticks(df.index)  
ax.set_xticklabels(df.manufacturer, rotation=60, fontdict={'horizontalalignment': 'right', 'size':12})  
ax.set_ylim(0, 30)  
  
# 注释  
for row in df.itertuples():  
    ax.text(row.Index, row.cty+.5, s=round(row.cty, 2), horizontalalignment= 'center', verticalalignment='bottom', fontsize=14)  
  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图17.png)
                      （图十七：棒棒糖图（Lollipop chart）的示例）

##### 17. 点状图（Dot plot）：
~~~
点图表示项目的等级顺序。因为它是沿水平轴对齐的，你可以很容易地想象出点之间的距离。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 准备数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
df = df_raw[['cty', 'manufacturer']].groupby('manufacturer').apply(lambda x: x.mean())  
df.sort_values('cty', inplace=True)  
df.reset_index(inplace=True)  
  
# 绘制图表  
fig, ax = plt.subplots(figsize=(16,10), dpi= 80)  
ax.hlines(y=df.index, xmin=11, xmax=26, color='gray', alpha=0.7, linewidth=1, linestyles='dashdot')  
ax.scatter(y=df.index, x=df.cty, s=75, color='firebrick', alpha=0.7)  
  
# 标题、标签、刻度和Y轴范围  
ax.set_title('Dot Plot for Highway Mileage', fontdict={'size':22})  
ax.set_xlabel('Miles Per Gallon')  
ax.set_yticks(df.index)  
ax.set_yticklabels(df.manufacturer.str.title(), fontdict={'horizontalalignment': 'right'})  
ax.set_xlim(10, 27)  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图18.png)
                         （图十八：点状图（Dot plot）的示例）

##### 18. 坡度图（Slope chart）：
~~~
坡度图最适合比较某一特定人物或物品的“之前”和“之后”位置。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import numpy as np  
import matplotlib.lines as mlines  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/gdppercap.csv")  
  
left_label = [str(c) + ', '+ str(round(y)) for c, y in zip(df.continent, df['1952'])]  
right_label = [str(c) + ', '+ str(round(y)) for c, y in zip(df.continent, df['1957'])]  
klass = ['red' if (y1-y2) < 0 else 'green' for y1, y2 in zip(df['1952'], df['1957'])]  
  
# 画线  
# https://stackoverflow.com/questions/36470343/how-to-draw-a-line-with-matplotlib/36479941  
def newline(p1, p2, color='black'):  
    ax = plt.gca()  
    l = mlines.Line2D([p1[0],p2[0]], [p1[1],p2[1]], color='red' if p1[1]-p2[1] > 0 else 'green', marker='o', markersize=6)  
    ax.add_line(l)  
    return l  
  
fig, ax = plt.subplots(1,1,figsize=(14,14), dpi= 80)  
  
# 垂直线  
ax.vlines(x=1, ymin=500, ymax=13000, color='black', alpha=0.7, linewidth=1, linestyles='dotted')  
ax.vlines(x=3, ymin=500, ymax=13000, color='black', alpha=0.7, linewidth=1, linestyles='dotted')  
  
# 点  
ax.scatter(y=df['1952'], x=np.repeat(1, df.shape[0]), s=10, color='black', alpha=0.7)  
ax.scatter(y=df['1957'], x=np.repeat(3, df.shape[0]), s=10, color='black', alpha=0.7)  
  
# 线段和注释  
for p1, p2, c in zip(df['1952'], df['1957'], df['continent']):  
    newline([1,p1], [3,p2])  
    ax.text(1-0.05, p1, c + ', ' + str(round(p1)), horizontalalignment='right', verticalalignment='center', fontdict={'size':14})  
    ax.text(3+0.05, p2, c + ', ' + str(round(p2)), horizontalalignment='left', verticalalignment='center', fontdict={'size':14})  
  
# 'Before' 和 'After' 注释  
ax.text(1-0.05, 13000, 'BEFORE', horizontalalignment='right', verticalalignment='center', fontdict={'size':18, 'weight':700})  
ax.text(3+0.05, 13000, 'AFTER', horizontalalignment='left', verticalalignment='center', fontdict={'size':18, 'weight':700})  
  
# 装饰  
ax.set_title("Slope chart: Comparing GDP Per Capita between 1952 vs 1957", fontdict={'size':22})  
ax.set(xlim=(0,4), ylim=(0,14000), ylabel='Mean GDP Per Capita')  
ax.set_xticks([1,3])  
ax.set_xticklabels(["1952", "1957"])  
plt.yticks(np.arange(500, 13000, 2000), fontsize=12)  
  
# 减弱边框  
plt.gca().spines["top"].set_alpha(.0)  
plt.gca().spines["bottom"].set_alpha(.0)  
plt.gca().spines["right"].set_alpha(.0)  
plt.gca().spines["left"].set_alpha(.0)  
plt.show()
```
###### · 绘图：
![|450](“Top%2050”图/Top%2050图19.png)
                        （图十九：坡度图（Slope chart）的示例）

##### 19. 哑铃图（Dumbbell plot）：
~~~
哑铃图传达了各种项目的“之前”和“之后”位置以及项目的排名顺序。如果你想可视化一个特定项目或计划对不同对象的影响，这是非常有用的。
~~~
###### · 绘图代码：
```Python
import matplotlib.pyplot as plt
import pandas as pd
import matplotlib.lines as mlines

# 导入数据
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/health.csv")
df.sort_values('pct_2014', inplace=True)
df.reset_index(inplace=True)

# 绘制线段的函数
def newline(p1, p2, color='black'):
    ax = plt.gca()
    l = mlines.Line2D([p1[0], p2[0]], [p1[1], p2[1]], color='skyblue')
    ax.add_line(l)
    return l

# 图和坐标轴
fig, ax = plt.subplots(1, 1, figsize=(14, 14), facecolor='#f7f7f7', dpi=80)

# 垂直线
ax.vlines(x=.05, ymin=0, ymax=26, color='black', alpha=1, linewidth=1, linestyles='dotted')
ax.vlines(x=.10, ymin=0, ymax=26, color='black', alpha=1, linewidth=1, linestyles='dotted')
ax.vlines(x=.15, ymin=0, ymax=26, color='black', alpha=1, linewidth=1, linestyles='dotted')
ax.vlines(x=.20, ymin=0, ymax=26, color='black', alpha=1, linewidth=1, linestyles='dotted')

# 点
ax.scatter(y=df['index'], x=df['pct_2013'], s=50, color='#0e668b', alpha=0.7)
ax.scatter(y=df['index'], x=df['pct_2014'], s=50, color='#a3c4dc', alpha=0.7)

# 线段
for i, p1, p2 in zip(df['index'], df['pct_2013'], df['pct_2014']):
    newline([p1, i], [p2, i])

# 装饰
ax.set_facecolor('#f7f7f7')
ax.set_title("Dumbbell Chart: Percentage Change - 2013 vs 2014", fontdict={'size':22})
ax.set(xlim=(0, .25), ylim=(-1, 27), ylabel='Mean GDP Per Capita')
ax.set_xticks([.05, .1, .15, .20])
ax.set_xticklabels(['5%', '15%', '20%', '25%'])
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图20.png)
                      （图二十：哑铃图（Dumbbell plot）的示例）


### 肆  分布（Distribution）

##### 20. 连续变量的直方图（Histogram for continuous variable）：
~~~
直方图显示给定变量的频率分布。下例基于分类变量对频率条进行分组，从而更深入地了解连续变量和分类变量的串联。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import numpy as np  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 准备数据  
x_var = 'displ'  
groupby_var = 'class'  
df_agg = df.loc[:, [x_var, groupby_var]].groupby(groupby_var)  
vals = [df[x_var].values.tolist() for i, df in df_agg]  
  
# 绘图  
plt.figure(figsize=(16,9), dpi= 80)  
colors = [plt.cm.Spectral(i/float(len(vals)-1)) for i in range(len(vals))]  
n, bins, patches = plt.hist(vals, 30, stacked=True, density=False, color=colors[:len(vals)])  
  
# 装饰  
plt.legend({group:col for group, col in zip(np.unique(df[groupby_var]).tolist(), colors[:len(vals)])})  
plt.title(f"Stacked Histogram of ${x_var}$ colored by ${groupby_var}$", fontsize=22)  
plt.xlabel(x_var)  
plt.ylabel("Frequency")  
plt.ylim(0, 25)  
plt.xticks(ticks=bins[::3], labels=[round(b,1) for b in bins[::3]])  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图21.png)
        （图二十一：连续变量的直方图（Histogram for continuous variable）的示例）

##### 21. 分类变量直方图（Histogram for categorical variable）：
~~~
分类变量的直方图显示该变量的频率分布。通过给条形图上色，可以将分布与代表颜色的另一个分类变量的联系可视化。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 准备数据  
x_var = 'manufacturer'  
groupby_var = 'class'  
df_agg = df.loc[:, [x_var, groupby_var]].groupby(groupby_var)  
vals = [df[x_var].values.tolist() for i, df in df_agg]  
  
# 绘图  
plt.figure(figsize=(16,9), dpi= 80)  
colors = [plt.cm.Spectral(i/float(len(vals)-1)) for i in range(len(vals))]  
n, bins, patches = plt.hist(vals, df[x_var].nunique(), stacked=True, density=False, color=colors[:len(vals)])  
  
# 装饰  
plt.legend({group:col for group, col in zip(np.unique(df[groupby_var]).tolist(), colors[:len(vals)])})  
plt.title(f"Stacked Histogram of ${x_var}$ colored by ${groupby_var}$", fontsize=22)  
plt.xlabel(x_var)  
plt.ylabel("Frequency")  
plt.ylim(0, 40)  
plt.xticks(rotation=90)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图22.png)
          （图二十二：分类变量直方图（Histogram for categorical variable）的示例）

##### 22. 密度图（Density plot）：
~~~
密度图是一种常用的可视化连续变量分布的工具。通过按“响应”变量对它们进行分组，您可以检查 X 和 Y 之间的关系。下面的示例用于表示目的，以描述城市里程的分布如何随汽缸数量而变化。
~~~
###### · 绘图代码：
```Python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 导入数据
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")

# 绘图
plt.figure(figsize=(16,10), dpi= 80)
sns.kdeplot(df.loc[df['cyl'] == 4, "cty"], shade=True, color="g", label="Cyl=4", alpha=.7)
sns.kdeplot(df.loc[df['cyl'] == 5, "cty"], shade=True, color="deeppink", label="Cyl=5", alpha=.7)
sns.kdeplot(df.loc[df['cyl'] == 6, "cty"], shade=True, color="dodgerblue", label="Cyl=6", alpha=.7)
sns.kdeplot(df.loc[df['cyl'] == 8, "cty"], shade=True, color="orange", label="Cyl=8", alpha=.7)

# Decoration
plt.title('Density Plot of City Mileage by Number of Cylinders', fontsize=22)
plt.legend()
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图23.png)
                      （图二十三：密度图（Density plot）的示例）

##### 23. 直方密度线图（Density curves with histogram）：
~~~
直方图的密度曲线将两个图所传达的信息集合在一起，这样您就可以把它们都放在一个图中而不是两个图中。
~~~
###### · 绘图代码：
```Python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 导入数据
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")

# 绘图
plt.figure(figsize=(13,10), dpi= 80)
sns.distplot(df.loc[df['class'] == 'compact', "cty"], color="dodgerblue", label="Compact", hist_kws={'alpha':.7}, kde_kws={'linewidth':3})
sns.distplot(df.loc[df['class'] == 'suv', "cty"], color="orange", label="SUV", hist_kws={'alpha':.7}, kde_kws={'linewidth':3})
sns.distplot(df.loc[df['class'] == 'minivan', "cty"], color="g", label="minivan", hist_kws={'alpha':.7}, kde_kws={'linewidth':3})
plt.ylim(0, 0.35)

# Decoration
plt.title('Density Plot of City Mileage by Vehicle Type', fontsize=22)
plt.legend()
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图24.png)
            （图二十四：直方密度线图（Density curves with histogram）的示例）

##### 24. 峰峦图（Joy plot）：
~~~
峰峦图可以让不同群体的密度曲线重叠，这是一个很好的方法来想象一个更大数量的群体之间的分布。它看起来很“讨人喜欢”，清晰地传达了正确的信息。它可以很容易地使用基于 matplotlib 的 joypy 包来构建。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import joypy  
  
# 导入数据  
mpg = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 绘制图形  
plt.figure(figsize=(8,5), dpi= 80)  
fig, axes = joypy.joyplot(mpg, column=['hwy', 'cty'], by="class", ylim='own', figsize=(3.5, 2.5))  
  
# 装饰  
plt.title('Joy Plot of City and Highway Mileage by Class', fontsize=22)  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图25.png)
                        （图二十五：峰峦图（Joy plot）的示例）

##### 25. 分布式包点图（Distributed dot plot）：
~~~
分布点图显示了被分组分割的点的单变量分布。点的颜色越深，表示该区域的数据点越集中。通过不同的中位数着色，组的真实位置立即变得明显。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
import matplotlib.patches as mpatches  
  
# 准备数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
cyl_colors = {4:'tab:red', 5:'tab:green', 6:'tab:blue', 8:'tab:orange'}  
df_raw['cyl_color'] = df_raw.cyl.map(cyl_colors)  
  
# 根据制造商计算平均和中位城市里程  
df = df_raw[['cty', 'manufacturer']].groupby('manufacturer').mean()  
df.sort_values('cty', ascending=False, inplace=True)  
df.reset_index(inplace=True)  
df_median = df_raw[['cty', 'manufacturer']].groupby('manufacturer').median()  
  
# 绘制水平线  
fig, ax = plt.subplots(figsize=(16,10), dpi= 80)  
ax.hlines(y=df.index, xmin=0, xmax=40, color='gray', alpha=0.5, linewidth=.5, linestyles='dashdot')  
  
# 绘制散点  
for i, make in enumerate(df.manufacturer):  
    df_make = df_raw.loc[df_raw.manufacturer==make, :]  
    ax.scatter(y=np.repeat(i, df_make.shape[0]), x=df_make['cty'], s=75, edgecolors='gray', c='w', alpha=0.5)  
    ax.scatter(y=i, x=df_median.loc[make, 'cty'], s=75, c='firebrick')  
  
# 注释  
ax.text(33, 13, "$Red \; dots \; are \; the \: median$", fontdict={'size':12}, color='firebrick')  
  
# 图形修饰  
red_patch = mpatches.Patch(color='firebrick', label='Median')  
plt.legend(handles=[red_patch])  
ax.set_title('Distribution of City Mileage by Make', fontdict={'size':22})  # 标题仍为英文  
ax.set_xlabel('Miles Per Gallon (City)', alpha=0.7)  
ax.set_yticks(df.index)  
ax.set_yticklabels(df.manufacturer.str.title(), fontdict={'horizontalalignment': 'right'}, alpha=0.7)  
ax.set_xlim(1, 40)  
plt.xticks(alpha=0.7)  
ax.spines["top"].set_visible(False)  
ax.spines["bottom"].set_visible(False)  
ax.spines["right"].set_visible(False)  
ax.spines["left"].set_visible(False)  
plt.grid(axis='both', alpha=.4, linewidth=.1)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图26.png)
                （图二十六：分布式包点图（Distributed dot plot）的示例）

##### 26. 箱线图（Box plot）：
~~~
箱形图是一种可视化分布的好方法，可以记住中位数、第 25、75 个四分位数和异常值。但是，您需要小心解释方框的大小，这可能会扭曲该组中包含的点数。因此，手动提供每个框中的观测值数量可以帮助克服这个缺点。

例如，左侧的前两个箱子的大小相同，尽管它们分别有 5 个和 47 个观测值。因此，在这种情况下，写入该组中的观测数量变得必要。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 绘制图形  
plt.figure(figsize=(13,10), dpi= 80)  
sns.boxplot(x='class', y='hwy', data=df, notch=False, palette='Set2')  
  
# 添加箱线图内的观测值数量（可选）  
def add_n_obs(df,group_col,y):  
    medians_dict = {grp[0]:grp[1][y].median() for grp in df.groupby(group_col)}  
    xticklabels = [x.get_text() for x in plt.gca().get_xticklabels()]  
    n_obs = df.groupby(group_col)[y].size().values  
    for (x, xticklabel), n_ob in zip(enumerate(xticklabels), n_obs):  
        plt.text(x, medians_dict[xticklabel]*1.01, "#obs : "+str(n_ob), horizontalalignment='center', fontdict={'size':14}, color='white')  
  
add_n_obs(df,group_col='class',y='hwy')  
  
# 装饰  
plt.title('Box Plot of Highway Mileage by Vehicle Class', fontsize=22)  
plt.ylim(10, 40)  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图27.png)
                        （图二十七：箱线图（Box plot）的示例）

##### 27. 含抖动数据点的箱线图（Dot ++ box plot）：
~~~
含抖动数据点的箱线图与分组分割的箱线图传达类似的信息。此外，这些点还表示每组中有多少个数据点。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 绘制图形  
plt.figure(figsize=(13,10), dpi= 80)  
sns.boxplot(x='class', y='hwy', data=df, hue='cyl', palette='Set2')  # 添加hue参数以绘制不同气缸数的箱线图  
sns.stripplot(x='class', y='hwy', data=df, color='black', size=3, jitter=1)  
  
for i in range(len(df['class'].unique())-1):  
    plt.vlines(i+.5, 10, 45, linestyles='solid', colors='gray', alpha=0.2)  
  
# 装饰  
plt.title('Box Plot of Highway Mileage by Vehicle Class', fontsize=22)  
plt.legend(title='Cylinders')  # 添加图例标题  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图28.png)
                （图二十八：含抖动数据点的箱线图（Dot ++ box plot）示例）

##### 28. 小提琴图（Violin plot）：
~~~
小提琴图是箱线图的一种视觉上令人愉悦的替代方式。小提琴的形状或面积取决于它所包含的观测数量。然而，小提琴图可能更难阅读，并且在专业环境中并不常用。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 绘制图形  
plt.figure(figsize=(13,10), dpi= 80)  
sns.violinplot(x='class', y='hwy', data=df, scale='width', inner='quartile', palette='Set2')  # 设置palette参数为Set2  
  
# 装饰  
plt.title('Violin Plot of Highway Mileage by Vehicle Class', fontsize=22)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图29.png)
                      （图二十九：小提琴图（Violin plot）的示例）

##### 29. 人口金字塔（Population pyramid）：
~~~
人口金字塔可以用来显示按体积排序的群体分布。或者，它也可以用来显示人口的逐级过滤，如下图所示，显示有多少人通过了营销漏斗的每个阶段。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 读取数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/email_campaign_funnel.csv")  
  
# 绘制图形  
plt.figure(figsize=(3.25, 2.5), dpi= 80)  
group_col = 'Gender'  
order_of_bars = df.Stage.unique()[::-1]  
colors = [plt.cm.Spectral(i/float(len(df[group_col].unique())-1)) for i in range(len(df[group_col].unique()))]  
  
for c, group in zip(colors, df[group_col].unique()):  
    sns.barplot(x='Users', y='Stage', data=df.loc[df[group_col]==group, :], order=order_of_bars, color=c, label=group)  
  
# 装饰  
plt.xlabel("Users")  
plt.ylabel("Stage of Purchase")  
plt.yticks(fontsize=8.8)  
plt.title("Population Pyramid of the Marketing Funnel", fontsize=22)  
plt.legend(title='Gender')  
plt.show()
```
###### · 绘图：
![|700](“Top%2050”图/Top%2050图30.png)
                （图三十：人口金字塔（Population pyramid）的示例）

##### 30. 分类图（Categorical plots）：
~~~
Seaborn 库提供的分类图可以用于可视化两个或多个分类变量之间的计数分布关系。
~~~
###### · 分类图画柱状图绘图代码：
```Python
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 加载数据集  
titanic = sns.load_dataset("titanic")  
  
# 绘图  
g = sns.catplot(x="alive", col="deck", col_wrap=4,  
                data=titanic[titanic.deck.notnull()],  # 只应该有一个data参数  
                kind="count", height=3.5, aspect=.8,  
                palette='tab20')  
  
g.fig.suptitle('sf')  # 设置图的总标题  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图31.png)
                           （图三十一：分类图画柱状图示例）
###### · 分类图画小提琴图绘图代码：
```Python
import seaborn as sns  
import matplotlib.pyplot as plt  
  
# 加载数据集  
titanic = sns.load_dataset("titanic")  
  
# 绘图  
sns.catplot(x="age", y="embark_town",  
            hue="sex", col="class",  
            data=titanic[titanic.embark_town.notnull()],  
            orient="h", height=5, aspect=1, palette="tab10",  
            kind="violin", dodge=True, cut=0, bw=.2)  
  
plt.show()
```
![](“Top%2050”图/Top%2050图32.png)
（图三十二：分类图画小提琴图示例）


### 伍  组成（Composition）

##### 31. 华夫图（Waffle chart）：
~~~
华夫图可以使用 pywaffle 包创建，用于显示更大群体中的群体组成。
~~~
###### · 绘图代码：
```Python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from pywaffle import Waffle

# 导入数据
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")

# 准备数据
df = df_raw['class'].value_counts().reset_index()
df.columns = ['class', 'counts']

# 使用 seaborn 的调色板
palette = sns.color_palette("husl", n_colors=df.shape[0])

# 绘制图表
fig = plt.figure(
    FigureClass=Waffle,
    rows=7,
    values=df['counts'],
    colors=palette,
    labels=list(df['class']),
    legend={'loc': 'upper left', 'bbox_to_anchor': (1, 1), 'fontsize': 12},
    title={'label': '# Vehicles by Class', 'loc': 'center', 'fontsize': 18}
)

plt.show()
```
###### · 绘图：
![](“Top%2050”图/Top%2050图33.png)
（图三十三：华夫图（Waffle chart）的示例）

##### 32. 饼状图（Pie chart）：
~~~
饼状图是显示群体组成的经典方法。然而，现在通常不建议使用它，因为饼部分的面积有时会产生误导。因此，如果您要使用饼状图，强烈建议明确地写下饼状图中每个部分的百分比或数字。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 导入数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 准备数据  
df = df_raw.groupby('class').size()  
  
# 使用 pandas 绘制图表  
df.plot(kind='pie', subplots=True, figsize=(8, 8))  
plt.title("Pie Chart of Vehicle Class - Bad")  
plt.ylabel("")  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图34.png)
                       （图三十四：饼状图（Pie chart）的示例）
###### · 有突出的饼状图绘图代码：
```Python
# 导入必要的库  
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
  
# 从URL读取原始数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 准备数据  
df = df_raw.groupby('class').size().reset_index(name='counts')  
  
# 绘制图表  
fig, ax = plt.subplots(figsize=(12, 7), subplot_kw=dict(aspect="equal"), dpi=80)  
  
data = df['counts']  
categories = df['class']  
explode = [0, 0, 0, 0, 0, 0.1, 0]  
  
# 定义百分比显示函数  
def func(pct, allvals):  
    absolute = int(pct/100. * np.sum(allvals))  
    return "{:.1f}% ({:d})".format(pct, absolute)  
  
# 绘制饼图  
wedges, texts, autotexts = ax.pie(data,  
                                  autopct=lambda pct: func(pct, data),  
                                  textprops=dict(color="w"),  
                                  colors=plt.cm.Dark2.colors,  
                                  startangle=140,  
                                  explode=explode)  
  
# 图表装饰  
ax.legend(wedges, categories, title="Vehicle Class", loc="center left", bbox_to_anchor=(1, 0, 0.5, 1))  
plt.setp(autotexts, size=10, weight=700)  
ax.set_title("Class of Vehicles: Pie Chart")  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图35.png)
                           （图三十五：有突出的饼状图示例）

##### 33. 树状图（Treemap）：
~~~
树状图与饼图有一定的相似之处，但树状图能够更好地展示每个组的贡献，而不会给人造成误导。
~~~
###### · 绘图代码：
```Python
# 导入必要的库  
import pandas as pd  
import matplotlib.pyplot as plt  
import squarify  
  
# 从URL读取原始数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 准备数据  
df = df_raw.groupby('class').size().reset_index(name='counts')  
labels = df.apply(lambda x: str(x[0]) + "\n (" + str(x[1]) + ")", axis=1)  
sizes = df['counts'].values.tolist()  
colors = [plt.cm.Spectral(i/float(len(labels))) for i in range(len(labels))]  
  
# 绘制图表  
plt.figure(figsize=(12,8), dpi=80)  
squarify.plot(sizes=sizes, label=labels, color=colors, alpha=0.8)  
  
# 图表装饰  
plt.title('Treemap of Vehicle Class')  
plt.axis('off')  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图36.png)
                       （图三十六：树状图（Treemap）的示例）

##### 34. 柱状图（Bar chart）：
~~~
条形图是基于计数或任何给定度量来可视化项目的经典方法。下面的图表为每个项目使用了不同的颜色，但您可能通常希望为所有项目选择一种颜色，除非您按组为它们上色。下面的代码将颜色名称存储在 all_colors 中。您可以通过在 plt.plot() 中设置颜色参数来更改条形图的颜色。
~~~
###### · 绘图代码：
```Python
# 导入必要的库  
import pandas as pd  
import matplotlib.pyplot as plt  
import random  
  
# 从URL读取原始数据  
df_raw = pd.read_csv("https://github.com/selva86/datasets/raw/master/mpg_ggplot2.csv")  
  
# 准备数据  
df = df_raw.groupby('manufacturer').size().reset_index(name='counts')  
n = df['manufacturer'].unique().__len__() + 1  
all_colors = list(plt.cm.colors.cnames.keys())  
random.seed(100)  
c = random.choices(all_colors, k=n)  
  
# 绘制条形图  
plt.figure(figsize=(16, 10), dpi=80)  
plt.bar(df['manufacturer'], df['counts'], color=c, width=0.5)  
for i, val in enumerate(df['counts'].values):  
    plt.text(i, val, float(val), horizontalalignment='center', verticalalignment='bottom', fontdict={'fontweight':500, 'size':12})  
  
# 图表装饰  
plt.gca().set_xticklabels(df['manufacturer'], rotation=60, horizontalalignment='right')  
plt.title("Number of Vehicles by Manufacturer", fontsize=22)  
plt.ylabel('Number of Vehicles')  
plt.ylim(0, 45)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图37.png)
                       （图三十七：柱状图（Bar chart）的示例）


### 陆  变化（Change）

##### 35. 时间序列图（Time series plot）：
~~~
时间序列图用于可视化给定度量随时间的变化情况。下例中您可以看到 1949 年到 1969 年间航空客运量的变化。
~~~
###### · 绘图代码：
```Python
# 导入必要的库  
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 从URL导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/AirPassengers.csv')  
  
# 打印数据框的列名  
print(df.columns)  
  
# 绘制线图  
plt.figure(figsize=(16, 10), dpi=80)  
plt.plot('date', 'value', data=df, color='tab:red', label='Traffic')  
  
# 图形修饰  
plt.ylim(50, 750)  
xtick_location = df.index.tolist()[::12]  
xtick_labels = [x[-4:] for x in df.date.tolist()[::12]]  
plt.xticks(ticks=xtick_location, labels=xtick_labels, rotation=0, fontsize=12, horizontalalignment='center', alpha=0.7)  
plt.yticks(fontsize=12, alpha=0.7)  
plt.title("Air Passenger Traffic (1949 - 1969)", fontsize=22)  
plt.grid(axis='both', alpha=0.3)  
  
# 图例  
plt.legend(['Traffic'], fontsize=14)  
  
# 去除边框  
plt.gca().spines["top"].set_alpha(0.0)  
plt.gca().spines["bottom"].set_alpha(0.3)  
plt.gca().spines["right"].set_alpha(0.0)  
plt.gca().spines["left"].set_alpha(0.3)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图38.png)
                   （图三十八：时间序列图（Time series plot）的示例）

##### 36. 带有峰值和谷值注释的时间序列图（Time series with peaks and troughs annotated）：
~~~
下面的时间序列绘制了所有的波峰和波谷，并注释了选定的特殊事件的发生。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
import matplotlib as mpl  
  
# 导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/AirPassengers.csv')  
  
# 获取峰值和谷值  
data = df['value'].values  
doublediff = np.diff(np.sign(np.diff(data)))  
peak_locations = np.where(doublediff == -2)[0] + 1  
  
doublediff2 = np.diff(np.sign(np.diff(-1*data)))  
trough_locations = np.where(doublediff2 == -2)[0] + 1  
  
# 绘图  
plt.figure(figsize=(16,10), dpi= 80)  
plt.plot('date', 'value', data=df, color='tab:blue', label='Air Traffic')  
plt.scatter(df.date[peak_locations], df.value[peak_locations], marker=mpl.markers.CARETUPBASE, color='tab:green', s=100, label='Peaks')  
plt.scatter(df.date[trough_locations], df.value[trough_locations], marker=mpl.markers.CARETDOWNBASE, color='tab:red', s=100, label='Troughs')  
  
# 注释  
for t, p in zip(trough_locations[1::5], peak_locations[::3]):  
    plt.text(df.date[p], df.value[p]+15, df.date[p], horizontalalignment='center', color='darkgreen')  
    plt.text(df.date[t], df.value[t]-35, df.date[t], horizontalalignment='center', color='darkred')  
  
# 装饰  
plt.ylim(50,750)  
xtick_location = df.index.tolist()[::6]  
xtick_labels = df.date.tolist()[::6]  
plt.xticks(ticks=xtick_location, labels=xtick_labels, rotation=90, fontsize=12, alpha=.7)  
plt.title("Peak and Troughs of Air Passengers Traffic (1949 - 1969)", fontsize=22)  
plt.yticks(fontsize=12, alpha=.7)  
  
# 线条颜色调整  
plt.gca().spines["top"].set_alpha(.0)  
plt.gca().spines["bottom"].set_alpha(.3)  
plt.gca().spines["right"].set_alpha(.0)  
plt.gca().spines["left"].set_alpha(.3)  
  
plt.legend(loc='upper left')  
plt.grid(axis='y', alpha=.3)  
plt.show()
```
###### · 绘图：
![|700](“Top%2050”图/Top%2050图39.png)
    （图三十九：带有峰值和谷值注释的时间序列图（Time series with peaks and troughs annotated）的示例）

##### 37. 自相关图和偏自相关图（Autocorrelation (ACF) and partial autocorrelation (PACF) plot）：
~~~
ACF 图显示了时间序列与其自身滞后的相关性。每条竖线（在自相关图上）表示序列与其从滞后 0 开始的滞后之间的相关性。图中蓝色阴影区域是显著性水平。那些位于蓝线以上的滞后是显著滞后。

那么如何解释呢?

对于航空乘客来说，我们看到多达 14 个滞后超过了蓝线，因此很重要。这意味着，14 年前的航空客流量对今天的客流量有影响。

PACF 在另一个屏幕上显示了自动变速器。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf  
  
# 导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/AirPassengers.csv')  
  
# 绘图  
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16,6), dpi=80)  
plot_acf(df.value.tolist(), ax=ax1, lags=50)  # 绘制自相关图  
plot_pacf(df.value.tolist(), ax=ax2, lags=20)  # 绘制偏自相关图  
  
# 装饰  
# 淡化边框  
ax1.spines["top"].set_alpha(.3); ax2.spines["top"].set_alpha(.3)  
ax1.spines["bottom"].set_alpha(.3); ax2.spines["bottom"].set_alpha(.3)  
ax1.spines["right"].set_alpha(.3); ax2.spines["right"].set_alpha(.3)  
ax1.spines["left"].set_alpha(.3); ax2.spines["left"].set_alpha(.3)  
  
# 刻度标签字体大小  
ax1.tick_params(axis='both', labelsize=12)  
ax2.tick_params(axis='both', labelsize=12)  
plt.show()
```
###### · 绘图：
![|700](“Top%2050”图/Top%2050图40.png)
   （图四十：自相关图和偏自相关图（Autocorrelation (ACF) and Partial Autocorrelation (PACF) Plot）的示例）

##### 38. 交叉相关图（Cross correlation plot）：
~~~
交叉相关图显示了两个时间序列之间的滞后。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
import statsmodels.tsa.stattools as stattools  
  
# 导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/mortality.csv')  
x = df['mdeaths']  
y = df['fdeaths']  
  
# 计算交叉相关性  
ccs = stattools.ccf(x, y)[:100]  
nlags = len(ccs)  
  
# 计算显著性水平  
# 参考链接: https://stats.stackexchange.com/questions/3115/cross-correlation-significance-in-r/3128#3128  
conf_level = 2 / np.sqrt(nlags)  
  
# 绘图  
plt.figure(figsize=(12,7), dpi=80)  
  
plt.hlines(0, xmin=0, xmax=100, color='gray')  # 绘制0轴  
plt.hlines(conf_level, xmin=0, xmax=100, color='gray')  
plt.hlines(-conf_level, xmin=0, xmax=100, color='gray')  
  
plt.bar(x=np.arange(len(ccs)), height=ccs, width=.3)  
  
# 装饰  
plt.title('$Cross\; Correlation\; Plot:\; mdeaths\; vs\; fdeaths$', fontsize=22)  
plt.xlim(0,len(ccs))  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图41.png)
                （图四十一：交叉相关图（Cross correlation plot）的示例）

##### 39. 时间序列分解图（Time series decomposition plot）：
~~~
时间序列分解图将时间序列分解为趋势分量、季节分量和残差分量。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
from statsmodels.tsa.seasonal import seasonal_decompose  
from dateutil.parser import parse  
  
# 导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/AirPassengers.csv')  
dates = pd.DatetimeIndex([parse(d).strftime('%Y-%m-01') for d in df['date']])  
df.set_index(dates, inplace=True)  
  
# 分解  
result = seasonal_decompose(df['value'], model='multiplicative')  
  
# 绘图  
fig, axes = plt.subplots(4, 1, figsize=(10,10))  
result.observed.plot(ax=axes[0], legend=False)  
axes[0].set_ylabel('Observed')  
result.trend.plot(ax=axes[1], legend=False)  
axes[1].set_ylabel('Trend')  
result.seasonal.plot(ax=axes[2], legend=False)  
axes[2].set_ylabel('Seasonal')  
result.resid.plot(ax=axes[3], legend=False)  
axes[3].set_ylabel('Residual')  
  
# 调整布局  
plt.tight_layout()  
  
# 添加图例  
plt.legend(['Observed', 'Trend', 'Seasonal', 'Residual'], loc='upper left')  
  
plt.suptitle('Time Series Decomposition of Air Passengers')  
plt.show()```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图42.png)
（图四十二：时间序列分解图（Time series decomposition plot）的示例）

##### 40. 多重时间序列（Multiple time series）：
~~~
您可以在同一图表上绘制测量相同值的多个时间序列，如下所示。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/mortality.csv')  
  
# 定义 Y 轴的上限、下限、间隔和颜色  
y_LL = 100  
y_UL = int(df.iloc[:, 1:].max().max() * 1.1)  
y_interval = 400  
mycolors = ['tab:red', 'tab:blue', 'tab:green', 'tab:orange']  
  
# 绘图和标注  
fig, ax = plt.subplots(1, 1, figsize=(16, 9), dpi=80)  
  
columns = df.columns[1:]  
for i, column in enumerate(columns):  
    plt.plot(df.date.values, df[column].values, lw=1.5, color=mycolors[i])  
    plt.text(df.shape[0]+1, df[column].values[-1], column, fontsize=14, color=mycolors[i])  
  
# 绘制刻度线  
for y in range(y_LL, y_UL, y_interval):  
    plt.hlines(y, xmin=0, xmax=71, colors='black', alpha=0.3, linestyles="--", lw=0.5)  
  
# 装饰  
plt.tick_params(axis="both", which="both", bottom=False, top=False,  
                labelbottom=True, left=False, right=False, labelleft=True)  
  
# 淡化边框  
plt.gca().spines["top"].set_alpha(.3)  
plt.gca().spines["bottom"].set_alpha(.3)  
plt.gca().spines["right"].set_alpha(.3)  
plt.gca().spines["left"].set_alpha(.3)  
  
plt.title('Number of Deaths from Lung Diseases in the UK (1974-1979)', fontsize=22)  
plt.yticks(range(y_LL, y_UL, y_interval), [str(y) for y in range(y_LL, y_UL, y_interval)], fontsize=12)  
plt.xticks(range(0, df.shape[0], 12), df.date.values[::12], horizontalalignment='left', fontsize=12)  
plt.ylim(y_LL, y_UL)  
plt.xlim(-2, 80)  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图43.png)
                （图四十三：多重时间序列（Multiple time series）的示例）

##### 41. 使用次要 y 轴绘制不同刻度的图表（Plotting with different scales using secondary Y axis）：
~~~
如果您要显示在同一时间点测量两个不同数量的两个时间序列，可以在右侧的次 y 轴上绘制第二个序列。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/economics.csv")  
  
x = df['date']  
y1 = df['psavert']  
y2 = df['unemploy']  
  
# 绘制线条1（左Y轴）  
fig, ax1 = plt.subplots(1,1,figsize=(16,9), dpi= 80)  
ax1.plot(x, y1, color='tab:red', label='Personal Savings Rate')  
  
# 绘制线条2（右Y轴）  
ax2 = ax1.twinx()  # 实例化第二个共享相同X轴的坐标轴  
ax2.plot(x, y2, color='tab:blue', label='Unemployed')  
  
# 装饰  
# ax1（左Y轴）  
ax1.set_xlabel('Year', fontsize=20)  
ax1.tick_params(axis='x', rotation=0, labelsize=12)  
ax1.set_ylabel('Personal Savings Rate', color='tab:red', fontsize=20)  
ax1.tick_params(axis='y', rotation=0, labelcolor='tab:red' )  
ax1.grid(alpha=.4)  
  
# ax2（右Y轴）  
ax2.set_ylabel("# Unemployed (1000's)", color='tab:blue', fontsize=20)  
ax2.tick_params(axis='y', labelcolor='tab:blue')  
ax2.set_xticks(np.arange(0, len(x), 60))  
ax2.set_xticklabels(x[::60], rotation=90, fontdict={'fontsize':10})  
ax2.set_title("Personal Savings Rate vs Unemployed: Plotting in Secondary Y Axis", fontsize=22)  
fig.tight_layout()  
plt.show()
```
###### · 绘图：
![|650](“Top%2050”图/Top%2050图44.png)
      （图四十四：使用次要 y 轴绘制不同刻度的图表（Plotting with different scales using secondary Y axis）的示例）

##### 42. 带有误差带的时间序列（Time series with error bands）：
~~~
如果您的时间序列数据集具有每个时间点（日期/时间标记）的多个观测值，则可以构造带有误差带的时间序列。下面您可以看到一些基于一天中不同时间的订单的示例。另一个例子是在 45 天内到达的订单数量。  

在这种方法中，订单数量的平均值用白线表示。在平均值周围计算并绘制 95% 置信区间。
~~~
###### · 绘图代码：
```Python
from scipy.stats import sem  
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/user_orders_hourofday.csv")  
df_mean = df.groupby('order_hour_of_day').quantity.mean()  
df_se = df.groupby('order_hour_of_day').quantity.apply(sem).mul(1.96)  
  
# 绘图  
plt.figure(figsize=(16,10), dpi= 80)  
plt.ylabel("# Orders", fontsize=16)  
x = df_mean.index  
plt.plot(x, df_mean, color="white", lw=2)  
plt.fill_between(x, df_mean - df_se, df_mean + df_se, color="#3F5D7D")  
  
# 装饰  
# 淡化边框  
plt.gca().spines["top"].set_alpha(0)  
plt.gca().spines["bottom"].set_alpha(1)  
plt.gca().spines["right"].set_alpha(0)  
plt.gca().spines["left"].set_alpha(1)  
plt.xticks(x[::2], [str(d) for d in x[::2]] , fontsize=12)  
plt.title("User Orders by Hour of Day (95% confidence)", fontsize=22)  
plt.xlabel("Hour of Day")  
  
s, e = plt.gca().get_xlim()  
plt.xlim(s, e)  
  
# 绘制水平刻度线  
for y in range(8, 20, 2):  
    plt.hlines(y, xmin=s, xmax=e, colors='black', alpha=0.5, linestyles="--", lw=0.5)  
  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图45.png)
        （图四十五：带有误差带的时间序列（Time series with error bands）的失示例）
###### · 绘图代码 2：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
  
# 数据来源：https://www.kaggle.com/olistbr/brazilian-ecommerce#olist_orders_dataset.csv  
# 导入日期解析和标准误差计算所需的库  
from dateutil.parser import parse  
from scipy.stats import sem  
  
# 导入数据  
df_raw = pd.read_csv('https://raw.githubusercontent.com/selva86/datasets/master/orders_45d.csv',  
                     parse_dates=['purchase_time', 'purchase_date'])  
  
# 准备数据：每日平均值和标准误差带  
df_mean = df_raw.groupby('purchase_date').quantity.mean()  
df_se = df_raw.groupby('purchase_date').quantity.apply(sem).mul(1.96)  
  
# 绘图  
plt.figure(figsize=(16,10), dpi= 80)  
plt.ylabel("# Daily Orders", fontsize=16)  
x = [d.date().strftime('%Y-%m-%d') for d in df_mean.index]  
plt.plot(x, df_mean, color="white", lw=2)  
plt.fill_between(x, df_mean - df_se, df_mean + df_se, color="#3F5D7D")  
  
# 修饰  
# 淡化边框  
plt.gca().spines["top"].set_alpha(0)  
plt.gca().spines["bottom"].set_alpha(1)  
plt.gca().spines["right"].set_alpha(0)  
plt.gca().spines["left"].set_alpha(1)  
plt.xticks(x[::6], [str(d) for d in x[::6]] , fontsize=12)  
plt.title("Daily Order Quantity of Brazilian Retail with Error Bands (95% confidence)", fontsize=20)  
  
# 坐标轴限制  
s, e = plt.gca().get_xlim()  
plt.xlim(s, e-2)  
plt.ylim(4, 10)  
  
# 绘制水平刻度线  
for y in range(5, 10, 1):  
    plt.hlines(y, xmin=s, xmax=e, colors='black', alpha=0.5, linestyles="--", lw=0.5)  
  
plt.show()
```
###### · 绘图：
![|550](“Top%2050”图/Top%2050图46.png)
                （图四十六：以周为横轴时间单位的，带有误差带的时间序列）

##### 43. 面积堆叠图（Stacked area chart）：
~~~
堆叠面积图直观地表示了多个时间序列的贡献程度，以便于相互比较。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv('https://raw.githubusercontent.com/selva86/datasets/master/nightvisitors.csv')  
  
# 决定颜色  
mycolors = ['tab:red', 'tab:blue', 'tab:green', 'tab:orange', 'tab:brown', 'tab:grey', 'tab:pink', 'tab:olive']  
  
# 绘制图表并注释  
fig, ax = plt.subplots(1,1,figsize=(16, 9), dpi= 80)  
columns = df.columns[1:]  
labs = columns.values.tolist()  
  
# 准备数据  
x  = df['yearmon'].values.tolist()  
y0 = df[columns[0]].values.tolist()  
y1 = df[columns[1]].values.tolist()  
y2 = df[columns[2]].values.tolist()  
y3 = df[columns[3]].values.tolist()  
y4 = df[columns[4]].values.tolist()  
y5 = df[columns[5]].values.tolist()  
y6 = df[columns[6]].values.tolist()  
y7 = df[columns[7]].values.tolist()  
y = np.vstack([y0, y2, y4, y6, y7, y5, y1, y3])  
  
# 绘制每列数据  
labs = columns.values.tolist()  
ax = plt.gca()  
ax.stackplot(x, y, labels=labs, colors=mycolors, alpha=0.8)  
  
# 修饰  
ax.set_title('Night Visitors in Australian Regions', fontsize=18)  
ax.set(ylim=[0, 100000])  
ax.legend(fontsize=10, ncol=4)  
plt.xticks(x[::5], fontsize=10, horizontalalignment='center')  
plt.yticks(np.arange(10000, 100000, 20000), fontsize=10)  
plt.xlim(x[0], x[-1])  
  
# 淡化边框  
plt.gca().spines["top"].set_alpha(0)  
plt.gca().spines["bottom"].set_alpha(.3)  
plt.gca().spines["right"].set_alpha(0)  
plt.gca().spines["left"].set_alpha(.3)  
  
plt.show()
```
###### · 绘图：
![|700](“Top%2050”图/Top%2050图47.png)
                （图四十七：面积堆叠图（Stacked area chart）的示例）

##### 44. 非堆积的面积图（Area chart unStacked）：
~~~
非堆叠面积图用于可视化两个或多个系列相对于彼此的进展（起伏）。在下面的图表中，你可以清楚地看到，随着失业持续时间的中位数增加，个人储蓄率是如何下降的。非堆叠面积图很好地展示了这一现象。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/economics.csv")  
  
# 准备数据  
x = df['date'].values.tolist()  
y1 = df['psavert'].values.tolist()  
y2 = df['uempmed'].values.tolist()  
mycolors = ['tab:red', 'tab:blue', 'tab:green', 'tab:orange', 'tab:brown', 'tab:grey', 'tab:pink', 'tab:olive']  
columns = ['psavert', 'uempmed']  
  
# 绘图  
fig, ax = plt.subplots(1, 1, figsize=(16,9), dpi= 80)  
ax.fill_between(x, y1=y1, y2=0, label=columns[1], alpha=0.5, color=mycolors[1], linewidth=2)  
ax.fill_between(x, y1=y2, y2=0, label=columns[0], alpha=0.5, color=mycolors[0], linewidth=2)  
  
# 修饰  
ax.set_title('Personal Savings Rate vs Median Duration of Unemployment', fontsize=18)  
ax.set(ylim=[0, 30])  
ax.legend(loc='best', fontsize=12)  
plt.xticks(x[::50], fontsize=10, horizontalalignment='center')  
plt.yticks(np.arange(2.5, 30.0, 2.5), fontsize=10)  
plt.xlim(-10, x[-1])  
  
# 绘制刻度线  
for y in np.arange(2.5, 30.0, 2.5):  
    plt.hlines(y, xmin=0, xmax=len(x), colors='black', alpha=0.3, linestyles="--", lw=0.5)  
  
# 淡化边框  
plt.gca().spines["top"].set_alpha(0)  
plt.gca().spines["bottom"].set_alpha(.3)  
plt.gca().spines["right"].set_alpha(0)  
plt.gca().spines["left"].set_alpha(.3)  
plt.show()
```
###### · 绘图：
![|650](“Top%2050”图/Top%2050图48.png)
              （图四十八：非堆积的面积图（Area chart unStacked）的示例）

##### 45. 日历热力图（Calendar heat map）：
~~~
与时间序列相比，日历地图是可视化基于时间的数据的另一种不太受欢迎的选择。虽然在视觉上很吸引人，但数值并不十分明显。然而，它很好地描绘了极端值和假日效果。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import calmap  
  
# 导入数据  
df = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/yahoo.csv")  
  
# 将日期列转换为日期时间格式  
df['date'] = pd.to_datetime(df['date'])  
  
# 将日期列设置为索引  
df.set_index('date', inplace=True)  
  
# 选择2014年的数据进行绘图  
df_2014 = df['2014-01-01':'2014-12-31']  
  
# 绘图  
plt.figure(figsize=(16,10), dpi= 80)  
calmap.calendarplot(df_2014['VIX.Close'], fig_kws={'figsize': (16,10)}, yearlabel_kws={'color':'black', 'fontsize':14}, subplot_kws={'title':'Yahoo Stock Prices'})  
plt.show()
```
###### · 绘图：
![](“Top%2050”图/Top%2050图49.png)
（图四十九：日历热力图（Calendar heat map）的示例）

##### 46. 季节图（Seasonal Plot）：
~~~
季节图可以用来比较时间序列在前一个季节（年/月/周等）同一天的表现。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
from dateutil.parser import parse  # 导入日期解析模块  
  
# 导入数据  
df = pd.read_csv('https://github.com/selva86/datasets/raw/master/AirPassengers.csv')  
  
# 准备数据  
df['year'] = [parse(d).year for d in df.date]  # 提取年份  
df['month'] = [parse(d).strftime('%b') for d in df.date]  # 提取月份并转换为英文缩写  
years = df['year'].unique()  # 获取唯一年份  
  
# 绘图  
mycolors = ['tab:red', 'tab:blue', 'tab:green', 'tab:orange', 'tab:brown', 'tab:grey', 'tab:pink', 'tab:olive', 'deeppink', 'steelblue', 'firebrick', 'mediumseagreen']  
plt.figure(figsize=(16,10), dpi= 80)  
  
for i, y in enumerate(years):  
    plt.plot('month', 'value', data=df.loc[df.year==y, :], color=mycolors[i], label=y)  
    plt.text(df.loc[df.year==y, :].shape[0]-.9, df.loc[df.year==y, 'value'][-1:].values[0], y, fontsize=12, color=mycolors[i])  
  
# 图表装饰  
plt.ylim(50,750)  
plt.xlim(-0.3, 11)  
plt.ylabel('$Air Traffic$')  # y轴标签  
plt.yticks(fontsize=12, alpha=.7)  
plt.title("Monthly Seasonal Plot: Air Passengers Traffic (1949 - 1969)", fontsize=22)  # 图表标题  
plt.grid(axis='y', alpha=.3)  
  
# 去除边框  
plt.gca().spines["top"].set_alpha(0.0)  
plt.gca().spines["bottom"].set_alpha(0.5)  
plt.gca().spines["right"].set_alpha(0.0)  
plt.gca().spines["left"].set_alpha(0.5)  
  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图50.png)
                      （图五十：季节图（Seasonal Plot）的示例）


### 柒  分组（Groups）

##### 47. 树状图（Dendrogram）：
~~~
树状图根据给定的距离度量将相似的点组合在一起，并根据点的相似度将它们组织成树状链接。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
import scipy.cluster.hierarchy as shc  # 导入层次聚类模块  
  
# 导入数据  
df = pd.read_csv('https://raw.githubusercontent.com/selva86/datasets/master/USArrests.csv')  
  
# 绘图  
plt.figure(figsize=(16, 10), dpi= 80)  
plt.title("USArrests Dendograms", fontsize=22)  
dend = shc.dendrogram(shc.linkage(df[['Murder', 'Assault', 'UrbanPop', 'Rape']], method='ward'), labels=df.State.values, color_threshold=100)  
plt.xticks(fontsize=10)  
plt.show()
```
###### · 绘图：
![|700](“Top%2050”图/Top%2050图51.png)
                   （图五十一：树状图（Dendrogram）的示例）

##### 48. 聚类图（Cluster plot）：
~~~
聚类图可用于划分属于同一聚类的点。下面是一个代表性的例子，根据 usarrested 数据集将美国各州分为5组。这个集群图使用“谋杀”和“攻击”列作为 x 轴和 y 轴。或者，你可以使用第一到主分量作为 x 和 y 轴。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import numpy as np  
import matplotlib.pyplot as plt  
from sklearn.cluster import AgglomerativeClustering  
from scipy.spatial import ConvexHull  
import matplotlib.colors as mcolors  
  
# 导入数据  
df = pd.read_csv('https://raw.githubusercontent.com/selva86/datasets/master/USArrests.csv')  
  
# 层次聚类  
cluster = AgglomerativeClustering(n_clusters=5, linkage='ward')  
cluster_labels = cluster.fit_predict(df[['Murder', 'Assault', 'UrbanPop', 'Rape']])  
  
# 绘图  
plt.figure(figsize=(14, 10), dpi= 80)  
plt.scatter(df.iloc[:,0], df.iloc[:,1], c=cluster_labels, cmap='tab10')  
  
# 绘制多边形  
def encircle(x, y, ax=None, **kw):  
    if not ax:  
        ax = plt.gca()  
    p = np.c_[x, y]  
    hull = ConvexHull(p)  
    poly = plt.Polygon(p[hull.vertices,:], **kw)  
    ax.add_patch(poly)  
  
# 绘制包围多边形  
tableau_colors = list(mcolors.TABLEAU_COLORS.keys())  
for i in range(5):  
    encircle(df.loc[cluster_labels == i, 'Murder'], df.loc[cluster_labels == i, 'Assault'], ec="k", fc=tableau_colors[i], alpha=0.2, linewidth=0)  
  
# 图表装饰  
plt.xlabel('Murder')  
plt.ylabel('Assault')  
plt.title('Agglomerative Clustering of USArrests (5 Groups)', fontsize=22)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图52.png)
                      （图五十二：聚类图（Cluster plot）的示例）

##### 49. 安德鲁斯曲线（Andrews curve）：
~~~
安德鲁斯曲线有助于可视化是否存在基于给定分组的数值特征的固有分组。如果特征（数据集中的列）不能帮助区分组（圈），那么行就不能很好地分离，如下所示。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
from pandas.plotting import andrews_curves  # 导入安德鲁斯曲线绘制函数  
  
# 导入数据  
df = pd.read_csv("https://github.com/selva86/datasets/raw/master/mtcars.csv")  
df.drop(['cars', 'carname'], axis=1, inplace=True)  
  
# 绘图  
plt.figure(figsize=(12,9), dpi= 80)  
andrews_curves(df, 'cyl', colormap='Set1')  # 通过汽缸数来着色  
  
# 使边框变得更轻  
plt.gca().spines["top"].set_alpha(0)  
plt.gca().spines["bottom"].set_alpha(.3)  
plt.gca().spines["right"].set_alpha(0)  
plt.gca().spines["left"].set_alpha(.3)  
  
plt.title('Andrews Curves of mtcars', fontsize=22)  
plt.xlim(-3,3)  
plt.grid(alpha=0.3)  
plt.xticks(fontsize=12)  
plt.yticks(fontsize=12)  
plt.show()
```
###### · 绘图：
![|500](“Top%2050”图/Top%2050图53.png)
                   （图五十三：安德鲁斯曲线（Andrews curve）的示例）

##### 50. 平行坐标（Parallel coordinates）：
~~~
平行坐标有助于可视化一个特征是否有助于有效地分离组。如果存在分离，那么该特征很可能在预测该组时非常有用。
~~~
###### · 绘图代码：
```Python
import pandas as pd  
import matplotlib.pyplot as plt  
from pandas.plotting import parallel_coordinates  # 导入平行坐标绘制函数  
  
# 导入数据  
df_final = pd.read_csv("https://raw.githubusercontent.com/selva86/datasets/master/diamonds_filter.csv")  
  
# 绘图  
plt.figure(figsize=(12,9), dpi= 80)  
parallel_coordinates(df_final, 'cut', colormap='Dark2')  # 通过钻石的切割质量来着色  
  
# 使边框变得更轻  
plt.gca().spines["top"].set_alpha(0)  
plt.gca().spines["bottom"].set_alpha(.3)  
plt.gca().spines["right"].set_alpha(0)  
plt.gca().spines["left"].set_alpha(.3)  
  
plt.title('Parallel Coordinated of Diamonds', fontsize=22)  
plt.grid(alpha=0.3)  
plt.xticks(fontsize=12)  
plt.yticks(fontsize=12)  
plt.show()
```
###### · 绘图：
![|600](“Top%2050”图/Top%2050图54.png)
                 （图五十四：平行坐标（Parallel coordinates）的示例）



~~~
内容整理自：“machine learning+”《 Top 50 matplotlib Visualizations – The Master Plots (with full python code) 》
网址：https://www.machinelearningplus.com/plots/top-50-matplotlib-visualizations-the-master-plots-python/
~~~