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

Next, activate the new conda environment by typing "conda activate gee" and press **Enter**. Then, install geemap into the environment we just activated by typing "conda install -c conda-forge geemap" and press **Enter** (see {numref}`ch01_conda_geemap`).

```{figure} images/ch01_conda_geemap.jpg
---
name: ch01_conda_geemap
---
Activating the new conda environment and installing geemap.
```

Geemap has a list of optional dependencies specified in the [requirements_all.txt](https://github.com/gee-community/geemap/blob/master/requirements_all.txt), such as GeoPandas, localtileserver, [osmnx](https://github.com/gboeing/osmnx), [rioxarray](https://github.com/corteva/rioxarray) and [rio-cogeo](https://github.com/cogeotiff/rio-cogeo). It can be a bit cumbersome to install these optional dependencies individually, but luckily these optional dependencies are available through the [pygis](https://pygis.gishub.org) Python package which can be installed with a single command.

Since pygis has many dependencies, it might take a while for conda to resolve dependencies. Therefore, we highly recommend you to install [Mamba](https://github.com/mamba-org/mamba), a fast, robust, and cross-platform package manager. Mamba is a re-write of conda that significantly increases the speed of resolving and installing packages. It runs on Windows, macOS, and Linux, and is fully compatible with conda packages and supports most of conda’s commands. The following commands install Mamba and pygis:

```bash
conda install -c conda-forge mamba
mamba install -c conda-forge pygis
```

To install Mamba, type "conda install -c conda-forge mamba" and press **Enter** (see {numref}`ch01_install_mamba`).

```{figure} images/ch01_install_mamba.jpg
---
name: ch01_install_mamba
---
Installing the Mamba package manager.
```

Once Mamba is installed in a conda environment, you can then simply replace any `conda` command with `mamba`. For example, to install pygis, type "mamba install -c conda-forge pygis" and press **Enter** (see {numref}`ch01_install_pygis`).

```{figure} images/ch01_install_pygis.jpg
---
name: ch01_install_pygis
---
Installing optional dependencies of geemap through the pygis package.
```

Congratulations! You have successfully installed geemap and its dependencies. We will dive into geemap in the next chapter.

### Installing with pip

Geemap is also available on [PyPI](https://pypi.org/project/geemap). It can be installed with pip using the following command:

```bash
pip install geemap
```

All optional dependencies of geemap are listed in [requirements_all.txt](https://github.com/gee-community/geemap/blob/master/requirements_all.txt), which can be installed using one of the following:

- `pip install geemap[extra]`: installing extra optional dependencies listed in requirements_extra.txt.
- `pip install geemap[all]`: installing all optional dependencies listed in requirements_all.txt.
- `pip install geemap[backends]`: installing keplergl, pydeck, and plotly.
- `pip install geemap[lidar]`: installing ipygany, ipyvtklink, laspy, panel, pyntcloud[LAS], pyvista, pyvista-xarray, and rioxarray.
- `pip install geemap[raster]`: installing geedim, localtileserver, rio-cogeo, rioxarray, netcdf4, and pyvista-xarray.
- `pip install geemap[sql]`: installing psycopg2 and sqlalchemy.
- `pip install geemap[apps]`: installing gradio, streamlit-folium, and voila
- `pip install geemap[vector]`: installing geopandas and osmnx.

### Installing from source

You may install the latest development version by cloning the GitHub repository with [Git](https://git-scm.com) and using pip to install from the local directory:

```bash
git clone https://github.com/gee-community/geemap
cd geemap
pip install .
```

It is also possible to install the latest development version directly from the GitHub repository with:

```bash
pip install git+https://github.com/gee-community/geemap
```

### Upgrading geemap

If you have installed geemap before and want to upgrade to the latest version, you can run the following command in your terminal:

```bash
pip install -U geemap
```

If you use conda, you can update geemap to the latest version by running the following command in your terminal:

```bash
conda update -c conda-forge geemap
```

To install the development version from GitHub directly within a Jupyter notebook without using Git, run the following code in a Jupyter notebook and restart the kernel to take effect:

```{code-cell} ipython3
import geemap

geemap.update_package()
```

### Using Docker

Geemap is also available on [Docker Hub](https://hub.docker.com/r/giswqs/geemap).

To use geemap in a Docker container, you first need to install [Docker](https://docs.docker.com/get-docker). Once Docker is installed, you can pull the latest geemap image from Docker Hub by running the following command in your terminal:

```bash
docker run -it -p 8888:8888 giswqs/geemap:latest
```

## Creating a Jupyter notebook

Let's activate the conda environment created in the previous section:

```bash
conda activate gee
```

Next, launch JupyterLab by typing the following commands in the **Terminal** or **Anaconda Prompt**:

```bash
jupyter lab
```

JupyterLab will open as a new tab in the browser. Click the **Python 3** icon in the top left corner of the JupyterLab **Launcher** window (see {numref}`ch01_jupyterlab`) or go to **File -> New -> Notebook** to create a new notebook. Select the newly created notebook in the JupyterLab File Browser tab and press **F2** to rename the notebook, e.g., **chapter01.ipynb**.

```{figure} images/ch01_jupyterlab.jpg
---
name: ch01_jupyterlab
---
The JupyterLab user interface.
```

Jupyter notebook has two modes: **Edit mode** and **Command mode**. The Edit mode allows you to type into the cells like a normal text editor. The Command mode allows you to edit the notebook as a whole, but not type into individual cells. Jupyter notebook has many keyboard shortcuts {cite}`Yordanov2017-hl`. Here are some commonly used shortcuts. Note that the shortcuts are for Windows and Linux users. For Mac users, replace `Ctrl` with `Command`.

Shortcuts in both modes:

- `Shift + Enter`: run the current cell, select below
- `Ctrl + Enter`: run selected cells
- `Alt + Enter`: run the current cell, insert below
- `Ctrl + S`: save and checkpoint

While in command mode (press `Esc` to activate):

- `A`: insert cell above
- `B`: insert cell below
- `X`: cut selected cells
- `C`: copy selected cells
- `V`: paste cells below
- `Y`: change the cell type to Code
- `M`: change the cell type to Markdown
- `P`: open the command palette

While in edit mode (press `Enter` to activate):

- `Esc`: activate the command mode
- `Tab`: code completion or indent
- `Shift + Tab`: show tooltip

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
