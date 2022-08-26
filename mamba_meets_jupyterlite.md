# Mamba meets Jupyterliteの日本語訳

この記事は[Mamba meets Jupyterlite](https://blog.jupyter.org/mamba-meets-jupyterlite-88ef49ac4dc8)の日本語翻訳記事です。
typoや翻訳ミスを見つけましたら[こちらのリポジトリ](https://github.com/PyDataOsaka/jupyterblog-translation)のissueから報告いただくか、当該記事に対応したmdファイルに対してプルリクエストを発行していただければと思います。

---

## Mamba meets JupyterLite

<!--
Introducing a mamba-based distribution for WebAssembly, and deploying scalable computing environments with JupyterLite.
-->

mambaを元にしたWebAssemblyパッケージの配布元と、スケーラブルなJupyterlite計算環境のデプロイの仕組みについて紹介する。

![JupyterLite logo](https://miro.medium.com/max/1400/1*dbJO26hiSR8EFygX1rnqrA.png)

<!--
JupyterLite is a Jupyter distribution that runs entirely in the web browser without any server components. To achieve this, all language kernels must also run in the browser.
-->

Jupyterliteは何のサーバーコンポーネントも用いることなくウェブブラウザ内で完全に動作するJupyterのディストリビューションです。
これを実現するために、すべての言語カーネルもブラウザ内で動作しなければなりません。

![A screenshot of JupyterLite running in the Browser. One can see as Matplotlib figure and some Pandas DataFrame code. Furthermore a `p5.js` kernel instance is visible.](https://miro.medium.com/max/1400/0*MoW-XpW5yQgCxinq.png)
<!--*JupyterLite running in the browser as a static website*-->
*ブラウザ内で静的サイトとして動作するJupyterLite*

<!--
A significant benefit of this approach is the ease of deployment. With JupyterLite, the only requirement to provide a live computing environment is a collection of static assets.
-->

このアプローチの重要な利点として開発を容易にすることが挙げられます。
Jupyterliteがあれば、ライブコンピューティング環境を提供するために必要となるのは静的なアセット群のみとなります。

<!--
It makes it possible to embed a console or a notebook interface on any static page or blog without having to deal with a server architecture deployment. The scalability of this approach allowed several major projects of our ecosystem (NumPy, SymPy, Pandas, PyMC, and many more) to embed interactive examples on their websites, which are visited by millions of users monthly.
-->

それによってサーバーアーキテクチャのデプロイを行うことなくコンソールやノートブックのUIを任意の静的ページやブログへと埋め込むことが可能となります。
このアプローチのスケーラビリティによって、我々のエコシステムにおけるいくつかのメジャーなプロジェクト(NumPy, SymPy, Pandas, PyMC, 等々)の、毎月何百万人ものユーザーが訪れるウェブサイト上に、インタラクティブに動作する実例を埋め込むことが可能となります。

<!--
    JupyterLite is the easiest and most scalable way to embed an interactive console or notebook on a web page without any server component.
-->

    Jupyterliteはインタラクティブなコンソールやノートブックを
    サーバーコンポーネントなしでウェブページに埋め込むための
    最も簡単でスケーラブルな方法です。

<!--
The most prominent JupyterLite kernel is the Pyolite Python kernel, which is based on the Pyodide distribution for WebAssembly.
-->
最も用いられているJupyterLiteカーネルはWebAssemblyに対するPyodideディストリビューションを元に構築されているPyolite Pythonカーネルです。

<!--
Beyond the CPython interpreter, Pyodide includes many popular scientific computing packages such as NumPy, Pandas, and Matplotlib.
-->
Pyodideは単にCPythonインタプリタであるだけでなく、NumPy, Pandas, Matplotlibのような多くの科学技術計算パッケージを含んでいます。

<!--
Pyodide also provides a foreign function interface (FFI) that allows calling Python from JavaScript and vice versa.
-->
Pyodideはまたforeign function interface(FFI)を提供しており、FFIによってJavaScriptからPythonを呼ぶことやその逆を行うことが可能となります。

![The JupyterLite inline console embedded on the SymPy project website](https://miro.medium.com/max/1400/1*rKzDNlHO6LnhH1ZDyb996g.png)

<!--### Pyodide — and beyond -->
### Pyodide、そしてその先へ

<!--
While Pyodide provides many scientific computing packages, its monolithic distribution model does not allow to specify package versions, although versions of pure python packages installed on top can be set.
-->
Pyodideは多くの科学技術計算パッケージを提供する一方で、
そのモノリシックなディストリビューションモデルではトップレベルにインストールされるpure pythonパッケージのバージョンを設定することはできますが、パッケージのバージョンを指定することはできません。

<!--
Our goal is to enable the composability of computing environments allowed by package managers and to adopt the conda-forge model for large-scale software distribution crowdsourcing.
-->

我々のゴールはパッケージマネージャによる計算環境の構成を可能とし
大規模なソフトウェアパッケージ配布のクラウドソーシングに向けたconda-forgeのモデルを採用することです。

<!--
Being able to pin down package versions in an environment is a strong requirement for software reproducibility. 
In fact, a locked WebAssembly environment could be seen as a reproducibility time capsule. 
-->

計算環境に特定のバージョンのパッケージを固定できることはソフトウェアにおける再現性のために強く要求されます。 
実際に、固定されたWebAseembly環境は再現性に対するタイムカプセルとして捉えることができます。

<!--
As WebAssembly is a recognized web standard, it ought to be runnable for much longer than native binary packages: these are bound to a combination of architecture and platform and will eventually require an emulator.
-->

WebAssemblyはWeb標準化されたものとして認識されており、ネイティブバイナリパッケージよりも長く実行できるべきであると考えられています。
つまり、アーキテクチャの組み合わせには境界が存在しており、結局のところエミュレータであることが求められます。

<!--
This is why we developed a mamba-based distribution of WebAssembly packages built with Emscripten.
-->
これが我々がEmscriptenを用いて構築されたWebAssemblyパッケージのmambaベースディストリビューションを開発した理由となります。

### Emscripten-forge

<!--
The choice of the Mamba/Conda package manager was natural. 
Its main strength is the conda-forge community-maintained distribution, which has become the de facto standard source of packages for scientific computing.
-->

Mamba/Condaパッケージマネージャを選ぶことは自然です。
その主な強みはコミュニティによってメンテナンスされているパッケージ配布元のconda-forgeであり、
科学技術計算パッケージのデファクトスタンダートな取得元となっています。

<!--
    Beyond its solid technological foundations and the multi-platform nature of the conda-forge distribution, 
    its main strength is its social model.
    It allowed for a crowdsourcing approach of the packaging problem,
    with a balance of separation of concerns between maintainer teams and across-the-board automation,
    plus an amazing maintainers community.

    訳注: across-the-board: 全面的な
-->

    conda-forgeの主な強みは、パッケージ配布における強固な技術的基盤と
    マルチプラットフォームな特性を持つことに加え、そのソーシャルモデルにあります。
    パッケージに関する問題をクラウドソーシングのアプローチによって解決し、
    メンテナチーム間での関心の分離と全面的な自動化のバランスを保つことで、
    驚くべきメンテナコミュニティを実現しています。

<!--
We plan on contributing this work to the conda-forge project, so that all recipes live in the same space.
-->

我々はすべてのレシピが同じ空間上に存在するように、この成果をconda-forgeプロジェクトへとコントリビュートすることを予定しています。

<!--
The Mamba/Conda package manager has support for many platforms and architectures such as Linux, OS X (for both x86 and arm64), and Windows. However, the WebAssembly family of platforms is not supported yet.
-->
Mamba/CondaパッケージマネージャはLinux, OS X(x86とarm64の両方)、Windowsのような多くのプラットフォームとアーキテクチャをサポートしています。
しかしながら、WebAssemblyに属するプラットフォームはまだサポートされていません。

<!-- 以下翻訳途中 -->

<!-- #### Adding support for WebAssembly to mamba & conda -->

#### mamba & condaへのWebAseemblyサポートの追加

<!--
To create conda packages for the WebAssembly platform, we relied on the Emscripten toolchain.
-->
WebAssemblyプラットフォームに対するcondaパッケージを作成するために、我々はEmscriptenツールチェインに頼りました。

<!--
We defined a new target platform for conda-build and boa, namely wasm32-unknown-emscripten for which we use the emscripten-32 shorthand name.
We then associated the Emscripten compiler, wrapped in a conda package as the C/C++ compiler for this new target.
-->
我々はcondaとboaに対して、emscripten-32を省略した名称であるwasm32-unknown-emscriptenと呼ばれる新しいターゲットプラットフォームを定義しました。
そして、我々はこの新しいターゲットに対するC/C++コンパイラとしてcondaパッケージ内に内包される、Emscriptenコンパイラを関連付けました。

<!--
This already allowed us to build many packages, including simple libraries like bzip2 and zlib, but also more complex packages like Python.
For Python extension modules we used crossenv which can create virtual environments for cross-compiling, and cross-python which integrates crossenv into conda.
-->
これはbzip2やzlibなどのような単純なライブラリだけでなく、Pythonのようなより複雑なパッケージを含む多くのパッケージをビルドすることを既に可能としています。
Python拡張モジュールに対しては、我々はクロスコンパイルのための仮想環境を作成可能なcrossenvを用い、crossenvをcondaへと統合するためのcross-pythonを用いました。

<!--
All the code and recipes for cross-compilation are hosted on the emscripten-forge GitHub repository.
We then used GitHub actions to build packages with Emscripten and upload them to a package server.
Packages are hosted on a deployment of the Quetz open-source server.
With this, you can easily create an environment for the emscripten-32 target:
-->
クロスコンパイルのための全てのコードとレシピはemscripten-forgeのGitHuリポジトリにホストされています。
そして、我々はGitHub actionsをEmscriptenを用いたパッケージビルドのために用いそれらをパッケージサーバーへとアップロードします。
パッケージはQuetzオープンソースサーバのデプロイメント上にホストされます。
これがあれば、ユーザーはemscripten-32ターゲットに対する環境を簡単に構築することができるようになります。

```sh
micromamba create -n my-env --platform=emscripten-32 \ 
    -c https://repo.mamba.pm/emscripten-forge \ 
    -c https://repo.mamba.pm/conda-forge \
    python ipython numpy jedi
```

<!--
Note that we not only added emscripten-forge as a channel, but also conda-forge. This means all noarch packages can be used.
-->
我々はemscripten-forgeをchannelとして追加しただけでなく、conda-forgeとしても追加したことに注意してください。これは全てのnoarchパッケージが使用可能となることを意味します。

<!--
#### Adding new packages to the emscripten-forge channel
-->
#### 新しいパッケージのemscripten-forgeチャンネルへの追加

<!--
Adding new packages is a simple procedure:
-->
新しいパッケージを追加することは単純な手続きです:

<!--
* fork the repository https://github.com/emscripten-forge/recipes
* create a folder for your package in recipes/recipes_emscripten/<my_package>
* add a recipe.yaml for your package in recipes/recipes_emscripten/<your_package>
* create a pull request containing the recipe. Once the pull request is merged, the package is automatically uploaded to the emscripten-forge channel.
-->
* リポジトリをforkする https://github.com/emscripten-forge/recipes
* パッケージに対するフォルダを recipes/recipes_emscripten/<my_package> に作成する
* パッケージに対してrecipe.yamlを recipes/recipes_emscripten/<your_package> の中に追加する
* レシピを含むプルリクエストを作成する。プルリクエストがマージされると、パッケージは自動的にemscripten-forgeチャンネルにアップロードされる

<!--
#### Integration with JupyterLite
-->
#### JupyterLiteとの統合

<!--
Even though this is a general-purpose conda-based distribution for Emscripten packages, we had one particular application in mind for this first iteration: JupyterLite. 
The existing Pyolite kernel is too tightly coupled with the Pyodide distribution, so we decided to go with xeus-python instead.
-->
Emscriptenパッケージをcondaベースで配布することは一般的な目的に向けたものですが、
当初から特定のアプリケーション:Jupyterliteについても気に留めていました。
既存のPyoliteカーネルはPyodideディストリビューションと非常に密接に結合しているため、我々はxeus-pythonを用いることに決定しました。

<!--
The main reason for picking xeus-python (over ipykernel) is that with xeus-based kernels, it is possible to override the communication layer of the kernel (switching e.g. from ZMQ to HTTP/2). 
In the case of JupyterLite, the implementation simply relies on direct JavaScript function calls.
-->
(ipykernelではなく)xeus-pythonを選ぶ主な理由はxeusベースのカーネルがあれば、カーネルのコミュニケーション層をオーバーライドすることが可能となるからです(例えばZMQからHTTP/2への切り替え)。
Jupyterliteの場合においては、実装は単純に直接的なJavaScriptの関数呼び出しに依存します。

<!--
You can check out our earlier blog post for more details on the JupyterLite xeus-based kernels.
-->
Jupyterliteのxeusベースのカーネルについての詳細は過去のブログ記事をチェックしてみてください。

<!--
#### Providing a complete Python development experience
-->
#### 完全なPythonでの開発体験の提供

<!--
Some remaining intrinsic limitations to the WebAssembly platform need to be worked around to provide a complete experience to end-users. 
For example, sockets cannot be created in WebAssembly, preventing the use of the default asyncio event loop implementation. 
Luckily, the Pyodide authors developed a custom asyncio event-loop called WebLoop: it wraps the browser event loop using the Python — JavaScript foreign function interface (FFI) provided with Pyodide.
-->
WebAssemblyプラットフォームに残っている制限としてエンドユーザーに完全な体験を提供するためのワークアラウンドが必要となることがあります。
例えば、socketはWebAssemblyでは作ることができず、デフォルトのasyncioのイベントループの実装を使うことを妨げます。
幸運なことに、Pyodideの著者達はWebLoopと呼ばれるカスタム版のasyncioのイベントループを開発しました、これはPyodideによって提供されるPython-JavaScriptのforeign function interface(FFI)を用いてブラウザのイベントループをラップしたものです。

<!--
#### Pyjs:
-->
#### Pyjsとは

<!--
Since it is non-trivial to extract Pyodide’s FFI and use it for other projects, we created a modern Python - JavaScript FFI from scratch. This was done with the following tricks:
-->
PyodideのFFIを抽出し他のプロジェクトで用いることは簡単ではないため、我々はモダンなPython-JavaScript FFIをスクラッチから実装しました。
これは次のトリックによって実現されています。

<!--
* Pybind11 is used to call Python from C++ and vice versa,
* Embind is used to call JavaScript from C++ and vice versa.
-->
* C++からPythonを呼び出すため、またはその逆を行うためにPybind11が用いられます
* C++からJavaScriptを呼び出すため、またはその逆を行うためにEmbindが用いられます 

<!--
When we use Pybind11 and Embind together we can call Python from JavaScript and vice versa, with C++ as a man in the middle. 
This not only allows us to write a simple FFI from scratch with relatively little code but also avoids calling any low-level CPython APIs 
and enables using high-level constructs — like pybind11::object and emscripten::val— instead.
The code is available in the pyjs repository. 
The API is very similar to Pyodide’s so that it can be used as a drop-in replacement in code, like Pyodide’s WebLoop implementation.
-->
Pybind11とEmbindを同時に用いる時、C++を中間に挟むことによって、我々はJavaScriptからPythonを呼び出すことやその逆が可能となります。
これはシンプルはFFIを相対的に少量のコードでスクラッチから書くことを可能とするだけでなく、低レベルなCPython APIの呼び出しを避けることができ、その代わりにpybind11::objectやemscripten::valのような高レベルな構成要素を用いることを可能とします。
このコードはpyjsリポジトリで使用可能です。
APIはPyodideのものと非常に似通っており、そのためコード中でPyodideのWebLoop実装と同じように代替として用いることが可能です。

#### Deployment:

The xeus-python-kernel allows conda packages to be pre-installed in the Python runtime. This can be done by passing the XeusPythonEnv.packages CLI option to jupyter lite build. The following command will install NumPy, Matplotlib, and ipyleaflet:

```sh
jupyter lite build --XeusPythonEnv.packages=\
    numpy,\
    matplotlib,\
    ipyleaflet
```

xeus-python kernel with the ipyleaflet widget visible.

![Running the xeus-python kernel with the ipyleaflet widget in JupyterLite](https://miro.medium.com/max/1400/1*JCiZIwwkFen5kwEA2rK4SA.png)

More details can be found in the xeus-python-kernel GitHub repository.

### What about the future?

This combination of JupyterLite and Mamba has the potential to open Jupyter to millions of additional users.
Given its scalability, ease of deployment, reproducibility, and accessibility, JupyterLite will be everywhere: countries, organizations, and schools that don't have access to sovereign cloud infrastructure will be able to deploy Jupyter-based education platforms on servers that they truly own, without endangering the data of their students or becoming too reliant on resources that they do not control.

#### In the short term, we are working on the following "next steps":

* Mambalite: To support the installation of packages at runtime. Similar to Pyodide’s pip-lite, it will allow downloading packages at runtime.
* Fortran: Compiling Fortran code with Emscripten is currently not supported, but it is necessary for key packages like SciPy. Pyodide relies on f2c, a Fortran-to-C converter, in conjunction with a set of patches to compile Fortran code with Emscripten. We are working on a more direct approach: compiling SciPy natively with LFortran.
* Binderlite: Binder converts a repository of notebooks into an executable JupyterLab environment, making code immediately reproducible by anyone, anywhere. Emscripten-forge is the missing piece to build BinderLite, a version of Binder relying on JupyterLite instances instead of vanilla JupyterLab instances.
* Rust/PyO3 support: We are working on integrating the work of the Pyodide team on Rust/PyO3 support in emscripten-forge. This will be important to build Rust extension modules like cryptography.

### Credits

This was built upon the work of a much bigger crowd!

#### The Pyodide team

The Pyodide project was started at the Mozilla foundation by Michael Droettboom and is now maintained by Hood Chatham, Roman Yurchak, and Gyeongjae Choi.

The foundational work of the Pyodide project pioneered the use of Python in the browser and made all of the rest possible, from JupyterLite to this work.

#### The Emscripten team

Both Pyodide and emscripten-forge are built upon the Emscripten toolchain, which provides the foundational components to be able to meaningfully run WebAssembly programs in the browser.

#### The JupyterLite team

The JupyterLite project was started by Jeremy Tuloup, with significant contributions from Nick Bollweg and Martin Renou.

#### The Mamba Org team

The mamba ecosystem has been instrumental in making these developments possible. We use the Quetz open-source server for hosting the packages and the Boa tool to build them. In the mamba development team, we should highlight the work of Wolf Vollprecht, Johan Mabille, Joel Lamotte, and Andreas Trawöger.

#### The Xeus team

The xeus project was started by Johan Mabille and Sylvain Corlay. It is at the foundation of the JupyterLite integration and helped to get all the pieces together (Xeus, Mamba, Jupyter). We should especially credit the work of Martin Renou and Thorsten Beier on this integration with JupyterLite.

### Acknowledgment

The work of Thorsten Beier, Johan Mabille, Martin Renou, Sylvain Corlay, Wolf Vollprecht, Joel Lamotte, and Andreas Trawoger at QuantStack was funded by Bloomberg.

### About the Authors

#### Thorsten Beier

Thorsten Beier is a Scientific Software Engineer at QuantStack. Before joining QuantStack, he graduated in computer science at the University of Heidelberg and worked at the EMBL. As an open-source developer, Thorsten worked on a variety of projects, from xeus and xtensor in C++ to inferno, kipoi, ilastik, and napari-splineit in Python.

#### Martin Renou

Martin Renou is a Scientific Software Engineer at QuantStack. Before joining QuantStack, he studied at the French Aerospace Engineering School SUPAERO. He also worked at Logilab in Paris and Enthought in Cambridge. As an open-source developer at QuantStack, Martin worked on a variety of projects, from xsimd, xtensor, and xframe in C++ to ipyleaflet and ipywebrtc in Python and JavaScript.

Thanks to David Brochart, Sylvain Corlay, Wolf Vollprecht, and Jeremy Tuloup
