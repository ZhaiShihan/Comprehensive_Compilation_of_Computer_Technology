#### 1. 中国地图绘制：

· 例：中国某一年的“疫情最新数据”：
![|625](Python%20地图图/Python%20地图图1.png)
                         （图一：中国某一年的“疫情最新数据”）

###### 绘图代码：
```Python
from pyecharts import options as opts  
from pyecharts.charts import Map  
  
nums = [59989, 1328, 1257, 1172, 1007, 982, 933, 629, 553, 543, 508, 464, 387, 333, 302, 292, 242, 240, 172, 163, 146, 130, 127, 121, 91, 89, 76, 73, 70, 61, 22, 18, 10, 1]  
# 名称需要和地图一一对应，确保名称正确  
provinces = ['湖北省', '广东省', '河南省', '浙江省', '湖南省', '安徽省', '江西省', '江苏省', '重庆市', '山东省', '四川省', '黑龙江省', '北京市', '上海市', '河北省', '福建省', '广西壮族自治区', '陕西省', '云南省', '海南省', '贵州省', '山西省', '天津市', '辽宁省', '甘肃省', '吉林省', '新疆维吾尔自治区', '内蒙古自治区', '宁夏回族自治区', '香港特别行政区', '台湾省', '青海省', '澳门特别行政区', '西藏自治区']  
sequence = [(a, b) for a, b in zip(provinces, nums)]  
  
mymap = Map()  
# 添加数据到地图  
mymap.add(  
    series_name='中国最新疫情地图',  
    data_pair=sequence,  
    maptype='china',  
    label_opts=opts.LabelOpts(is_show=False)  # 不显示省份名称  
)  
mymap.set_global_opts(  
    visualmap_opts=opts.VisualMapOpts(  
        pieces=[  
            {"min": 1, "max": 9, "label": "1-9", "color": '#ADD8E6'},  
            {"min": 10, "max": 99, "label": "10-99", "color": '#87CEEB'},  
            {"min": 100, "max": 999, "label": "100-999", "color": '#00BFFF'},  
            {"min": 1000, "max": 9999, "label": "1000-9999", "color": '#1E90FF'},  
            {"min": 10000, "max": 60000, "label": "≥10000", "color": '#0000FF'}  
        ],  
        is_piecewise=True  
    )  
)  
  
# 渲染为 HTML 文件并保存  
mymap.render('疫情分布图.html')
```

· 注：生成的是一个 html 文件（在对应的路径下）


#### 2. 世界地图绘制：

· 世界一些国家 GDP 的示例数据：
![](Python%20地图图/Python%20地图图2.png)
（图二： 世界一些国家 GDP 的示例数据）

###### 绘图代码：
```Python
from pyecharts import options as opts  
from pyecharts.charts import Map  
  
# 示例数据，包含一些国家和相应的 GDP 数据（单位：亿美元）  
gdp_nums = [21137, 14140, 5157, 3846, 2972, 2827, 2716, 2134, 1975, 1857, 1126, 1027, 962, 868, 830, 725, 689, 605, 549, 537, 514, 503, 494, 487, 455, 432, 414, 391, 369, 357, 349, 338, 329, 324]  
countries = ['United States', 'China', 'Japan', 'Germany', 'India', 'United Kingdom', 'France', 'Italy', 'Brazil', 'Canada', 'South Korea', 'Russia', 'Australia', 'Spain', 'Mexico', 'Indonesia', 'Netherlands', 'Saudi Arabia', 'Turkey', 'Switzerland', 'Taiwan', 'Sweden', 'Poland', 'Belgium', 'Thailand', 'Iran', 'Austria', 'Nigeria', 'United Arab Emirates', 'Israel', 'Argentina', 'Norway', 'Ireland', 'South Africa']  
  
sequence = [(a, b) for a, b in zip(countries, gdp_nums)]  
  
mymap = Map()  
# 添加数据到地图  
mymap.add(  
    series_name='World GDP Map',  
    data_pair=sequence,  
    maptype='world',  
    label_opts=opts.LabelOpts(is_show=False)  # 不显示国家名称  
)  
mymap.set_global_opts(  
    visualmap_opts=opts.VisualMapOpts(  
        pieces=[  
            {"min": 1, "max": 499, "label": "1-499", "color": '#ADD8E6'},  
            {"min": 500, "max": 999, "label": "500-999", "color": '#87CEEB'},  
            {"min": 1000, "max": 2999, "label": "1000-2999", "color": '#00BFFF'},  
            {"min": 3000, "max": 9999, "label": "3000-9999", "color": '#1E90FF'},  
            {"min": 10000, "max": 25000, "label": "≥10000", "color": '#0000FF'}  
        ],  
        is_piecewise=True  
    )  
)  
  
# 渲染为 HTML 文件并保存  
mymap.render('world_gdp_map.html')
```

· 注：生成的是一个 html 文件（在对应的路径下）