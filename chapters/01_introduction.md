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

# مدخل الى GEE و geemap

```{contents}
:local:
:depth: 2
```

+++

## مدخل
ان [Google Earth Engine](https://earthengine.google.com) هي منصــــة معالجه سحابيـــة واســـــعة الانتشــــــار فـــــــــــي مجتـــــمع  الجغرافيـــــة   المكانيـــــة. تعرض GEE  قائمة من البيانات تصل إلى ألاف  من تيرابايت أو مـــــا يســـــــــــمى  multi-Petabytes من صور الأقمار الاصطنـــــاعية و البيانـــــــات  المكانيــــة و التي تمكن المستخدم من أجراء عمليات تحليل و معالجة و تمثيل نتائج بسهوله و كافأه  و باستخدام   القليل من البرمجــــــة أو دون الحاجة إلى إن تكون ذا معرفه بالبرمجة. لقد تم بناء هذه المنصة على  [Earth Engine Python API](https://developers.google.com/earth-engine/guides/python_install) على الكثير من المكتبات و المصادر المفتوحة و اهمها [geemap](https://geemap.org) ذات الأهمية في خلق أدوات و نماذج برمجيه  تخص  تطبيقات الاستشعار عن بعد و نظم المعلومات الجغرافيــــــــــــة في بية Jupyter. و التي لاقت انتشاراً واسعاً منذ إطلاقها في أبريل عام 2020, حيث اصبحت من أهم المصادر و المنصات في أجراء عمليات تحليل تفاعلية و تمثيل للبيانات في Earth Engine. 

في هذا الفصل سوف نستعرض أساسيات علوم البيانات المكانية و GEE  و كذلك geemap. 
كذلك سوف تتعلم الخطوات المطلوبة لعملية أعداد بيئة conda  و تثبيت geemap  في بيئة العمل, و أيضا توظيف geemap  مع Google colab من دون الحاجة إلى تثبيت أي برنامج إضافي أخر. 
و أخيراً سوف نقترح بعض المصادر القيمة و المفيدة في تعلم GEE  و geemap.  


## ما هي علوم البيانات المكانيـــــــــــة 

قبل البدء بعلوم البيانات المكانيـــــــــــــة  يجب إن نأخذ لحمــــة عامة عن علوم البيانات **data science** . لاقى علوم البيانات أهميه كبيرة خلال العقد المنصــــــــرم بما يشتمل عليه  من مصطلحــــــــــــات مثل  **machine learning** ,**data analytics** ,**big data** . و طبقاً إلى موقع [Google Trends](https://bit.ly/40lfnpW), فأن اهتمام الباحثين عبر الأنترنت عن علوم البيانات **data science** قد ازداد و بشكل منقطع النظير منذ العام 2016 (see {numref}`ch01_google_trends`). 
حيث يشير الموقع إلى أن 4.1 مليون عمليه بحث مسجلـــــــــــــــة عن علوم البيانات "data science", و 7.5 مليار عمليــــــة بحث عن "Big data" و 2.6 مليار عمليــة بحث عن التعلم الألي "Machine Learning", و اكثر من 1.9 مليار عن "data analytic"  و من المفاجئ إن نســـــــــــــــبة البحث عن "Big data"  تنـــــــــــــــــــاقص بشــــكل ملحوظ منذ العــــــــام 2018 مع زيادة في العمــــــليات الأخرى.


```{figure} images/ch01_google_trends.jpg
---
name: ch01_google_trends
width: 100%
---
البحث عبر الانترنت عن علوم البيانات في كوكل لغاية 28-أذار-2028. يمثل المحور العمودي أعلى النقاط في المخطط للبحث عن المسطلحات المذكوره للفتره (2004-2022). القيمة 100 تمثل أعلى انتشار للمسطلح. في حين 50 تمثل نصف الاحصائيات. و 0 تمثل عدم وجود بيانات كافيه للاحصاء.
```

حسناَ, ماهو مصطلح علوم البيانات **data science**؟ تُعَرف علوم البيانات  على أنها مصطلح واسع يشملُ عدد من المجالات في مساحة عمل واحدة, و من منظور أعلى يعرف علوم البيانات على انه العلم الذي يختص{cite}`Cao2017-eb` بدراسة البيانات {cite}`Cao2017-eb` . من وجهة نظر المطورين يعرف علوم البيانات ذلك حقلُ المعرفة الذي يشمل مساحات متداخلة من تطبيق طرق علمية و عمليات رياضية و خوارزميات و نظم إحصاء لاستخلاص أنماط معرفية و دلالات عميقة من البيانات المنظمة أو غير المنظمة في هيكل معين {cite}`Dhar2013-lk.

علوم البيانات الجغرافية-المكانية **Geospatial data science** هو فرع علوم البيانات الذي يختص بدراسة المحتوى المكاني للبيانات. و يعرف على أنه فرع علوم البيانات الذي يجمع النظريات و المفاهيم و التطبيقات التي تختص بالبيانات الجغرافية {cite}`Hassan2019-ub`. مثال على علوم البيانات المكانية (GDS) هو قاعدة بيانات تحليل المعلومات الزمانية و المكانية المقدمة من مؤسسة الإدارة الوطنية للمحيطات و الغلاف الجوي NOAA's و التي هي عبارة عن صور أقمار صناعية (راستر) و بيانات مناخ, و أرصاد جوي, و التي تزود نشرة أرصاد البراكين باستخدام التعلم الألي (ML) و النماذج الرياضية {cite}`Eftelioglu2017-gi`. 


## ماهو Google Earth Engine (GEE)
هو عبارة عن منصة سحابية تحتوي على كم هائل من البيانات المكانية  و صور الأقمار الصناعية  تبلغ عشرات البيتابايت [data catalog](https://developers.google.com/earth-engine/datasets) {cite}`Gorelick2017-mz`. خلال السنوات الأخيرة انتشر استخدام GEE بشكل كبير جداً في تطبيقات البيئة و على جميع المستويات حول العالم في مجتمع المعلومات الجغرافية {cite}`Amani2020-vb,Boothroyd2020-fx,Tamiminia2020-df,Wu2019-at` . منذ العام 2010 أصبح GEE تزايد في عدد البحوث المنشوره فيما يخصها   (see {numref}`ch01_gee_pubs`). استنادا الى التحليل البايومتري, فان 1077 بحث منشور يتضمن كلمه Google Eearth Engine في العنوان الرئيسي و 2969 بحث يتضمنها في العنوان الرئيسي و الخلاصة. في العام 2022, ان عدد البحوث المنشوره التي تتضمن كلمه ”Google Earth Engine“ في العنوان و الخلاصة قد بلغ 1150, و التي هي 280 ضعف عن العام 2014 و الذي كان 4 بحوث في ذلك العام. 

```{figure} images/ch01_gee_pubs.jpg
---
name: ch01_gee_pubs
width: 100%
---
Google Earth Engine  عدد البحوث المنشوره في
```
لكي تتمكن من إستخدام منصة Earth Engine, يجب التسجيل حساب و عبر الرابط [sign up for an Earth Engine account](https://code.earthengine.google.com/register) ({numref}`ch01_gee_signup`). لا يمكن استخدام Google ما لم تتم الموافقة على الطلب. و حال إستلامك بريد ألكتروني بالموافقة يمكنك الدخو الى منصة محرر كود جافا سكربت [Earth Engine JavaScript Code Editor](https://code.earthengine.google.com) و لكي تتعرف على JavaScript API.

```{figure} images/ch01_gee_signup.jpg
---
name: ch01_gee_signup
width: 100%
---
Earth Engine تسجيل حساب على 
```

## ما هو geemap
يمكن GEE مستخدميه من البرمجة بيئتين برمجيتين هما JavaScript API  و Python API. تعتبر البرمجة في بيئة  JavaScript API أكفاء بكثير منها في بيئة Python من حيث وظائف تمثيل النتائج و بشكل فعال و ذلك لوجود مصادر متعددة [documentation](https://developers.google.com/earth-engine)  و أيضاَ احتوائها على بيئة تطويــر (IDE)  (i.e., [GEE JavaScript Code Editor](https://code.earthengine.google.com)) متكاملة تفاعلية, و الذي يعتبر ذلك كله نقص في بيئة Python من حيث القدرة على أستعراض و تمثيل النتائج. و لأجل معالجة هذه المشكلة تم طرح حزمة **geemap**, حسب {cite}`Wu2020-br`. لقد تم بناء **geemap** بالاعتماد على مجموعة من الحزم الموجودة مسبقاً  مثل [earthengine-api](https://pypi.org/project/earthengine-api), [folium](https://python-visualization.github.io/folium), [ipyleaflet](https://github.com/jupyter-widgets/ipyleaflet) و [ipywidgets](https://github.com/jupyter-widgets/ipywidgets).  تمكن **geemap** المستخدم من تحليل و استعراض البيانات المكانية  المتاحة من قبل GEE و بشكل فعال داخل بيئة jupyter مع كتابة القليل من البرمجة (see {numref}`ch01_geemap_gui`).

تمكن **Geemap** الباحثين و الطلاب الراغبين في توظيف تنوع المكتبات و الادوات في Python من إستعراض و تمثيل البيانات المتاحة في Google Earth Engine. و كذلك تمكن من تحويل و ترجمة برامج JavaScript API الى Pyhton API. و   تزود **geemap** و اجهه مستخدم تفاعلية لاتمام عملية التحويل من JavaScript الى Python بدون كتابة اي اكواد. و التي تحفظ للمستخدم الكثير من الوقت والجهد من خلال تزويد واجهه بسيطة لاستعراض وتمثيل البيانات من Earth Engine. 


```{figure} images/ch01_geemap_gui.jpg
---
name: ch01_geemap_gui
width: 100%
---
 ipywidgets  التي تم بنائها الى مكتبة Geemap واجهة
```

(ch01:install)=

## تثبيت geemap
تحتوي geemap  على العديد من المكتبات و الحزم الاختيارية مثل [GeoPandas](https://geopandas.org) و [localtileserver](https://github.com/banesullivan/localtileserver). و التي في بعض الأحيان يستصعب تنصيبها في بيئة Windows, لذلك ننصح بأتباع الخطوات التالي لتجنب حدوث المشاكل. و لحسن الحظ فأن تثبيت geemap يقوم بشكل اتوماتيكي بتثبيت هذه الدوال بما فيها earthengine-api لذا فأنك غير محتاج إلى إعادة تنصيبها. 

(ch01:conda)=

### التثبيت في conda

لأجل تثبيت geemap ينصح استخدام حزمة و مدير بيئة [conda](https://conda.io/en/latest). و  التي يمكن الحصول عليها من الموقع التالي [Anaconda Distribution](https://www.anaconda.com/products/distribution)   (و الذي هو عبارة عن موزع و مترجم Python مع المكتبات الأساسية للعمل على البيانات), أو كما و يمكن الحصول على ذلك من خلال تنصيب  [Miniconda](https://docs.conda.io/en/latest/miniconda.html) (و الذي هو عباره عن موزع يحتوي على مرتجم Python مع مدير بيئة conda). يمكن الاطلاع على شرح في الموقع [installation docs](https://conda.io/docs/user-guide/install/download.html) حول المزيد من ذلك. 
توجد geemap عن طريق قناة Anocanda  التي تسمى [conda-forge](https://anaconda.org/conda-forge/geemap). و التي هي عبارة عن جهد مجاني لمجموعة محترفه تزود حزم دعم لطيف واسع من البرامج. يفضل تنصيب  geemap في بيئة جديدة و نظيفة لأجل تجنب حدوث التعارض مع حزم الدوال و المكتبات التي تتعامل مع البيانات المكانية  و التي قد تكون موجودة مسبقاً, لذا يستحسن تنصيب geemap في بيئة نظيفة. الخطوات التالية هي لخلق بيئة جديدة و تنصيب متكامل ل geemap.


أن قناة Anaconda عبارة عن جهد لمجموعة محترفة قدمت هذه الحزمه للاستخدام في طيف واسع من البرامج. أن تنصيب **geemap** في بيئة جديدة ليس ضرورياَ, و لكن قد يتعارض مع النسخ الاخرى التي تحتوي على حزم تتعامل مع البيانات الجيومكانية, لذل يفضل تنصيبها في بيئة جديدة لاجل تجنب هذا التعارض. الايعاز التالي يقوم بخلق بيئة جديدة في conda بأسم `gee` و تنصيب geemap:


```bash
conda create -n gee python
conda activate gee
conda install -c conda-forge geemap
```
أولا, فتح **Anaconda Prompt**  أو **Terminal** ثم كتابة “conda create –n gee” ثم الضغط على **Enter** سوف يتم خلق بيئة جديدة بأسم  `gee`                                                           
 (see {numref}`ch01_conda_create`).

```{figure} images/ch01_conda_create.jpg
---
name: ch01_conda_create
---
Creating a new conda environment named `gee`.
```
ثانيا, تفعيل بيئة conda  من خلال كتابة الإيعاز التالي "conda activate gee". بعد ذلك قم بتنصيب geemap في هذه البيئة من خلال كتابة الايعاز  "conda install -c conda-forge geemap".  و كما في الشكل (see {numref}`ch01_conda_geemap`)               
 .

```{figure} images/ch01_conda_geemap.jpg
---
name: ch01_conda_geemap
---
Activating the new conda environment and installing geemap.
```

كما أسلفنا أن هناك العديد من المكتبات الملحقة كما في [requirements_all.txt](https://github.com/gee-community/geemap/blob/master/requirements_all.txt), مثل  GeoPandas, localtileserver, [osmnx](https://github.com/gboeing/osmnx), [rioxarray](https://github.com/corteva/rioxarray) و [rio-cogeo](https://github.com/cogeotiff/rio-cogeo). لحسن الحظ إن هذه المكتبات متوفرة في حزمة   [pygis](https://pygis.gishub.org)  و التي بالإمكان تنصيبها في إيعاز واحد. 

على الرغم من ذلك إلى إن تنصيبها يأخذ وقتاً طويلاً. لتحميل جميع المكتبات الملحقة, لذا ننصح تنصيبها من خلال  حزمه [Mamba](https://github.com/mamba-org/mamba) و التي هي أسرع و أكفاء,و كما أنها منصة قابلة للتنصيب في بيئات مختلفة. Mamba هي عبارة عن حزمة قابله للكتابة و التنفيذ و التي تجعل من تنصيب الحزم الأخرى أسرع, و هي أيضا قابلة للتنصيب في بيئات Windows, Linux, MacOS و متوافقة مع جميع الحزم الأخرى. و الايعازات التالية لتنصيب Mamba و pygis:

```bash
conda install -c conda-forge mamba
mamba install -c conda-forge pygis
```
لاجل تنصيب Mamba, قم بكتابة الايعاز التالي "conda install -c conda-forge mamba", ثم الضغط على **Enter** و كما في الشكل التالي  (see {numref}`ch01_install_mamba`).

```{figure} images/ch01_install_mamba.jpg
---
name: ch01_install_mamba
---
Mamba تنصيب مدير الحزم .
```
في حال تم تنصيب Mamba في بيئة conda, يمكنك ببساطة تبديل اي `conda` بالايعاز `mamba`. و على سبيل المثال, لاجل تنصيب pygis أكتب الايعاز التالي "mamba install -c conda-forge pygis" ثم أضغط على **Enter** و كما في الشكل التالي  (see {numref}`ch01_install_pygis`).

```{figure} images/ch01_install_pygis.jpg
---
name: ch01_install_pygis
---
  Installing optional dependencies of geemap through the pygis package.
```
مبروك, الى هنا تمت عملية تنصيب geemap مع كامل الملحقات. في الفصول القادمه سوف نشرح كيفيه استخدام geemap في التطبيقات المختلفة.


### تثبيت باستخدام pip
أن geemap كذلك موجوده في [PyPI](https://pypi.org/project/geemap). و التي يمكن تنصيبها من خلال الايعاز التالي:



```bash
pip install geemap
```
جميع الملحقات الضروريه في geemap موجوده في القائمة [requirements_all.txt](https://github.com/gee-community/geemap/blob/master/requirements_all.txt) و التي يمكن تنصيبها من خلال واحده من الايعازات التالية:


- `pip install geemap[extra]`: requirements_extra.txt تنصيب اختيارات اخرى من القائمه .
- `pip install geemap[all]`: requirements_all.txt تنصيب جميع الاختيارات من القائمة .
- `pip install geemap[backends]`: keplergl, pydeck, and plotly  تنصيب مكتبتي.
- `pip install geemap[lidar]`: ipygany, ipyvtklink, laspy, panel, pyntcloud[LAS], pyvista, pyvista-xarray, and rioxarray تنصيب المكتبات.
- `pip install geemap[raster]`:  geedim, localtileserver, rio-cogeo, rioxarray, netcdf4, and pyvista-xarray  تنصيب المكتبات.
- `pip install geemap[sql]`:  psycopg2 and sqlalchemy  تنصيب مكتبتي.
- `pip install geemap[apps]`: gradio, streamlit-folium, and voila تنصيب مكتبات.
- `pip install geemap[vector]`: geopandas and osmnx  تنصيب مكتبتي.

### تثبيت من مصادر أخرى
يمكن تنصيب geemap عن طريق أحد المستودعات الموجودة في GitHub مع [Git](https://git-scm.com)  و استخدام pip و حسب الإيعاز التالي:


```bash
git clone https://github.com/gee-community/geemap
cd geemap
pip install .
```
و يمكن تنصيب أخر اصدار مباشراَ من خلال مستودع GitHub و كما في الايعاز التالي:


```bash
pip install git+https://github.com/gee-community/geemap
```

### تحديث geemap
اذا تمت عملية التنصيب ل geemap و كنت ترغب في تحديث او الانتقال الى النسخه الاعلى, يمكن استخدام الايعاز التالي في terminal:


```bash
pip install -U geemap
```
و اذا كنت تستخدم conda, يمكنك اجراء عملية التحديث من خلال الايعاز التالي و من داخل terminal:

```bash
conda update -c conda-forge geemap
```
ولأجل تنصيب  أخر نسخه مطوره من GitHub و من داخل jupyter noteobook و بدون Git يمكن تنفيذ الإيعاز التالي في Jupyter notebook مع أعاده تشغيل kernel لأجل تفعيل الإصدار الجديد. 


```{code-cell} ipython3
import geemap

geemap.update_package()
```

### تثبيت باستخدام Docker

ان geemap موجوده ايضا في [Docker Hub](https://hub.docker.com/r/giswqs/geemap).

لاجل استخدام geemap في حاويه Docker,  يجب ان تقوم بعملية تنصيب لحاويه [Docker](https://docs.docker.com/get-docker).و حال تنصيب Docker, يمكن ان يمكن تنصيب اخر اصدار من geemap من خلال حاوية Docker, و من ثم تنفيذ الإيعاز التالي:


```bash
docker run -it -p 8888:8888 giswqs/geemap:latest
```

## كيفية خلق Jupyter notebook
اولا علينا تفعيل البيئة الجديده التي تم خلقها في conda و من خلال الايعاز التالي:

```bash
conda activate gee
```
ثانيا, اطلاق Jupyter من خلال كتابة الايعاز التالي في **Terminal** or **Anaconda Prompt**:

```bash
jupyter lab
```
ان Jupyter سوف يقوم بفتح صفحه جديده في متصفح الانترنت الخاص بجهازك. أضغط ايقونه **Python 3** على الجهه العاليا من الزاويه اليسرى لمنصه  JupyterLab **Launcher** يمكنك ملاحظه ذلك في (see {numref}`ch01_jupyterlab`)  أو الذهاب الى  **File -> New -> Notebook** لاجل ان تخلق صفحه ملاحظات notebook جديدة. اختر صفحة الملاحظات الجديده في JupyterLab File Browser ثم قم بالضغط على **F2** لاجل أعادة التسميه للصفحة, مثل **chapter01.ipynb**. 


```{figure} images/ch01_jupyterlab.jpg
---
name: ch01_jupyterlab
---
The JupyterLab user interface.
```
يتضمن Jupyter notebook واجهتين هما **Edit mode**  و  **Command mode**. يختص Edit mode  بكتابة الكود البرمجي على شكل خلايا أشبه بمحرر النصوص, في حين إن command mode يسمح بكتابة الكود البرمجي على شكل رزمه كاملة و لكن لا يمكن كتابة الكود البرمجي بشكل خلايا منفصلة. يحتوي Jupyter notebook على عدد من مختصرات لوحه المفاتيح {cite}`Yordanov2017-hl`. في ادناه مجموعة من هذه المختصرات. مع ملاحظه انها لمستخدمي Windows و Linux. و لمستخدمي Mac, استبدل `Ctrl` ب `Command`. 

 

مختصارت في كلا الواجهتين :

- `Shift + Enter`: تنفيذ الخلية الحالية, و الانتقال الى الاخرى السفلى
- `Ctrl + Enter`:تنفيذ الخلية المختارها او الحالية 
- `Alt + Enter`: تنفيذ الخلية الحالية, و ادخال خلية جديدة 
- `Ctrl + S`: حفظ و تدقيق

في بيئة command  يجب عليك (press `Esc` to activate):

- `A`: ادخال خلية الى الاعلى
- `B`: ادخال خلية الى الاسفل
- `X`: استقطاع الخلية المختارة
- `C`: نسخ الخلية
- `V`: لصق الخلية
- `Y`: تغيير الخلية الى كود
- `M`: تغيير الخلية الى نص
- `P`: فتح لوحة الايعازات

في حين في بيئة Edit يجب  (press `Enter` to activate):

- `Esc`: تفعيل command mode
- `Tab`: الانتهاء من الايعاز او المسافة البادئة
- `Shift + Tab`: لا ظهار tooltip

(ch01-ee-auth)=

## Earth Engine authentication

You need to authenticate Earth Engine before using it. The package for the Earth Engine Python API is called [earthengine-api](https://pypi.org/project/earthengine-api), which should have been automatically installed by the geemap package as described in {numref}`ch01:install`. Enter the following script into a code cell of a Jupyter notebook and press `Shift + Enter` to execute:

```{code-cell} ipython3
import ee

ee.Authenticate()
```

After running the above script, a new tab will open in the browser asking you to sign in to your Earth Engine account. After signing in, you will be asked to authorize the Google Earth Engine Authenticator. If this is the first time you are authenticating Earth Engine, click **CHOOSE PROJECT** to select a Cloud Project to use for Earth Engine (see {numref}`ch01_generate_token`).

```{figure} images/ch01_generate_token.jpg
---
name: ch01_generate_token
---
Earth Engine Notebook Authenticator.
```

You can either choose an existing Cloud Project or create a new one. If you choose to create a new Cloud Project, enter a project name, e.g., `ee-your-username` and click the blue **SELECT** button to create a new Cloud Project. If a red warning message appears at the bottom of the page, click on the **Cloud Terms of Service** link to accept the terms of service and then click the **SELECT** button again (see {numref}`ch01_create_project`).

```{figure} images/ch01_create_project.jpg
---
name: ch01_create_project
---
Creating a new Cloud Project.
```

After selecting a Cloud Project, click the **GENERATE TOKEN** button to generate a new token. You will be asked to choose your Earth Engine account for the Notebook Client (see {numref}`ch01_choose_account`).

```{figure} images/ch01_choose_account.jpg
---
name: ch01_choose_account
---
Choosing an account for the Earth Engine Notebook Client.
```

Click the **Allow** button to allow the Notebook Client to access your Earth Engine account (see {numref}`ch01_notebook_client`).

```{figure} images/ch01_notebook_client.jpg
---
name: ch01_notebook_client
---
Choosing an account for the Earth Engine Notebook Client.
```

An authentication code will be generated and displayed on the page. Copy the authorization code and paste it into the notebook cell asking for the verification code. Press **Enter** and the `Successfully saved authorization token` message should appear beneath the authorization code you entered (see {numref}`ch01_auth_code`).

```{figure} images/ch01_auth_code.jpg
---
name: ch01_auth_code
---
Copying the authentication code.
```

Congratulations! You have successfully authenticated Earth Engine for use in your Jupyter notebook. In general, authentication for local installations is a one-time step that generates a persistent authorization token stored on a local computer. The token can be found in the following file path depending on your operating system. Note that you might need to show the hidden directories on your computer in order to see the `.config` folder under the home directory.

```{code-cell}
Windows: C:\\Users\\USERNAME\\.config\\earthengine\\credentials
Linux: /home/USERNAME/.config/earthengine/credentials
MacOS: /Users/USERNAME/.config/earthengine/credentials
```

Once Earth Engine is authenticated, you can run the following script to initialize Earth Engine for a new Python session.

```{code-cell} ipython3
ee.Initialize()
```

In general, you will need to initialize Earth Engine for each new Python session, i.e., whenever you open a Jupyter notebook or Python script and want to use Earth Engine. Fortunately, geemap can automatically initialize Earth Engine for you when creating an interactive map, which will be covered in the next chapter. In other words, you rarely need to run `ee.Initialize()` explicitly.

## Using Google Colab

If you have difficulties installing geemap on your computer, you can try out geemap with [Google Colab](https://colab.research.google.com) without installing anything on your machine. Google Colab is a free Jupyter notebook environment that runs entirely in the cloud. Most importantly, it does not require a setup and the notebooks that you create can be simultaneously edited by your team members - just like the way you edit documents in Google Docs!

Click [01_introduction.ipynb](https://colab.research.google.com/github/giswqs/geebook/blob/master/chapters/01_introduction.ipynb) to launch the notebook in Google Colab.

Next, press **Ctrl + /** to uncomment the following line to install geemap:

```{code-cell} ipython3
# %pip install geemap
```

After geemap has been installed successfully, type the following code in a new cell:

```{code-cell} ipython3
import geemap

Map = geemap.Map()
Map
```

Follow the on-screen instructions to authenticate Earth Engine. After that, you should be able to see the interactive map displayed beneath the code cell (see {numref}`ch01_colab`).

```{figure} images/ch01_colab.jpg
---
name: ch01_colab
width: 100%
---
The interactive map displayed in Google Colab.

```

## Using geemap with a VPN

When using geemap through a VPN, it's important to use "geemap.set_proxy(port=your-port-number)" to connect to Earth Engine servers ({numref}`ch01_vpn_proxy`). Failure to do so may result in a connection timeout issue.

```{code-cell} ipython3
import geemap

geemap.set_proxy(port='your-port-number')
Map = geemap.Map()
Map
```

```{figure} images/ch01_vpn_proxy.jpg
---
name: ch01_vpn_proxy
---
Using geemap with a VPN.
```

## Key features of geemap

Below is a partial list of geemap features. Please check the geemap [API Reference](https://geemap.org/geemap) and [tutorials](https://geemap.org/tutorials) for more details.

- Convert Earth Engine JavaScript projects to Python scripts and Jupyter notebooks.
- Display Earth Engine data layers on interactive maps.
- Support Earth Engine JavaScript API-styled functions in Python, such as Map.addLayer(), Map.setCenter(), Map.centerObject(), Map.setOptions().
- Visualize Earth Engine vector and raster data without coding.
- Retrieve Earth Engine data interactively using the Inspector tool.
- Creating interactive plots from Earth Engine data by simply clicking on the map.
- Convert data between the GeoJSON and Earth Engine FeatureCollection formats.
- Use drawing tools to interact with Earth Engine data.
- Use shapefiles with Earth Engine without having to upload data to one's GEE account.
- Export data in the Earth Engine FeatureCollection format to other formats (i.e., shp, csv, json, kml, kmz).
- Export Earth Engine Image and ImageCollection as GeoTIFF.
- Extract pixels from an Earth Engine Image into a 3D numpy array.
- Calculate zonal statistics by group.
- Add a custom legend for Earth Engine data.
- Convert Earth Engine JavaScript projects to Python code from directly within a Jupyter notebook.
- Add animated text to GIF images generated from Earth Engine data.
- Add colorbar and images to GIF animations generated from Earth Engine data.
- Create satellite timelapse animations with animated text using Earth Engine.
- Search places and datasets from Earth Engine Data Catalog.
- Use the timeseries inspector to visualize landscape changes over time.
- Export Earth Engine maps as HTML files and PNG images.
- Search Earth Engine API documentation within Jupyter notebooks.
- Import Earth Engine assets from personal Earth Engine accounts.
- Publish interactive GEE maps directly within a Jupyter notebook.
- Add local raster datasets (e.g., GeoTIFF) to the map.
- Support Cloud Optimized GeoTIFF (COG) and SpatioTemporal Asset Catalog (STAC).
- Perform image classification and accuracy assessment.
- Extract pixel values interactively and export data as shapefile and CSV.
- Visualize land cover change with Sankey diagrams.
- Load vector data from a PostGIS server.
- Create publication-quality maps with cartoee.

## Summary

In this chapter, we began by covering the fundamentals of Geospatial Data Science, Google Earth Engine, and geemap. We then provided guidance on setting up a conda environment for installing geemap and its dependencies. Additionally, we walked through the process of using geemap with Google Colab as a cloud-based alternative to a local installation.

By now, you should have a fully functional conda environment that is ready for working with Earth Engine and geemap. In our next chapter, we will explore geemap in greater depth.
