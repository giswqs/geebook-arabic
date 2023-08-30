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

(chapter01)=

# إِحتراف تطبيق GEE و geemap في بيئة Python

```{contents}
:local:
:depth: 2
```

+++

## مدخل 
## الفصل الأول:
### 1.1 ما هو  [Google Earth Engine](https://earthengine.google.com)(GEE) ؟  
هي منصــــة معالجه سحابيـــة واســـــعة الانتشــــــار فـــــــــــي مجتـــــمع  الجغرافيـــــة   المكانيـــــة  (Geo-Spatial community).   تعرض GEE  قائمة من البيانات تصل إلى ألاف  من تيرابايت أو مـــــا يســـــــــــمى  ulti-Petabytes من صور الأقمار الاصطنـــــاعية و البيانـــــــات  المكانيــــة و التي تمكن المستخدم من أجراء عمليات تحليل و معالجة و تمثيل نتائج بسهوله و كافأه  و باستخدام   القليل من البرمجــــــة أو دون الحاجة إلى إن تكون ذا معرفه بالبرمجة.

تحتوي GEE  على الكثير من المكتبات و المصادر المفتوحة ذات الأهمية في خلق أدوات و نماذج برمجيه  تخص  تطبيقات الاستشعار عن بعد و نظم المعلومات الجغرافيــــــــــــة. تعتبر geemap  و احده من اهم هذه الدوال و التي لاقت انتشاراً واسعاً منذ إطلاقها في أبريل عام 2020  و التي تم بنائها في لغة بايثون [Earth Engine Python API](https://developers.google.com/earth-engine/guides/python_install)  أعتماداً على مصادر خرائط مجاني و مكتبات مفتوحة المصدر. تمكن geemap المستــــــــــخدم من خلق تطبيقات تحليل تفاعليه (و تسمى interactive map) لها القابلية على التمثيل المرن للبيانـــــــــــات في بيئة jupyter و إجراء تبادل ثنائي الاتجاه بين المستخدم و البيانات المكانية الممثلة على الخرائط أو المرئيات. و يقصد  بـ (  interactive map) هي واجهات تمثيل البيــــــانات المكانيـــــــــــة التي تمكن المستــــــــــــخدم من إجراء عمليات تحليل و استعراض  للخصائص البيانات على مديات مختلفة. كذلك تمكن الخارطة التفاعلية من إجراء عمليات استعلام  (query) عن خصائص محددة و مؤشرات معينة مثل حالة الوضع الاجتماعي و الاقتصادي لمجموعة معينه أو مواقع محطات المســـــــــــاعدة و الدعم لعمليات الإنقاذ عند حدوث الكوارث الطبيعية. 

و تمكن الخارطة التفاعلية (  interactive map) من صناعة و استعراض البيانات و نتائج التحيل لحقول معينه في جدول البيانات, بالإضافة إلى ذلك تمكن المستــــــــــــــخدم من تســــــــــــجيل إحداثيات النقاط على الخارطة لغرض إجراء التحليل المحدد لتلك المنطقة. كما إن هناك العديد من المزايــــــــــا التي توفرها geemap  و التي سوف نستعرضها  في الفصول القادمة.

في هذا الفصل سوف نستعرض أساسيات علوم البيانات المكانية و GEE  و كذلك geemap. كذلك سوف تتعلم الخطوات المطلوبة لعملية أعداد بيئة conda  و تثبيت geemap  في بيئة العمل, و أيضا توظيف geemap  مع Google colab من دون الحاجة إلى تثبيت أي برنامج إضافي أخر. و أخيراً سوف نقترح بعض المصادر القيمة و المفيدة في تعلم GEE  و geemap. 

##  1.2	ما هي علوم البيانات المكانيـــــــــــة؟
قبل البدء بعلوم البيانات المكانيـــــــــــــة  يجب إن نأخذ لحمــــة عامة عن علوم البيانات. لاقى علوم البيانات أهميه كبيرة خلال العقد المنصــــــــرم بما يشتمل عليه  من مصطلحــــــــــــات مثل Deep Learning, Machine Learning , Data analytic, Big data, و طبقاً إلى موقع [Google Trend](https://trends.google.com/trends/explore?q=Data%20Science,Big%20Data,Data%20Analytics,Machine%20Learning&geo=US&date=all#TIMESERIES) فأن اهتمام الباحثين عبر الأنترنت عن علوم البيانات قد ازداد و بشكل منقطع النظير منذ العام 2016. حيث يشير الموقع إلى أن 4.1 مليون عمليه بحث مسجلـــــــــــــــة عن علوم البيانات, و 7.5 مليار عمليــــــة بحث عن Big data, و 2.6 مليار عمليــة بحث عن Machine Learning, و اكثر من مليار عن data analytic. و من المفاجئ إن نســـــــــــــــبة البحث عن Big data  تنـــــــــــــــــــاقص بشــــكل ملحوظ منذ العــــــــام 2016 مع زيادة في العمــــــليات الأخرى (كما في الشكل 1.1 أدناه). 


```{figure} images/ch01_google_trends.jpg
---
name: ch01_google_trends
width: 100%
---
```

تُعَرف علوم البيانات على أنها مصطلح واسع يشملُ عدد من المجالات في مساحة عمل واحدة, و من منظور أعلى يعرف علوم البيانات على انه العلم الذي يختص بدراسة البيانات  [Cao, 2017](https://book.geemap.org/chapters/bibliography.html#id8). من وجهة نظر المطورين يعرف علوم البيانات ذلك حقلُ المعرفة الذي يشمل مساحات متداخلة من تطبيق طرق علمية و عمليات رياضية و خوارزميات و نظم إحصاء لاستخلاص أنماط معرفية و دلالات عميقة من البيانات المنظمة أو غير المنظمة في هيكل معين [Dhar, 2013](https://book.geemap.org/chapters/bibliography.html#id9).

علوم البيانات المكانية (Geospatial Data Science GDS): هو فرع علوم البيانات الذي يختص بدراسة المحتوى المكاني للبيانات. و يعرف على أنه فرع علوم البيانات الذي يجمع النظريات و المفاهيم و التطبيقات التي تختص بالبيانات الجغرافية [Hassan, 2019](https://book.geemap.org/chapters/bibliography.html#id10). مثال على علوم البيانات المكانية (GDS) هو قاعدة بيانات تحليل المعلومات الزمانية و المكانية المقدمة من مؤسسة الإدارة الوطنية للمحيطات و الغلاف الجوي NOAA و التي هي عبارة عن صور أقمار صناعية (راستر) و بيانات مناخ, و أرصاد جوي, و التي تزود نشرة أرصاد البراكين باستخدام التعلم الألي (ML) و النماذج الرياضية [Eftelioglu et al., 2017](https://book.geemap.org/chapters/bibliography.html#id10). 




###	1.3 ما هو Google Earth Engine (GEE)؟ 
هو عبارة عن منصة سحابية تحتوي على كم هائل من البيانات المكانية  و صور الأقمار الصناعية [Gorelick et al., 2017](https://book.geemap.org/chapters/bibliography.html#id3) . خلال السنوات الأخيرة انتشر استخدام GEE بشكل كبير جداً في تطبيقات البيئة و على جميع المستويات حول العالم [Amani et al., 2020, Boothroyd et al., 2020, Tamiminia et al., 2020, Wu et al., 2019](https://book.geemap.org/chapters/bibliography.html#id3). في الشكل أدناه 1.2 يوضح عدد البحوث المنشورة التي تتضمن استخدام GEE.

```{figure} images/ch01_gee_pubs.jpg
---
name: ch01_gee_pubs
width: 100%
---
Google الشكل 1.2 عدد البحوث المنشورة المسجلة في
```

استخدام GEE  يتطلب أن يكون لدى المستخدم حساب على منصة Google. يمكن الدخول إلى GEE من خلال محرر أكواد جافا الخاص بجوجل و الذي يسمى    Google Earth JavaScript Code Editorكما في الشكل 1.3.

```{figure} images/ch01_gee_signup.jpg
---
name: ch01_gee_signup
width: 100%
---
 تسجيل حساب على Earth Engine.
```

###	1.4 ما هو **geemap** ؟
يمكن جوجل أيرث GEE مستخدميه من البرمجة بيئتين برمجيتين هما JavaScript API  و Python API. تعتبر البرمجة في بيئة JavaScript أكفاء بكثير منها في بيئة Python من حيث وظائف تمثيل النتائج و بشكل فعال و ذلك لوجود مصادر متعددة و احتوائها على بيئة تطوير متكاملة تشمل برامج و تطبيقات جاهزة تزيد من قدرة المطورين في هذا المجال على تحرير و بناء و اختبار البيانات المكانية  فضلا عن تصدير الطبقات المتعددة للبيانات. يعتبر ذلك كله نقص في بيئة Python, و لاجل معالة هذه المشكلة تم طرح حرزمه geemap من قبل الدكتور  [2020,Wu](https://book.geemap.org/chapters/bibliography.html#id3) .    لقد تم بناء geemap بالاعتماد على مجموعة من المكتبات مفتوحه المصدر الموجودة مسبقاً في Python مثل [folium](https://python-visualization.github.io/folium), [earthengine-api](https://pypi.org/project/earthengine-api) , [ipyleaflet](https://github.com/jupyter-widgets/ipyleaflet), and [ipywidgets](https://github.com/jupyter-widgets/ipywidgets).  تمكن geemap المستخدم من تحليل و استعراض البيانات المكانية  المتاحة من قبل GEE و بشكل فعال داخل بيئة conda jupyter مع كتابة القليل من البرمجة, كما في الشكل 1.4 أدناه.
```{figure} images/ch01_geemap_gui.jpg
---
name: ch01_geemap_gui
width: 100%
---
 الشكل 1.4 واجهه geemap
```

إن الغرض الرئيسي من geemap هو تمكين المطورين من البرمجة في بيئة Python  لما لها من القدرة في العمل على البيانات, و كذلك الوصول و الاستفادة من الأرشيف الهائل للبيانات في GEE, و أيضاً سرعة تحويل البرامج منJavaScript  إلى Python و بدون الحاجة إلى جهد برمجي كبير. و بالتالي, فأن geemap  تقلل من الوقت المبذول في عملية البرمجة و من خلال واجهه بسيطة و قدرة على تصدير بيانات GEE.


(ch01:install)=

###	1.5 تثبيت geemap:
 تحتوي geemap  على العديد من المكتبات و الحزم الاختيارية و التي تفيد المطورين في مجال علوم البيانات المكانية  من العمل بشكل كفوء في هذا المجال و تقديم نماذج اكثر فهما و تطبيقا للواقع الخارجي, و من امثله هذه المكتبات  [GeoPandas](https://geopandas.org) ,and [localtileserver](https://github.com/banesullivan/localtileserver) و التي في بعض الأحيان يستصعب تنصيبها في بيئة Windows, لذلك ننصح بأتباع الخطوات التالي لتجنب حدوث المشاكل. و لحسن الحظ فأن تثبيت geemap يقوم بشكل اتوماتيكي بتثبيت هذه الدوال بما فيها earthengine-api لذا فأنك غير محتاج إلى إعادة تنصيبها. 

(ch01:conda)=

   1.5.1	تثبيت geemap في بيئة conda: 
لأجل تثبيت geemap ينصح استخدام حزمة Conda و التي يمكن الحصول عليها من الموقع التالي ([conda](https://conda.io/en/latest)  و الذي هو عبارة عن موزع و مترجم Python مع المكتبات الأساسية للعمل على البيانات. كما و يمكن الحصول على           ذلك من خلال تنصيب  [Miniconda](https://docs.conda.io/en/latest/miniconda.html) و الذي هو عباره عن موزع يحتوي على مرتجم Python مع مدير بيئة conda.   
توجد geemap عن طريق قناة Anocanda  التي تسمى [conda-forge](https://anaconda.org/conda-forge/geemap) و التي هي عبارة عن جهد مجاني لمجموعة محترفه تزود حزم دعم لطيف واسع من البرامج. يفضل تنصيب  geemap في بيئة جديدة و نظيفة لأجل تجنب حدوث التعارض مع            حزم الدوال و المكتبات التي تتعامل مع البيانات المكانية  و التي قد تكون موجودة مسبقاً, لذا يستحسن تنصيب geemap في بيئة نظيفة. الخطوات التالية هي لخلق بيئة جديدة و تنصيب متكامل ل geemap.

للبدء بتنصيب geemap في بيئة جديدة أتبع الخطوات التالية:
1-فتح Anaconda Prompt  أو Terminal ثم كتابة “conda create –n gee” ثم الضغط على Enter سوف يتم خلق بيئة جديدة بأسم gee كما في الشكل التالي رقم 1.5 ادناه:

```bash
conda create -n gee python
conda activate gee
conda install -c conda-forge geemap
```

```{figure} images/ch01_conda_create.jpg
---
name: ch01_conda_create
---
gee الشكل 1.5 خلق بيئة جديدة بأسم.
```

-تفعيل بيئة gee  من خلال كتابة الإيعاز التالي “conda activate gee”  ستلاحظ تحول إِمتداد بيئة العمل من (base) إلى (gee).

3-تنصيب geemap في بيئة gee من خلال الإيعاز “conda install –c conda-forge geemap” كما في الشكل التالي (أدراج شكل 1.6).

```{figure} images/ch01_conda_geemap.jpg
---
name: ch01_conda_geemap
---
شكل 1.6 تعفيل بيئة gee و تنصيب geemap
```

كما أسلفنا أن هناك العديد من المكتبات الملحقة بـ geemap مثل GeoPandas, localtileserver, [osmnx](https://github.com/gboeing/osmnx), [rioxarray](https://github.com/corteva/rioxarray) and [rio-cogeo](https://github.com/cogeotiff/rio-cogeo) ,و للمزيد من المعلومات عن المكتبات الملحقة يمكن الدخول إلى الرابط التالي [requirements_all.txt](https://github.com/gee-community/geemap/blob/master/requirements_all.txt)  . لحسن الحظ إن هذه المكتبات متوفرة في حزمة  [pygis](https://pygis.gishub.org/)  و التي بالإمكان تنصيبها في إيعاز واحد. على الرغم من ذلك إلى إن تنصيب geemap يأخذ وقتاً طويلاً لتحميل جميع المكتبات الملحقة, لذا يمكن تنصيب pygis من خلال  حزمه [Mamba](https://github.com/mamba-org/mamba) و التي هي أسرع و أكفاء كما أنها منصة قابلة للتنصيب في بيئات مختلفة.mamba هي عبارة عن حزمة قابله للكتابة و التنفيذ و التي تجعل من تنصيب الحزم الأخرى أسرع, و هي أيضا قابلة للتنصيب في بيئات Windows, Linux, MacOS و متوافقة مع جميع الحزم الأخرى. وتنصب كما في  الإيعازات التالية  في prompt  و كما في الشكل 1.7 ادناه:

```bash
conda install -c conda-forge mamba
mamba install -c conda-forge pygis
```
.

```{figure} images/ch01_install_mamba.jpg
---
name: ch01_install_mamba
---
Fig. 1.7: Installing the Mamba package manager.
```

#### 1.5.2 تنصيب geemap باستخدام **pip** :
عتبر pip عباره عن مدير الحزم الضرورية للموديل الذي يتم بناءه. يمكن تنصيب **geemap** باستخدام **pip**  عن طريق الإيعاز التالي _pip install geemap_ . يُمكن pip من تنصيب جميع الملحقات التابعة ل geemap بشكل منفرد و كما موضح في بعض الأمثلة أدناه:

*_ملاحظة:و حسب شكل الإيعاز التالي pip install geemap[extra]  حيث  يتم وضع اسم المكتبة الملحقة بدل كلمه extra._*

       • pip install geemap[all]
       • pip install geemap[backends]
       • pip install geemap[lidar]
       • pip install geemap[raster]
       • pip install geemap[sql]
       • pip install geemap[apps]
       • pip install geemap[vector]

يمكمن تنصيب geemap من خلال [PyPI](https://pypi.org/project/geemap) و بأستخدام **pip**


```bash
pip install geemap
```

####	1.5.3 تنصيب geemap باستخدام مصدر خارجي:
يمكن تنصيب geemap عن طريق أحد المستودعات الموجودة في [Git](https://git-scm.com) مع pip و حسب الإيعاز التالي:

```bash
git clone https://github.com/gee-community/geemap
cd geemap
pip install .
```
أو عن طريق الإيعاز التالي:
```bash
pip install git+https://github.com/gee-community/geemap
```


و لتنصيب أخر إصدار مباشراً من داخل terminal يمكن استخدام الإيعاز التالي: 

```bash
pip install -U geemap
```

لاجل أجراء عملية تحديث على نسخة geemap يمكن استخدام الايعاز التالي من داخل conda

```bash
conda update -c conda-forge geemap
```

ولأجل تنصيب  أخر نسخه مطوره من داخل jupyter noteobook و بدون Git يمكن تنفيذ الإيعاز التالي مع أعاده تشغيل kernel لأجل تفعيل الإصدار الجديد. 

```{code-cell} ipython3
import geemap

geemap.update_package()
```

#### 1.5.5	تنصيب geemap من خلال Docker
يمكن تنصيب geemap من خلال حاوية [Docker](https://docs.docker.com/get-docker/) ولكن يجب تنصيب Docker أولا, و من ثم تنفيذ الإيعاز التالي من خلال الTerminal الخاص بك:


```bash
docker run -it -p 8888:8888 giswqs/geemap:latest
```

###	1.6 العمل في بيئة **Jupyter notebook**:

العمل في بيئة jupyter notebook : بعد خلق بيئة جديدة في conda يجب تفعيلها و من خلال الإيعاز التالي من داخل Conda Command Prompt 


```bash
conda activate gee
```
بعد الدخول فأن امتداد ملف العمل في الذكرة   (Directory)  على سبيل المثال

'''bash
( cd C:\Users\'-_yourname_-'\Courses_Projects\Geographic_Data_Science
'''

ثم كتابه **JupyterLab**. بعد ذلك سوف يقوم المتصفح الافتراضي بفتح نافذة جديدة تحتوي على بيئة jupyter. عند النقر على **Python3** عند الجهة العليا على اليسار كما في الشكل 1.8 أو من خلال قائمة **File** -> **New** -> **Notebook**

```{figure} images/ch01_jupyterlab.jpg
---
name: ch01_jupyterlab
---
Fig. 1.8: The JupyterLab user interface.
```
يتضمن Jupyter notebook واجهتين هما command mode  و  Edit mode. يختص Edit mode  بكتابة الكود البرمجي على شكل خلايا أشبه بمحرر النصوص, في حين إن command mode يسمح بكتابة الكود البرمجي على شكل رزمه كاملة. كما في الشكل 1.8 أعلاه. هناك العديد من مختصرات لوحة المفاتيح التي تساعد على سرعة إنجاز البرامج في بيئة jupyter و التي بعضها تستخدم في كلا الواجهين و البعض الاخر يستخدم لواجهه معينة.

تستخدم المختصرات التالية في كلا الواجهتين:


- `Shift + Enter`: run the current cell, select below
- `Ctrl + Enter`: run selected cells
- `Alt + Enter`: run the current cell, insert below
- `Ctrl + S`: save and checkpoint

و المختصرات التالية تستخدم في  command mode بعد الضغط على Esc لأجل التفعيل:

- `A`: insert cell above
- `B`: insert cell below
- `X`: cut selected cells
- `C`: copy selected cells
- `V`: paste cells below
- `Y`: change the cell type to Code
- `M`: change the cell type to Markdown
- `P`: open the command palette


و في بيئة Edit يتم استخدام المختصرات التالية مع الضغط على Enter لأجل التفعيل: 

- `Esc`: activate the command mode
- `Tab`: code completion or indent
- `Shift + Tab`: show tooltip

(ch01-ee-auth)=

 1.7 المصادقة على الدخول إلى GEE:

 يتطلب المصادقة على الدخول إلى GEE  الحصول على حساب Google و لا يمكن الوصول إلى البيانات المكانية  في أرشيف GEE إلا بعد تأكيد رابط التفعيل في الأيميل المسجل لفتح الحساب. إن حزمة [earthengine-api](https://pypi.org/project/earthengine-api) هي المسؤولة عن الدخول إلى GEE  و التي يتم تنصيبها بشكل أتوماتيكي مع geemap. و بعد كتابة الإيعازات التالية ثم الضغط على shift + Enter يتم البدء بعملية الحصول على التصريح بالدخول إلى GEE:

```{code-cell} ipython3
import ee

ee.Authenticate()
```

سوف يتم فتح نافذة جديدة في المتصفح تطلب أدخال الحساب الذي تم تسجيله في جوجل, و بعد إدخال الحساب سوف يُطلب من المستخدم تخويل GEE و إجراء عمليه المصادقة, و اذا كانت لأول مره يفضل الدخول إلى CHOOSE PROJECT لتحديد المساحة السحابية المخصصة لتنفيذ البرنامج على GEE كما في الشكل 1.9:

```{figure} images/ch01_generate_token.jpg
---
name: ch01_generate_token
---
Fig. 1.9: Earth Engine Notebook Authenticator.
```


يمكن اختيار مشروع سحابي جديد أو مشروع موجود أصلا في حال اختيار مشروع جديد. أدخل أسم للمشروع مثل (_ee-your-name_) ثم الضغط على تبويب **SELECT** لخلق مشروع جديد. اذا ظهرت لك رسالة تحذير باللون الأحمر أضغط على سياسة الخدمة السحابية **Cloud Terms of Service** للموافقة ثم اختيار SELECT كما في الشكل 1.10 التالي (see {numref}`ch01_create_project`).

```{figure} images/ch01_create_project.jpg
---
name: ch01_create_project
---
Fig. 1.10: Creating a new Cloud Project.
```

بعد تحديد أو خلق مشروع سحابي أضغط على توليد رابط مشفر **GENERATE TOKEN** سوف يتم السؤال عن حساب المستخدم في **EE** للتسجيل في خدمة عملاء **Notebook** و كما في الشكل التالي 1.11  التالي: (see {numref}`ch01_choose_account`).

```{figure} images/ch01_choose_account.jpg
---
name: ch01_choose_account
---
Fig. 1.11: Choosing an account for the Earth Engine Notebook Client.
```

ثم أضغط على زر **Allow**  للسماح لخدمة عملاء **Notebook** للوصول إلى حساب المستخدم في **EE** كما في الشكل 1.12 ادناه:


```{figure} images/ch01_notebook_client.jpg
---
name: ch01_notebook_client
---
Fig. 1.12: Choosing an account for the Earth Engine Notebook Client.
```

سوف يتم فتح صفحة جديدة في المتصفح تحتوي على كود المصادقة, قم بنسخ الكود ثم لصقه في تبويب خلية **Notebook** و التي تَسئلك عن رمز التحقق, ثم أضغط **Enter** وسوف تظهر عبارة Successfully saved authorization token كما في الشكل 1.13. إلى هنا تمت عملية المصادقة على الدخول إلى EE  عن طريق **jupyter Notebook** و الوصول إلى أرشيف **Google** و هذه الخطوات يتم تنفيذها مره واحده فقط. 
{numref}`ch01_auth_code`).

```{figure} images/ch01_auth_code.jpg
---
name: ch01_auth_code
---
Fig. 1.13: Copying the authentication code.
```

إلى هنا تمت عملية المصادقة على الدخول إلى EE  عن طريق jupyter Notebook و الوصول إلى أرشيف Google و هذه الخطوات يتم تنفيذها مره واحده فقط. و للوصول إلى رمز التحقق في ذاكرة الحاسوب فهو في الامتداد التالي  و حسب نوع نظام التشغيل: 



```{code-cell}
Windows: C:\\Users\\USERNAME\\.config\\earthengine\\credentials
Linux: /home/USERNAME/.config/earthengine/credentials
MacOS: /Users/USERNAME/.config/earthengine/credentials
```

بعد اتمام عمليه الحصول على تخويل الوصول الى EE, يمكن خلق فصل جديد في Python من خلال الايعاز التالي:

```{code-cell} ipython3
ee.Initialize()
```

###  1.8	استخدام Google Colab:
اذا كان لديك مشكله في تنصيب geemap في جهازك يمكن استخدام Google colab, و الذي هو عباره عن محرر كود Jupyter سحابي  تم تزويده مجانياً من قبل Google. و من مميزاته انه لا يحتاج إلى تنصيب و يمكن لأي من أعضاء فريق العمل أن يقوم بعميلة الدخول إلى البرنامج و القيام بعملية تحرير كما هو في عمل مشــــــــاركة (Google Doc.) مع الزملاء. من خلال الرابط التالي يمكن تحرير [colab 01_introduction.ipynb ](https://colab.research.google.com/github/giswqs/geebook/blob/master/chapters/01_introduction.ipynb). 

بعد إتمام عمليه التنصيب لـgeemap  يمكن للمستخدم من اطلاق الخارطة التفاعلية من خلال الإيعازات التالية مع ملاحظة ازله # من الايعاز.


```{code-cell} ipython3
# %pip install geemap

import geemap
Map = geemap.Map()
Map
```

د إتمام عمليه الحصول على تخويل الدخول إلى GEE و تنفيذ الإيعازات أعلاه سوف تظهر لك الخارطة التفاعلية كما في اشكل رقم 1.14 أدنا: 
 (see {numref}`ch01_colab`).

```{figure} images/ch01_colab.jpg
---
name: ch01_colab
width: 100%
---
Fig. 1.14: The interactive map displayed in Google Colab.

```


###	1.9 خصائص و مميزات geemap:
 قبل البدء بشرح استخدام geemap بشكل مفصل يجب إن نطلع على بعض مميزاتها للتعرف على مدى أهميه العمل عليها و ما يميزها عن باقي برامج بناء تطبيقات البيانات المكانية.

 •	القدرة على تحويل برنامج JavaScript إلى برنامج Python.  بما في ذلك دعم بعض الدوال الموجدة في JavaScript مثل Map.addLayer(), Map.setCenter(), Map.centerObject(), Map.setOptions(). 
•	الوصول إلى بيانات EE و استـــــــعراضها على شـــــكل طبقات في الخارطة التفاعلية بما في ذلك shapefile, satellite images دون الحاجة إلى كتابة برامج كبيرة.

•	إمكانية الوصول إلى shapefile في EE و استخراج البيانات المكانية داخل الحيز المحدد للمتجه مع إجراء عمليات إحصائية جغرافية و استعراضها بطبقة خاصة.

•	الوصول إلى البيانات المكانية  في كل طبقة من خلال Inspector tool الموجودة في الخارطة, مع القدرة على استخلاص البيانات المكانية  في كل إحداثي من كل طبقة على شكل مصفوفه ذات أبعاد معينة في numpy.

•	تحويل البيانات المتبادل بين GeoJSON و دالة تجميع الخصائص المكانية EE.FeatureCollctioin(). 

•	تصدير البيانات من صيغة تجميع للخصائص المستخدمة في EE.FeatureCollection() إلى صيغ أخرى shp, csv, json, kml, and  kmz . 

•	تصدير مجموعة الصور في ImageCollectoin أو Image إلى صيغة GeoTIFF. 

•	إمكانية خلr نصوص متحركة مع شريط الألوان إلى الصور بصيغة  GIF  و التي يتم توليدها من بيانات EE .

•	القدرة على خلق صور متحركة حسب التسلسل الزمني بمفهوم TimeLapse  لصور الأقمار الصناعية.

•	تمكين المستخدم من توظيف مستكشف السلسلة الزمنية لتمثيل التغيرات عبر الزمن.

•	دعم استيــــــــراد البيـــــــانات المكانيــــــــة  من صيغة GeoTIFF و أرشيــــــــف البينــــــات الزمكانيــــــــه  ( Spatio-Temporal Asset Catalog) المعدلة سحابياً من قبل Google. 

•	القدرة على تحميل البيانات من خادم PostGIS


### 1.10 خلاصة:
علوم البيانات المكانية هو ذلك الحقل الذي يقوم بتطبيق النظريات و الخوارزميات و النماذج الرياضية التي يشتقها ميدان علوم البيانات على المعلومات التي تقترن إحداثيات على سطح الأرض و التي تسمى البينات المكانية. لقد تم تغطيه أساسيات علوم الجغرافيا المكانية و استراض أهمية  Google Earth Engine GEE في عمليه تزويد المستخدمين بأرشيف هائل من البيانات المعدلة و الجاهزة للاستخدام في جميع التطبيقات. كذلك استخدام geemap في colab  و تثبيتها في anaconda و العمل في بيئة jupyter Notebook و ما يميز العمل فيها. في الفصل الأحق سوف نتعمق في استخدام geemap و كيفية بناء بعض التطبيقات. ختام الفصل الاول.

