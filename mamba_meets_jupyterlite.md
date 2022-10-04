# Mamba meets Jupyterliteの日本語訳

この記事は[Mamba meets Jupyterlite](https://blog.jupyter.org/mamba-meets-jupyterlite-88ef49ac4dc8)の日本語翻訳記事です。
typoや翻訳ミスを見つけましたら[こちらのリポジトリ](https://github.com/PyDataOsaka/jupyterblog-translation)のissueから報告いただくか、当該記事に対応したmdファイルに対してプルリクエストを発行していただければと思います。

---

## Mamba meets JupyterLite

<!--
Introducing a mamba-based distribution for WebAssembly, and deploying scalable computing environments with JupyterLite.
-->

mambaをベースにしたWebAssembly用パッケージ配布と、Jupyterliteを用いたスケーラブルな計算環境のデプロイ方法について紹介する。

![JupyterLite logo](https://miro.medium.com/max/1400/1*dbJO26hiSR8EFygX1rnqrA.png)

<!--
JupyterLite is a Jupyter distribution that runs entirely in the web browser without any server components. To achieve this, all language kernels must also run in the browser.
-->

Jupyterliteはサーバーを用いることなくウェブブラウザ内で完全に動作するJupyterのディストリビューションです。
この動作を実現するためには、(Jupyterの)言語カーネルもブラウザ内で動作させなければなりません。

![A screenshot of JupyterLite running in the Browser. One can see as Matplotlib figure and some Pandas DataFrame code. Furthermore a `p5.js` kernel instance is visible.](https://miro.medium.com/max/1400/0*MoW-XpW5yQgCxinq.png)
<!--*JupyterLite running in the browser as a static website*-->
*ブラウザ内で静的サイトとして動作するJupyterLite*

<!--
A significant benefit of this approach is the ease of deployment. With JupyterLite, the only requirement to provide a live computing environment is a collection of static assets.
-->

このアプローチの大きな利点は**デプロイが容易となる**ことです。
Jupyterliteがあれば、ライブコンピューティング環境を提供するために必要となるのは静的なアセット群のみとなります。

<!--
It makes it possible to embed a console or a notebook interface on any static page or blog without having to deal with a server architecture deployment. The scalability of this approach allowed several major projects of our ecosystem (NumPy, SymPy, Pandas, PyMC, and many more) to embed interactive examples on their websites, which are visited by millions of users monthly.
-->

これにより、予めサーバーアーキテクチャのデプロイを行わずとも、コンソールやノートブックのUIを任意の静的ページやブログへと埋め込むことが可能となります。
このアプローチの**拡張性**によって、私たちのエコシステム([NumPy](https://numpy.org/), [SymPy](https://www.sympy.org/en/shell.html), [Pandas](https://pandas.pydata.org/getting_started.html), [PyMC](https://www.pymc.io/welcome.html), 等々)の内のいくつかのメジャーなプロジェクトはウェブサイト上にインタラクティブに動作する実例を埋め込めるようになり、そこには毎月何百万人ものユーザーが訪れています。

<!--
    JupyterLite is the easiest and most scalable way to embed an interactive console or notebook on a web page without any server component.
-->

    Jupyterliteはインタラクティブなコンソールやノートブックを
    サーバーコンポーネントなしでウェブページに埋め込むための
    最も簡単でスケーラブルな方法です。

<!--
The most prominent JupyterLite kernel is the Pyolite Python kernel, which is based on the Pyodide distribution for WebAssembly.
-->
最も有名なJupyterLiteのカーネルはPyolite Pythonカーネルであり、WebAssembly向けの[Pyodide](https://pyodide.org/en/stable/)ディストリビューションを元に構築されています。

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
そのモノリシックな配布モデルでは、Pyodide上にインストールされるPythonのみで書かれたパッケージのバージョンを設定することはできますが、そうではないパッケージのバージョンを指定することはできません。

<!--
Our goal is to enable the composability of computing environments allowed by package managers and to adopt the conda-forge model for large-scale software distribution crowdsourcing.
-->
我々のゴールはパッケージマネージャによる**計算環境の構成**を可能とし
大規模なソフトウェアパッケージ配布のクラウドソーシングのためにconda-forgeのモデルを採用することです。

<!--
Being able to pin down package versions in an environment is a strong requirement for software reproducibility. 
In fact, a locked WebAssembly environment could be seen as a reproducibility time capsule. 
-->

計算環境に特定のバージョンのパッケージを固定できることはソフトウェアにおける**再現性**のために強く要求されます。 
実際に、固定されたWebAseembly環境は再現性の**タイムカプセル**として捉えることができます。

<!--
As WebAssembly is a recognized web standard, it ought to be runnable for much longer than native binary packages: these are bound to a combination of architecture and platform and will eventually require an emulator.
-->

WebAssemblyはWeb標準化されたものとして認識されており、ネイティブバイナリパッケージよりもはるかに長く実行できるはずです。
つまり、これらはアーキテクチャとプラットフォームの組み合わせに縛られるため、最終的にはエミュレータが必要となるでしょう。

<!--
This is why we developed a mamba-based distribution of WebAssembly packages built with Emscripten.
-->
これが我々がEmscriptenを用いて構築されたWebAssemblyパッケージのmambaベースによる配布方法を開発した理由となります。

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
しかしながら、**WebAssembly**に属するプラットフォームはまだサポートされていません。

<!-- #### Adding support for WebAssembly to mamba & conda -->

#### mamba & condaへのWebAseemblyサポートの追加

<!--
To create conda packages for the WebAssembly platform, we relied on the Emscripten toolchain.
-->
WebAssemblyプラットフォーム用のcondaパッケージを作成するために、我々は[Emscriptenツールチェイン](http://emscripten%20toolchain/)に頼りました。

<!--
* We defined a new target platform for conda-build and boa, namely wasm32-unknown-emscripten for which we use the emscripten-32 shorthand name.
We then associated the Emscripten compiler, wrapped in a conda package as the C/C++ compiler for this new target.
This already allowed us to build many packages, including simple libraries like bzip2 and zlib, but also more complex packages like Python.
* For Python extension modules we used crossenv which can create virtual environments for cross-compiling, and cross-python which integrates crossenv into conda.
All the code and recipes for cross-compilation are hosted on the emscripten-forge GitHub repository.
* We then used GitHub actions to build packages with Emscripten and upload them to a package server.
* Packages are hosted on a deployment of the Quetz open-source server.
-->
* 我々はcondaとboaに対して、`emscripten-32`を省略した名称である`wasm32-unknown-emscripten`と呼ばれる新しいターゲットプラットフォームを定義しました。
そして、我々は[Emscripten](http://emscripten%20toolchain/)コンパイラを関連付けました。それはこの新しいターゲット用の C/C++ コンパイラとして[condaパッケージに同封](https://github.com/emscripten-forge/recipes/tree/main/recipes/recipes/emscripten_emscripten-32)されています。
これは`bzip2`や`zlib`などのような単純なライブラリだけでなく、`Python`のようなより複雑なパッケージを含む多くのパッケージをビルドすることを既に可能としています。
* Python拡張モジュールに対しては、我々はクロスコンパイルのための仮想環境を作成可能な[crossenv](http://crossenv/)を用い、crossenvをcondaへと統合するための[cross-python](https://github.com/conda-forge/cross-python-feedstock)を用いました。
クロスコンパイルのための全てのコードとレシピは[emscripten-forgeのGitHub](https://github.com/emscripten-forge/recipes)リポジトリにホストされています。
* そして、[GitHub actions](https://github.com/emscripten-forge/recipes/blob/main/.github/workflows/build_recipes.yaml)を用いてEmscriptenでパッケージをビルドし、それらをパッケージサーバーへとアップロードします。
* パッケージは[Quetz](https://github.com/mamba-org/quetz)オープンソースサーバのデプロイメント上にホストされます。

<!--
With this, you can easily create an environment for the emscripten-32 target:
-->
これがあれば、ユーザーは`emscripten-32`ターゲット用の環境を(下記のように)簡単に構築することができるようになります:

```sh
micromamba create -n my-env --platform=emscripten-32 \ 
    -c https://repo.mamba.pm/emscripten-forge \ 
    -c https://repo.mamba.pm/conda-forge \
    python ipython numpy jedi
```

<!--
Note that we not only added emscripten-forge as a channel, but also conda-forge. This means all noarch packages can be used.
-->
我々はemscripten-forgeをchannelとして追加しただけでなく、[conda-forge](https://repo.mamba.pm/conda-forge)としても追加したことに注意してください。これは全てのnoarchパッケージが使用可能となることを意味します。

<!--
#### Adding new packages to the emscripten-forge channel
-->
#### 新しいパッケージのemscripten-forgeチャンネルへの追加

<!--
Adding new packages is a simple procedure:
-->
新しいパッケージを追加するのは簡単な手続きです:

<!--
* fork the repository https://github.com/emscripten-forge/recipes
* create a folder for your package in recipes/recipes_emscripten/<my_package>
* add a recipe.yaml for your package in recipes/recipes_emscripten/<your_package>
* create a pull request containing the recipe. Once the pull request is merged, the package is automatically uploaded to the emscripten-forge channel.
-->
* リポジトリをforkする https://github.com/emscripten-forge/recipes
* パッケージに対するフォルダを [recipes/recipes_emscripten/<my_package>](https://github.com/emscripten-forge/recipes/tree/main/recipes/recipes_emscripten) に作成する
* パッケージに対して[recipe.yaml](https://github.com/emscripten-forge/recipes/blob/main/recipes/recipes_emscripten/widgetsnbextension/recipe.yaml)を`recipes/recipes_emscripten/<your_package>`の中に追加する
* レシピを含むプルリクエストを作成する。プルリクエストがマージされると、パッケージは自動的に`emscripten-forge`チャンネルにアップロードされる

<!--
#### Integration with JupyterLite
-->
#### JupyterLiteとの統合

<!--
Even though this is a general-purpose conda-based distribution for Emscripten packages, we had one particular application in mind for this first iteration: JupyterLite. 
The existing Pyolite kernel is too tightly coupled with the Pyodide distribution, so we decided to go with xeus-python instead.
-->
Emscriptenパッケージをcondaベースで配布することは一般的な目的に向けたものですが、
当初から特定のアプリケーション:**Jupyterlite**についても気に留めていました。
既存のPyoliteカーネルはPyodideディストリビューションと非常に密接に結合しているため、我々は[xeus-python](https://github.com/jupyter-xeus/xeus-python)を用いることに決定しました。

<!--
The main reason for picking xeus-python (over ipykernel) is that with xeus-based kernels, it is possible to override the communication layer of the kernel (switching e.g. from ZMQ to HTTP/2). 
In the case of JupyterLite, the implementation simply relies on direct JavaScript function calls.
-->
(ipykernelではなく)[xeus-python](https://github.com/jupyter-xeus/xeus-python)を選ぶ主な理由はxeusベースのカーネルがあれば、カーネルのコミュニケーション層をオーバーライドすることが可能となるからです(例えばZMQからHTTP/2への切り替え)。
Jupyterliteの場合においては、実装は単純に直接的なJavaScriptの関数呼び出しに依存します。

<!--
You can check out our earlier blog post for more details on the JupyterLite xeus-based kernels.
-->
Jupyterliteのxeusベースのカーネルについての詳細は[過去のブログ記事](https://blog.jupyter.org/xeus-lite-379e96bb199d)をチェックしてみてください。

<!--
#### Providing a complete Python development experience
-->
#### 完全なPythonでの開発体験の提供

<!--
Some remaining intrinsic limitations to the WebAssembly platform need to be worked around to provide a complete experience to end-users. 
For example, sockets cannot be created in WebAssembly, preventing the use of the default asyncio event loop implementation. 
Luckily, the Pyodide authors developed a custom asyncio event-loop called WebLoop: it wraps the browser event loop using the Python — JavaScript foreign function interface (FFI) provided with Pyodide.
-->
WebAssemblyプラットフォームに残っている制限としてエンドユーザーに完全な体験を提供するための応急措置が必要となることがあります。
例えば、socket は WebAssembly では作ることができず、デフォルトの asyncio イベントループ実装を使用できません。
幸いなことに、Pyodideの作者達は[WebLoop](https://pyodide.org/en/latest/usage/api/python-api/webloop.html)と呼ばれるカスタム版のasyncioのイベントループを開発しました、これは[Pyodide](https://pyodide.org/en/stable/usage/type-conversions.html)によって提供されるPython-JavaScriptのforeign function interface(FFI)を用いてブラウザのイベントループをラップしたものです。

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
* C++からPythonを呼び出すため、またはその逆を行うために[Pybind11](https://github.com/pybind/pybind11)が用いられます
* C++からJavaScriptを呼び出すため、またはその逆を行うために[Embind](https://emscripten.org/docs/porting/connecting_cpp_and_javascript/embind.html)が用いられます 

<!--
When we use Pybind11 and Embind together we can call Python from JavaScript and vice versa, with C++ as a man in the middle. 
This not only allows us to write a simple FFI from scratch with relatively little code but also avoids calling any low-level CPython APIs 
and enables using high-level constructs — like pybind11::object and emscripten::val— instead.
The code is available in the pyjs repository. 
The API is very similar to Pyodide’s so that it can be used as a drop-in replacement in code, like Pyodide’s WebLoop implementation.
-->
[Pybind11](https://github.com/pybind/pybind11)と[Embind](https://emscripten.org/docs/porting/connecting_cpp_and_javascript/embind.html)を同時に用いる場合は、C++を中間に挟むことによって、我々はJavaScriptからPythonを呼び出すことやその逆が可能となります。
これはシンプルなFFIを比較的少ないコードでスクラッチから書くことを可能とするだけでなく、低レベルなCPython APIの呼び出しを避けることができ、その代わりに[pybind11::object](https://pybind11.readthedocs.io/en/stable/advanced/pycpp/object.html#calling-python-functions)や[emscripten::val](https://emscripten.org/docs/api_reference/val.h.html)のような高レベルな構成要素を用いることを可能とします。
このコードはpyjsリポジトリで使用可能です。
APIはPyodideのものと非常に似通っており、そのためコード中でPyodideの[WebLoop](https://pyodide.org/en/latest/usage/api/python-api/webloop.html)実装と同じように代替として用いることが可能です。

<!--
#### Deployment:
-->
#### デプロイ方法:

<!--
The xeus-python-kernel allows conda packages to be pre-installed in the Python runtime. 
This can be done by passing the XeusPythonEnv.packages CLI option to jupyter lite build. 
The following command will install NumPy, Matplotlib, and ipyleaflet:
-->
[xeus-python-kernel](https://github.com/jupyterlite/xeus-python-kernel)はcondaパッケージをPythonランタイム内にプリインストールすることを可能とします。
これは`XuesPythonEnv.packages`のCLIオプションを`jupyter lite build`に渡すことによって行えるようになります。
次のコマンドは`NumPy`, `Matplotlib`, `ipyleaflet`をインストールします。 

```sh
jupyter lite build --XeusPythonEnv.packages=\
    numpy,\
    matplotlib,\
    ipyleaflet
```

<!--
xeus-python kernel with the ipyleaflet widget visible.
-->
xeus-pythonカーネルを用いてipyleafletウィジェットが可視化されている例。

![Running the xeus-python kernel with the ipyleaflet widget in JupyterLite](https://miro.medium.com/max/1400/1*JCiZIwwkFen5kwEA2rK4SA.png)

<!--
More details can be found in the xeus-python-kernel GitHub repository.
-->
[xeus-python-kernelのGitHubリポジトリ](https://github.com/jupyterlite/xeus-python-kernel)に更なる詳細が載っています。

<!--
### What about the future?
-->
### その将来はどうなるのか?

<!--
This combination of JupyterLite and Mamba has the potential to open Jupyter to millions of additional users.
Given its scalability, ease of deployment, reproducibility, and accessibility, JupyterLite will be everywhere: 
countries, organizations, and schools that don't have access to sovereign cloud infrastructure will be able to deploy Jupyter-based education platforms on servers that they truly own, 
without endangering the data of their students or becoming too reliant on resources that they do not control.
-->
JupyterLiteとMambaの連携は更に何百万人ものユーザーにJupyterを使用してもらう潜在的な可能性を秘めています。
そのスケーラビリティと、デプロイの容易さ、再現性、そしてアクセシビリティがあれば、
JupyterLiteはあらゆる場所で活用されるでしょう。
つまり、独立したクラウドインフラストラクチャへのアクセスができない国、組織、学校は、
生徒のデータを危険に晒したり、生徒がコントロールしないリソースに依存し過ぎることなく、
実際に所有するサーバー上にJupyterベースの教育プラットフォームを展開することが可能となるでしょう。

<!--
#### In the short term, we are working on the following "next steps":
-->
#### 「次のステップ」として取り組んでいる項目の要約

<!--
* Mambalite: To support the installation of packages at runtime. Similar to Pyodide’s pip-lite, it will allow downloading packages at runtime.
* Fortran: Compiling Fortran code with Emscripten is currently not supported, but it is necessary for key packages like SciPy. Pyodide relies on f2c, a Fortran-to-C converter, in conjunction with a set of patches to compile Fortran code with Emscripten. We are working on a more direct approach: compiling SciPy natively with LFortran.
* Binderlite: Binder converts a repository of notebooks into an executable JupyterLab environment, making code immediately reproducible by anyone, anywhere. Emscripten-forge is the missing piece to build BinderLite, a version of Binder relying on JupyterLite instances instead of vanilla JupyterLab instances.
* Rust/PyO3 support: We are working on integrating the work of the Pyodide team on Rust/PyO3 support in emscripten-forge. This will be important to build Rust extension modules like cryptography.
-->
* **Mambalite**: ランタイム上でのパッケージインストールのサポート。Pyodideのpip-liteと同様に、ランタイム上でパッケージをダウンロードすることを可能とします。
* **Fortran**: Emscriptenを用いてFortranのコードをコンパイルすることは現在サポートされていませんが、SciPyのようなキーとなるパッケージで必要不可欠です。PyodideはFortranをCへと変換するコンバータのf2cに依存しており、FortranのコードをEmscriptenでコンパイルするためのパッチの集合を用いています。我々はより直接的な、SciPyを[LFortran](https://lfortran.org/)を用いてコンパイルするアプローチを試しています。 
* **Binderlite**: Binderはnotebookを含むリポジトリをJupyterLab環境上で実行できるように変換し、コードを即座に誰にでも、どこででも再現可能とするためのものです。普通の JupyterLab インスタンスの代わりに JupyterLite インスタンスに依存するバージョンの Binder となる BinderLite を構築するためピースがEmscripten-forgeには欠けています。
* **Rust/PyO3サポート**: 我々は[PyodideチームのRust/PyO3サポートに関する成果](https://blog.pyodide.org/posts/rust-pyo3-support-in-pyodide/)をemscripten-forgeへ統合することを進めています。これはcryptograpyのようなRustの拡張モジュールをビルドする上で重要となるでしょう。

<!--
### Credits
-->
### 功績に対する称賛

<!--
This was built upon the work of a much bigger crowd!
-->
このプロジェクトは多くの方々の成果の上に成り立っています！

<!--
#### The Pyodide team
-->
#### Pyodideチーム

<!--
The Pyodide project was started at the Mozilla foundation by Michael Droettboom and is now maintained by Hood Chatham, Roman Yurchak, and Gyeongjae Choi.
-->
PyodideプロジェクトはMozilla foundationで[Michael Droettboom](https://twitter.com/MDroettboom)によって始められ、今は[Hood Chatham](https://github.com/hoodmane), [Roman Yurchak](https://twitter.com/RomanYurchak), [Gyeongjae Choi](https://github.com/ryanking13)らによってメンテナンスされています。

<!-- The foundational work of the Pyodide project pioneered the use of Python in the browser and made all of the rest possible, from JupyterLite to this work. -->
    Pyodideプロジェクトが実現したブラウザ内でPythonを使用可能にするという基盤となる成果に基づいて、
    Jupyterliteから今回の成果に至るまでの全ての成果が生み出されました。
   
<!-- #### The Emscripten team -->
#### Emscriptenチーム

<!-- Both Pyodide and emscripten-forge are built upon the Emscripten toolchain, which provides the foundational components to be able to meaningfully run WebAssembly programs in the browser. -->
Pyodideとemscripten-forgeの両方がEmscriptenツールチェイン上に構築され、WebAssemblyのプログラムをWebブラウザ上で意図通りに実行できるようにするための基盤となるコンポーネントを提供しています。

<!-- #### The JupyterLite team -->
#### JupyterLiteチーム

<!-- The JupyterLite project was started by Jeremy Tuloup, with significant contributions from Nick Bollweg and Martin Renou. -->
JupyterLiteプロジェクトは[Jeremy Tuloup](https://twitter.com/jtpio)によって始められ、[Nick Bollweg](https://github.com/bollwyvl)と[Marting Renou](https://twitter.com/martinrenou)らから重要な貢献を受けたプロジェクトです。

<!-- #### The Mamba Org team -->
#### Mamba Orgチーム

<!-- The mamba ecosystem has been instrumental in making these developments possible. We use the Quetz open-source server for hosting the packages and the Boa tool to build them. In the mamba development team, we should highlight the work of Wolf Vollprecht, Johan Mabille, Joel Lamotte, and Andreas Trawöger. -->
mambaのエコシステムはこれらの開発を可能にするための手段となりつつあります。
我々はQuestzオープンソースサーバをパッケージのホスティングに利用し、Boaというツールをそれらのパッケージをビルドするために用いています。mamba開発チームにおいて、我々は[Wolf Vollprecht](https://twitter.com/wuoulf), [Johan Mabille](https://twitter.com/johanmabille), [Joel Lamotte](https://twitter.com/MJKlaim), [Andreas Trawöge](https://twitter.com/atrawog)の成果を強調したいと思います。

<!-- #### The Xeus team -->
#### Xeusチーム

<!-- The xeus project was started by Johan Mabille and Sylvain Corlay.
It is at the foundation of the JupyterLite integration and helped to get all the pieces together (Xeus, Mamba, Jupyter).
We should especially credit the work of Martin Renou and Thorsten Beier on this integration with JupyterLite. -->
Xeusプロジェクトは[Johan Mabille](https://twitter.com/johanmabille)と[SylvainCorlay](https://twitter.com/JohanMabille/)によって始められました。
XeusプロジェクトはJupyterLiteへの統合の基礎となり、全てのピース(Xeus, Mamba, Jupyter)をまとめ上げる役割を果たしました。
この JupyterLite との統合における [Martin Renou](https://twitter.com/martinRenou)と[Thorsten Beier](https://twitter.com/thorstenbeier)の功績は特筆すべきです。

<!-- ### Acknowledgment -->
### 謝辞

[QuantStack](https://twitter.com/QuantStack)での Thorsten Beier, Johan Mabille, Martin Renou, Sylvain Corlay, Wolf Vollprecht, Joel Lamotte, Andreas Trawoger らの仕事は[Bloomberg](https://twitter.com/TechAtBloomberg?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor)による資金援助を受けました。

<!-- ### About the Authors -->
### 著者達について

#### Thorsten Beier

<!-- Thorsten Beier is a Scientific Software Engineer at QuantStack.
Before joining QuantStack, he graduated in computer science at the University of Heidelberg and worked at the EMBL. As an open-source developer, Thorsten worked on a variety of projects, from xeus and xtensor in C++ to inferno, kipoi, ilastik, and napari-splineit in Python. -->
[Thorsten Beier](https://twitter.com/thorstenbeier)は[QuantStack](https://twitter.com/QuantStack)で働く科学ソフトウェアエンジニアです。
[QuantStack](http://quantstack.net/)にジョインする前は、Heidelberg大学でコンピュータサイエンスの学位を取得し[EMBL](https://www.embl.org/)で働きました。
オープンソース開発者として、ThorstenはC++では[xeus](https://github.com/jupyter-xeus/xeus)や[xtensor](https://github.com/QuantStack/xtensor)、Pythonでは[inferno](https://github.com/inferno-pytorch/inferno), [kipoi](https://kipoi.org/), [ilastik](https://www.ilastik.org/), [napari-splineit](https://github.com/uhlmanngroup/napari-splineit)など、さまざまなプロジェクトに貢献しました。

#### Martin Renou

<!-- Martin Renou is a Scientific Software Engineer at QuantStack. Before joining QuantStack, he studied at the French Aerospace Engineering School SUPAERO. He also worked at Logilab in Paris and Enthought in Cambridge. As an open-source developer at QuantStack, Martin worked on a variety of projects, from xsimd, xtensor, and xframe in C++ to ipyleaflet and ipywebrtc in Python and JavaScript. -->
[Martin Renou](https://twitter.com/martinRenou)は[QuantStack](https://twitter.com/QuantStack)で働く科学ソフトウェアエンジニアです。
[QuantStack](http://quantstack.net/)にジョインする前は、彼は仏・航空宇宙工学学校[SUPAERO](https://www.isae-supaero.fr/en)で学んでいました。
彼はまたパリのLogilabで働き、ケンブリッジのEnthoughtで働きました。
[QuantStack](http://quantstack.net/)におけるオープンソース開発者として、MartinはC++では[xsimd](https://github.com/QuantStack/xsimd), [xtensor](https://github.com/QuantStack/xtensor), [xframe](https://github.com/QuantStack/xframe)、PythonとJavaScriptでは[ipyleaflet](https://github.com/jupyter-widgets/ipyleaflet)や[ipywebrtc](https://github.com/maartenbreddels/ipywebrtc)など、さまざまなプロジェクトに貢献しました。

<!-- Thanks to David Brochart, Sylvain Corlay, Wolf Vollprecht, and Jeremy Tuloup -->
David Brochart, Sylvain Corlay, Wolf Vollprecht, Jeremy Tuloupに感謝します。
