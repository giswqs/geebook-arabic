---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

(chapter02)=

# خلق خارطة تفاعلية

```{contents}
:local:
:depth: 2
```

## مدخل
هناك العديد من الحزم و المكتبات التي تتعامل مع رسم الخرائط تفاعلية او تحليل المعلومات الجيومكانية {cite}`Wu2021-iv`. على الرغم من ذلك, كل حزمه لها وجه تطبيق(API) خاصه لخلق الخرائط و استعراض البيانات, و التي قد تكون صعبة على المبتدئين للتصفح. ان geemap تبسط العملية من خلال واجهه تطبيقية (API) تجمع بين خلق الخارطه التفاعلية و أستعراض البيانات, و كذلك تمكين المستخدمن من رسم خلفية الخارطة بسطر برمجي واحد. 

من خلال هذا الفصل, سوف تتعرف على كيفية خلق الخارطة التفاعلية بأستخدام واحدة من خمس طرق لرسم خلفيات الخرائط. كذلك, سوف نعطيك بعض الامثلة التطبيقية حول كيفية اضافة و اختيار الخارطة الاساس الى الخارطة من بين المئات من خرائط الاساس الموجوده و باستخدام ايعاز واحد.


(chapter02:requirements)=

## المتطلبات التقنية
للسير في هذا الفصل, يجب ان قمت بتثبيت geemap مع الملحقات الاضافية لها. و اذا قت بأتباع الخطوات التاليه في الشكل 
كما اسلفنا في الفصل الأول فان العمل على الخارطة التفاعلية يحتاج إلى بعض من المكتب التي يجب ان يتم استيرادها للعمل على البيانات المكانية. و تتطلب هذه المكتبات  بعض أدارة لبيئة conda و تثبيت بعض الحزم الضرورية و طلب تصريح العمل على بيانات Google Earth Engine. و يأتي ملخص الخطوات التقنية التي تسبق العمل على الخارطة التفاعلية بما يلي {numref}`ch01:install` - باسم _Installing geemap_, و بالضروره ان تكون قد قمت بعملية بية conda مع الحزم الضرورية. عدى ذلك يمكن خلق بيئة جديدة من conda و تنصيب [pygis](https://pygis.gishub.org) بواسطة الايعازات التالية, و التي تقوم بتنصيب geemap و جميع متطلباتها:

```bash
conda create -n gee python
conda activate gee
conda install -c conda-forge mamba
mamba install -c conda-forge pygis
```
ثانيا, اطلاق Jupyter من خلال الايعاز التالي في terminal او Anaconda prompt:

```bash
jupyter lab
```
او من خلال استخدام geemap في البيئة السحابية Google Colab بدون الحاجة الى تنصيب اي حزم على الكومبيوتر الخاص بيك. أضغط على [02_maps.ipynb](https://colab.research.google.com/github/giswqs/geebook/blob/master/chapters/02_maps.ipynb) لكي يتم اطلاق اطلاق Google Colab.

حال اطلاق Colab, يمكنك رفع التعليق من الايعاز التالي و تنفيذه لكي تتمكن من تنصيب pygis, و التي تتضمن geemap و جميع الملحقات الضمنية فيها:

```{code-cell} ipython3
# %pip install pygis
```

أن عملية التنصيب تأخذ اكثر من 2-3 دقائق. و حال تم تصيب pygis, أضغط على زر **RESTART RUNTIME** الذي يظهر أخر خطوه تنصيب أو قائمة **Runtime** ثم اختيار  **Restart runtime**. بعد ذلك, يمكن كتابة الكود الخاص.

للبدء, يجب استيراد المكتبات الضرورية التي تستخدم في هذا الفصل:


```{code-cell} ipython3
import ee
import geemap
```

تنصيب واجه تطبيق Earth Engine Pyhton:


```{code-cell} ipython3
geemap.ee_initialize()
```
أذا كان هذا اول تنفيذ للايعازات اعلاه, سوف تحتاج الى طلب تصريح دخول الى Earth Engine. الايعازات التالية في  {numref}`ch01-ee-auth` لـطلب _Earth Engine authentication_ لتنصيب Earth Engine.
 

## رسم الخلفيات الاساسية

تحتوي **Geemap** على خمس مكتبات لرسم الخلفية الاساس, و التي هي [ipyleaflet](https://github.com/jupyter-widgets/ipyleaflet), [folium](https://python-visualization.github.io/folium), [plotly](https://plotly.com), [pydeck](https://deckgl.readthedocs.io/en/latest), و [kepler.gl](https://docs.kepler.gl/docs/keplergl-jupyter). أن الخارطة التفاعلية تستخدم واحدة من هذه الطرق لاجل استعراض في بيئة Jupyter مثل Google Colab, Jupyter Notebook و JupyterLab. في الحالة الافتراضية, إن `import geemap` سوف يستخدم ipyleaflet لأجل رسم الخلفية الاساس.

ان هذه المكتبات الخمس الموجوده في geemap تختلف في الوظائف و التطبيقات التي تحتويها. أن مكتبة ipyleaflet تزود افضل الوظائف و اكثرها بما في ذلك واجهه المستخدم التفاعلية (GUI) لتحميل, و تحليل, و تمثيل البيانات المكانية تفاعليا بدون كود. و على سبيل المثال, يستطيع المستخدم اضافة متجه بيانات مثل (e.g., GeoJSON, Shapefile, KML, GeoDataFrame) و بيانات من مرئيات مثل (e.g., GeoTIFF, Cloud Optimized GeoTIFF [COG]) الى الخارطة باسخدام القليل من الازرار. و ايضا ان المستخدم يستطيع ان يقوم بعمليات تحليل للبيانات من المكانية باستخدام WhiteboxTools GUI باستخدام اكثر من 500 ادواع عمليات جغرافية مباشرا من داخل الخارطة التفاعلية. ان الوظائف التفاعلية للعمليات الاخراى ثمل (e.g., split-panel map, linked map, time slider, timeseries inspector) من الممكن استخدامها بشكل مفيد لاجل استعراض و تمثيل البيانات المكانية. ان حزمة ipyleaflet تم بنائها بالاعتماد على ipywidgets و التي تسمح بالتواصل الثنائي بين الواجهه الامامية و الخلفية, و التي تمكن المستخدم من التقاط القيم المدخلة كما {cite}`QuantStack2019-bd`. 


بالمقارنة, أن folium فيها من المحدودية في بعض الوظائف.و التي تقوم بعملية استعراض ثابتة للبيانات. انها مفيدة لتفعيل التطبيقات على الويب و التي ipyleaflet لا تدعم ذلك. ملاحظه ان ذلك ان واجهه المستخدم التفاعلية موجوده في الحزم الاخرى.ان geemap تزود وجهه مميزه تمكن المسخدم من التبديل بين الخلفيات الاساسية بسهوله. و لاختيار الحزمه المطلوبه, بالامكان تنفيذ احد الايعازات التالية:  

- `import geemap.geemap as geemap`
- `import geemap.foliumap as geemap`
- `import geemap.deck as geemap`
- `import geemap.kepler as geemap`
- `import geemap.plotlymap as geemap`

### استخدام حزمه Ipyleaflet
ببساطة يمكنك استخدام `geemap.Map()` لخلق خارطة تفاعلية مع الاعدادات الافتراضية. أولا عليك استيراد مكتبة  `geemap` :

```{code-cell} ipython3
import geemap
```
ثانياَ, خلق خارطة تفاعلية باستخدام دالة رسم الخلفية الاساسية ipyleaflet. ان مكتبة [geemap.Map](https://geemap.org/geemap/#geemap.geemap.Map) تفوم بأستيراد دالة [ipyleaflet.Map](https://ipyleaflet.readthedocs.io/en/latest/map_and_basemaps/map.html). لذلك يمكنك استخدام نفس السياق لخلق خارطة تفاعلية كما ترغب مع الايعاز `ipyleaflet.Map`.


```{code-cell} ipython3
Map = geemap.Map()
```
هذا السطر من الكود  يخلق خارطة جديدة و يسجلها الى متغير جديد اسمه `Map`. لاجل استغراض الخارطه في بيئة Jupyter notebook, ببساطة اكتب الاسم كما في الايعاز التالي:

```{code-cell} ipython3
Map
```
اجعل في ذهنك ان خلال هذا الكتاب,  `Map` بشكل عام يشير الى كائن الخارطه التفاعلية. انها فقط عباره عن اسم متغير لاغير. يمكنك استخدام اي اسم ترغب فيه مثل (e.g., `m`) طالما يتم ترجمته من قبل معالج لغة Python ضمن القواعد لاختيار الاسماء:

Keep in mind that throughout this book, `Map` is commonly used to refer to the interactive map. It is just a variable name. You can use whatever name you want (e.g., `m`) as long as it complies with the following Python variable names rules:

- الاسم يجب ان يبداء بحرف او رمز مسطر
- الاسم يجب ان لا يبداء برقم
- الاسم فقط يحتوي على حروف و شارحة سفلى (A-z, 0-9, و \_ )

بشكل عام, ان المتغيرات في  Python يجب ان تكون في شكل الحروف الصغيرة. و السبب في استخدام `Map` بدلا من `m` ان في Earth Engine JavaScript API يعتبر `Map` اسم محجوز الى الخارطة التفاعلية, و لاجل الحفاط على التوحد مع Earth Engine JavaScript API فان المستخدم بذلك يحافط على الانتفال السهل الى geemap. على أي حال فان المستخدم يتطلب ان يستخدم `Map` كمتغير يشغير الى الخارطة التفاعلية.

لاجل تخصيص الخارطة, بالامكان تخصيص مختلف المدخلات مثل `center` ([lat, lon]), `zoom`, `width` و `height`. ان العرض الافتراضي `width` هو `100%`, و الذي ياخذ كل عرض الخلية في Jupyter. أما الارتفاع `height` يقبل رقم او نص. اذا كان المدخل رقم فانه يمثل ارتفاع الخارطه بوحده قياس البكسل. أما اذا كان المدخل نص فان النص يجب ان يكون بصيغة رقم متبوع بـ `px`, e.g., `600px`. 


```{code-cell} ipython3
Map = geemap.Map(center=[40, -100], zoom=4, height=600)
Map
```
ان الاعدادات الافتراضية للخراطة تأتي مع الظوابط التالية (see {numref}`ch02_ipyleaflet`):

- `attribution_ctrl`
- `data_ctrl`
- `draw_ctrl`
- `fullscreen_ctrl`
- `layer_ctrl`
- `measure_ctrl`
- `scale_ctrl`
- `toolbar_ctrl`
- `zoom_ctrl`

```{figure} images/ch02_ipyleaflet.jpg
---
name: ch02_ipyleaflet
width: 100%
---
A map created using the ipyleaflet plotting backend.
```
لاجل اخفاء اي من الادوات يجب تغير الاعداد الى الحالة السلبية كما في `control_name` الى `False`, `draw_ctrl=False`. 

```{code-cell} ipython3
Map = geemap.Map(data_ctrl=False, toolbar_ctrl=False, draw_ctrl=False)
Map
```
و ايضا بالامكان تخصيص اعداد  `lite_mode=True` لاظهار زر التكبير و التصغير فقط: 

```{code-cell} ipython3
Map = geemap.Map(lite_mode=True)
Map
```
لاجل حفط الخارطة بصيغة HTML يجب تنفيذ الايعاز `save` كما في التالي:


```{code-cell} ipython3
Map.save('ipyleaflet.html')
```


### استخدام حزمة Folium
لكي تحلق خارطة تفاعلية باستخدام حزمه folium لرسم خلفية اساسية ببساطه علىك استخدام المكتبة الخاصة كما في الايعاز التالي:

```{code-cell} ipython3
import geemap.foliumap as geemap
```
بعد ذلك, يمكن استخدام نفس الايعاز اعلاه لخلق و استعراض الخارطة التفاعلية ({numref}`ch02_folium`) حيث فعلت في استخدام بناء حزمه ipyleaflet.

```{code-cell} ipython3
Map = geemap.Map(center=[40, -100], zoom=4, height=600)
Map
```

```{figure} images/ch02_folium.jpg
---
name: ch02_folium
width: 100%
---
A map created using the folium plotting backend.
```
ان folium لا تدعم التواصل الثنائي بين القاعدة الخلفية و الواجهه الامامية {cite}`QuantStack2019-bd`. حال خلق و استعراض الخارطة في خلية notebook, لا يمكن تعديل خصائص الخارطة, مثل اضافة/ازالة, اضافة ازرار, تغيير الارتفاع/العرض. كذلك التحكم بالبيانات و شريط الادوات الموجود في ipyleaflet  في (see {numref}`ch02_ipyleaflet`) ليست مدعومه في folium حيث انها لاتدعم ipywidgets. 


و لاجل حفط الخارطة في HTML يحب استخدام او ستدعاء الايعاز `sav` كما في الشكل الأتي:

```{code-cell} ipython3
Map.save('folium.html')
```

### استخدام حزمة Plotly

لاجل خلق خارطة تفاعلية باستخدام حزمة plotly ببساطة عليك استيراد المكتبة الخاصة كما في الايعاز التالي:

```{code-cell} ipython3
import geemap.plotlymap as geemap
```
بعد ذلك, يمكنك خلق الخارطة التفاعلية  ({numref}`ch02_plotly`) بأستخدام الايعاز التالي. 


```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_plotly.jpg
---
name: ch02_plotly
width: 100%
---
A map created using the plotly plotting backend.
```
ملاحظة لا يمكن قد لا ترى الخارطة عند استعراضعا في Colab حيث انه لا يدعم حزمة plotly FigureWidget. و اذا جاءت رسالة تخبرك `FigureWidget - 'mapbox._derived' Value Error` (see [plotly issue 2570](https://github.com/plotly/plotly.py/issues/2570#issuecomment-738735816)), احذف التعليق من الايعاز التالي ثم قم بتنفيذه.

```{code-cell} ipython3
# geemap.fix_widget_error()
```

### استخدام حزمه Pydeck

اجل خلق خارطة تفاعلية باستخدام حزمة pydeck ببساطة عليك استيراد المكتبة الخاصة كما في الايعاز التالي:



```{code-cell} ipython3
import geemap.deck as geemap
```
بعد ذلك, يمكنك خلق الخارطة التفاعلية  ({numref}`ch02_pydeck`) بأستخدام الايعاز التالي.


```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_pydeck.jpg
---
name: ch02_pydeck
width: 100%
---
A map created using the pydeck plotting backend.
```

### استخدام حزمة KeplerGL
جل خلق خارطة تفاعلية باستخدام حزمة [kepler.gl](https://docs.kepler.gl/docs/keplergl-jupyter) ببساطة عليك استيراد المكتبة الخاصة كما في الايعاز التالي:


```{code-cell} ipython3
import geemap.kepler as geemap
```
بعد ذلك, يمكنك خلق الخارطة التفاعلية  ({numref}`ch02_kepler`) بأستخدام الايعاز التالي.


```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_kepler.jpg
---
name: ch02_kepler
width: 100%
---
A map created using the KeplerGL plotting backend.
```

## أضافة الخارطة التفاعلية Adding basemaps
هناك عدة طرق لاخراج الخارطة الاساسية الى القاعدة الاساس في الخارطة. يمكن تخصيص الخراطة الاساس باستخدام المفتاح الادخال `basemap` عند خلقها مع استخدام طريقة `()geemap.Map`.بديلا عن ذلك, يمكن إضافة طبقة القاعدة الاساس بأسخدام  `()Map.add_basemap`, ان geemap تتضمن المئات من خرائط الاساس الموجوده اساسا من خلال  [xyzservices](https://github.com/geopandas/xyzservices) و التي من اسهل اضافتها الى بأيعاز واحد. 


### Built-in basemaps

Let's try out some of the built-in basemaps. First, import the geemap library as follows:

```{code-cell} ipython3
import geemap
```

Next, create a map by specifying the basemap to use as follows. For example, the `HYBRID` basemap represents the Google Satellite Hybrid basemap ({numref}`ch02_basemap_hybrid`).

```{code-cell} ipython3
Map = geemap.Map(basemap='HYBRID')
Map
```

```{figure} images/ch02_basemap_hybrid.jpg
---
name: ch02_basemap_hybrid
width: 100%
---
The Google Satellite Hybrid basemap.
```

You can add as many basemaps as you like to the map. For example, the following code adds the `OpenTopoMap` basemap to the map above ({numref}`ch02_basemap_opentopomap`):

```{code-cell} ipython3
Map.add_basemap('OpenTopoMap')
```

```{figure} images/ch02_basemap_opentopomap.jpg
---
name: ch02_basemap_opentopomap
width: 100%
---
The OpenTopoMap basemap.

```

To find out the list of available basemaps:

```{code-cell} ipython3
for basemap in geemap.basemaps.keys():
    print(basemap)
```

In total, there are 100+ basemaps available.

```{code-cell} ipython3
len(geemap.basemaps)
```

### XYZ tiles

You can also add XYZ tile layers to the map using the `Map.add_tile_layer()` method. For example, the following code creates an interactive map and adds the Google Terrain basemap to it:

```{code-cell} ipython3
Map = geemap.Map()
Map.add_tile_layer(
    url="https://mt1.google.com/vt/lyrs=p&x={x}&y={y}&z={z}",
    name="Google Terrain",
    attribution="Google",
)
Map
```

### WMS tiles

Similarly, you can add WMS tile layers to the map using the `Map.add_wms_layer()` method. For example, the following code creates an interactive map and adds the National Land Cover Database (NLCD) 2019 basemap to it ({numref}`ch02_basemap_nlcd`):

```{code-cell} ipython3
Map = geemap.Map(center=[40, -100], zoom=4)
url = 'https://www.mrlc.gov/geoserver/mrlc_display/NLCD_2019_Land_Cover_L48/wms?'
Map.add_wms_layer(
    url=url,
    layers='NLCD_2019_Land_Cover_L48',
    name='NLCD 2019',
    format='image/png',
    attribution='MRLC',
    transparent=True,
)
Map
```

```{figure} images/ch02_basemap_nlcd.jpg
---
name: ch02_basemap_nlcd
width: 100%
---
The National Land Cover Database (NLCD) 2019 basemap.
```

Want to find out more freely available WMS basemaps? Check out the [USGS National Map Services](https://apps.nationalmap.gov/services). Once you get a WMS URL, you can follow the example above to add it to the map.

### Planet basemaps

[Planet Labs](https://www.planet.com) provides high-resolution global satellite imagery with a high temporal frequency. The monthly and quarterly global basemaps can be streamed via the [XYZ Basemap Tile Service](https://developers.planet.com/docs/basemaps/tile-services/xyz) for use in web mapping applications or for visualization purposes. A valid [Planet account](https://developers.planet.com/docs/apis/data/#how-to-become-a-planet-developer) is required to access the Basemap Tile Service. Once you sign up for a Planet account, you can get your API key by navigating to the [Account Settings page](https://www.planet.com/account/#/user-settings) as shown in ({numref}`ch02_planet_api_key`).

```{figure} images/ch02_planet_api_key.jpg
---
name: ch02_planet_api_key
width: 100%
---
Planet account profile page.
```

Replace `YOUR_API_KEY` below with the API key copied from your Account Settings page shown above. You can create an environment variable called `PLANET_API_KEY` and set it to the API key so that you can access the basemap tiles without having to specify the API key every time.

```{code-cell} ipython3
import os

os.environ["PLANET_API_KEY"] = "YOUR_API_KEY"
```

First, let's look at the list of available quarterly basemaps. Planet has quarterly basemaps going back to the first quarter of 2016. The names of the quarterly basemaps are: `Planet_2016q1`, `Planet_2016q2`, ..., `Planet_2022q1`, and so on.

```{code-cell} ipython3
quarterly_tiles = geemap.planet_quarterly_tiles()
for tile in quarterly_tiles:
    print(tile)
```

Next, let's look at the list of available monthly basemaps. Planet has monthly basemaps going back to January 2016. The names of the monthly basemaps are: `Planet_2016_01`, `Planet_2016_02`, ..., `Planet_2022_04`, and so on.

```{code-cell} ipython3
monthly_tiles = geemap.planet_monthly_tiles()
for tile in monthly_tiles:
    print(tile)
```

To add a monthly basemap to the map, use the `Map.add_planet_by_month()` method as follows:

```{code-cell} ipython3
Map = geemap.Map()
Map.add_planet_by_month(year=2020, month=8)
Map
```

To add a quarterly basemap to the map, use the `Map.add_planet_by_quarter()` method as follows:

```{code-cell} ipython3
Map = geemap.Map()
Map.add_planet_by_quarter(year=2019, quarter=2)
Map
```

The map should look like {numref}`ch02_basemap_planet`.

```{figure} images/ch02_basemap_planet.jpg
---
name: ch02_basemap_planet
width: 100%
---
The Planet quarterly basemap for second quarter of 2019.

```

### Basemap GUI

Geemap makes it easy for users to add basemaps to their maps without having to write any code. By clicking the "map" icon on the toolbar, the basemap GUI is activated ({numref}`ch02_basemap_gui`). Simply click the dropdown menu to select the basemap that you want to add. Additionally, if you have created an environment variable called `PLANET_API_KEY` and set it to your Planet API key, the Planet basemaps will also be available to select from the dropdown menu.

To set the Planet API key, use the following code:

```{code-cell} ipython3
import os

os.environ["PLANET_API_KEY"] = "YOUR_API_KEY"
```

Then the Planet basemaps will be available via the basemap dropdown menu.

```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_basemap_gui.jpg
---
name: ch02_basemap_gui
width: 100%

---
The basemap GUI for changing basemaps interactively without coding.
```

## Summary

In this chapter, we began by introducing the fundamentals of Jupyter notebooks. We then provided hands-on examples of using the five geemap plotting backends to create interactive maps. Additionally, we explored how to incorporate various basemap layers, such as built-in basemaps, XYZ tiles, WMS tiles, and Planet basemaps, into our maps.

By now, you should be proficient in using Jupyter notebook to generate interactive maps and switch between various basemaps seamlessly. That concludes this chapter. In the next chapter, we will delve into Google Earth Engine data.
