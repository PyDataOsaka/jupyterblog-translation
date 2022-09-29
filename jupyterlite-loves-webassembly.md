# JupyterLite:Jupyter❤️WebAssembly❤️Pythonの日本語訳

この記事は[JupyterLite:Jupyter❤️WebAssembly❤️Python](https://blog.jupyter.org/jupyterlite-jupyter-%EF%B8%8F-webassembly-%EF%B8%8F-python-f6e2e41ab3fa)の日本語翻訳記事です。
typoや翻訳ミスを見つけましたら[こちらのリポジトリ](https://github.com/PyDataOsaka/jupyterblog-translation)のissueから報告いただくか、当該記事に対応したmdファイルに対してプルリクエストを発行していただければと思います。

## JupyterLite:Jupyter❤️WebAssembly❤️Python

<!-- JupyterLite is a JupyterLab distribution that runs entirely in the web browser, backed by in-browser language kernels. -->
JupyterLiteはブラウザ内で動作する言語カーネルに支えられた、完全にウェブブラウザ中で動作するJupyterLabのディストリビューションです。

![](https://miro.medium.com/max/1024/1*CMvcTaLSAD5A-WHCtnIFwA.png)

<!-- ### Motivation -->
### モチベーション 

<!-- JupyterLite is a reboot of several attempts at making a full static Jupyter distribution that runs in the browser, without having to start the Python Jupyter Server on the host machine, usually done by running jupyter lab or jupyter notebook in a terminal. -->
JupyterLiteは、ブラウザ内で動作する完全な静的Jupyterディストリビューションを作る試みの仕切り直しになります。それは、通常ターミナルでjupyter labやjupyter notebookを実行することによって行われるホストマシン上のPythonのJupyterサーバーの起動を必要としません。

<!-- The goal of the project is to provide a lightweight computing environment accessible in a matter of seconds with a single click, in a web browser, and without having to install anything on the end-user device. -->
プロジェクトの目標はエンドユーザーのデバイス上に何かをインストールさせる必要なく、シングルクリックで数秒のうちにアクセス可能となる軽量な計算機環境を供給することです。

<!-- With in-browser distributions, there is no need to provision the execution environment in the backend. Since the application is mostly a set of static files, it scales more easily, and it is also easier to deploy. -->
ブラウザ内で動作するディストリビューションがあれば、バックエンドに実行環境を準備する必要がなくなります。
アプリケーションはほとんどが静的ファイルの集合であるため、より簡単にスケールさせることが可能となり、またより簡単にデプロイすることが可能となります。

![](https://miro.medium.com/max/1400/1*bR--bXsuiMIDz-D_V4BfLw.png)
<!-- JupyterLite running in the browser as a static website on ReadTheDocs -->
ReadTheDocs上の静的なウェブサイトとしてブラウザ内で動作するJupyterLite

<!-- ### A full JupyterLab distribution running in the browser -->
### ブラウザ内で動作する完全なJupyterLabディストリビューション

<!-- JupyterLite is built from the ground up, reusing many JupyterLab plugins and components as is. -->
JupyterLiteは、多くのJupyterLabのプラグインやコンポーネントをそのまま再利用できるよう新規に構築されています。

<!-- In addition to JupyterLab, JupyterLite also includes the RetroLab interface by default: -->
JupyterLabに加えて、JupyterLiteはRetroLabインタフェースをデフォルトで含んでいます。

![](https://miro.medium.com/max/1400/1*B1Se7Fe1JXBt2a1NjwRmxg.png)
<!-- JupyterLite with the RetroLab interface -->
RetroLabインタフェースを備えたJupyterLite

<!-- By reusing JupyterLab components, JupyterLite benefits from many of the upstream improvements such as new features, accessibility fixes, and upkeep improvements. 
The recent work on real time collaboration coming in JupyterLab 3.1 and championed by Kevin Jahns, Carlos Herrero, and Eric Charles can be enabled in JupyterLite too! -->
JupyterLabのコンポーネントを再利用することによって、JupyterLiteは新機能や、アクセシビリティの改善、メンテナンス面の改善のようなupstreamにおける多くの改善点の恩恵を受けることができます。
Kevin Jahns, Carlos Herrero, Eric Charles によって支援された、JupyterLab 3.1で導入されるリアルタイムコラボレーションにおける最近の成果もJupyterLite内で有効化することができます！

![](https://miro.medium.com/max/1400/1*HwMx3Fd6iICkWjjvUCtvwA.gif)
<!-- Real Time Collaboration with JupyterLite on ReadTheDocs -->
ReadTheDocs上で動作するJupyterLiteにおけるリアルタイムコラボレーション

<!-- ### Pyolite, a Python kernel backed by Pyodide -->
Pyodideをバックエンドとして用いたPythonカーネルであるPyolite

<!-- Pyodide consists of the CPython 3.8 interpreter compiled to WebAssembly which allows Python to run in the browser. -->
<!-- Many popular scientific Python packages have also been compiled and made available.  -->
PyodideはPythonがブラウザ上で動作するようにWebAssemblyへとコンパイルされたCPython 3.8(訳注: 記事執筆時)のインタプリタを含んでいます。
多くのポピュラーな科学技術計算用のPythonパッケージについてもJupyterLiteで利用できるようにコンパイルされています。

<!-- In addition, Pyodide can install any Python package with a pure Python wheel from the Python Package Index (PyPI). -->
加えて、Pyodideは全てのpure Python wheelのPythonパッケージをPython Package Index(PyPI)からインストールできます。

<!-- Pyodide also includes a comprehensive foreign function interface that exposes the ecosystem of Python packages to JavaScript and the browser user interface, including the DOM, to Python. -->
PyodideはPythonパッケージのエコシステムをJavaScriptおよびブラウザのUIへと公開するための包括的なforeign function interfaceも含んでいます。

![](https://miro.medium.com/max/1400/1*usFUmvzRo6v8TuRFQhK0uw.png)
<!-- Pyodide: Python with the scientific stack, compiled to WebAssembly -->
Pyodide: WebAssemblyへとコンパイルされた科学技術計算スタックを備えたPython

<!-- Currently at version 0.17, Pyodide has benefited from many improvements over the past years: smaller binary size, support for asyncio, and better type translations between Python and JavaScript. -->
現在はバージョン0.17(訳注: 現在は0.21)であり、Pyodideは数年を経て多くの改善による恩恵(より小さなバイナリサイズ、asyncioのサポート、PythonとJavaScriptの間におけるより良い型変換、など)を受けてきています。

<!-- JupyterLite ships by default with Pyolite, a Python kernel backed by Pyodide. Pyolite runs in a Web Worker and thus doesn’t block the main UI thread when intensive computations are executed. -->
JupyterLiteはデフォルトでPyodideをバックエンドとしたPythonカーネルであるPyoliteとともに配布されます。
PyoliteはWeb Worker内で動作するため、重たい計算が実行された場合においてもメインのUIスレッドをブロックすることはありません。

<!-- ### IPython in the browser -->
### ブラウザ内におけるIPython

<!-- Thanks to the work by Madhur Tandon in this pull request, Pyolite is now powered by IPython. -->
<!-- This provides access to magics, code completion, rich display, interactive widgets, and many other features. -->
[このプルリクエスト]()におけるMadhur Tandonの仕事のおかげで、PyoliteはIPythonの力によって動作するようになりました。
これによりmagicコマンドやコード補完、リッチな表示、インタラクティブなウィジェット、そしてその他の多くの機能へのアクセスが提供されます。

![](https://miro.medium.com/max/1400/1*2WBnuwRGkASGVNYUGjyPHg.gif)
<!-- Using IPython in JupyterLite -->
JupyterLiteでのIPythonの使用

<!-- ### Interactive Visualization -->
### インタラクティブな可視化

<!-- Many visualizations libraries such as Altair and Plotly are also supported in JupyterLite, allowing for quick and convenient figures and plots right in the browser: -->
AltairやPlotlyのような多くの可視化ライブラリもJupyterLiteでサポートされており、ブラウザで素早く便利な図やプロットを作成できます。

![](https://miro.medium.com/max/1400/1*SRr162bkckWsmoSMxVd2uQ.gif)
<!-- Using Altair in JupyterLite -->
JupyterLiteでのAltairの使用

![](https://miro.medium.com/max/1400/1*w7Y4wRz9h2OFMgm5MfdD0A.gif)
<!-- Creating Plotly figures in JupyterLite -->
JupyterLiteでのPlotlyの図の作成

<!-- ### Support for Jupyter Widgets -->
### Jupyterウィジェットのサポート

<!-- Jupyter Widgets rely on the Custom Messages specification of the Jupyter Protocol to send messages back and forth between the kernel and the frontend. -->
Jupyterウィジェットは、カーネルとフロントエンドの間でのメッセージのやり取りのためのJupyterプロトコルのカスタムメッセージ仕様に依存しています。

<!-- This pull request by Martin Renou added support for Comms in the Pyolite kernel, which enabled many of the existing core and third-party Jupyter Widgets to work out of the box in JupyterLite such as bqplot, ipyleaflet and ipycanvas. -->
Martin RenouによるプルリクエストはPyoliteカーネルにおける通信サポートを追加し、bqplot, ipyleaflet, ipycanvasのような多くの既存のコアおよびサードパーティJupyterウィジェットをJupyterLiteですぐに使用できるようにしました。

![](https://miro.medium.com/max/1400/1*c1DIbxS6bZ7bHDcOTEIJfg.gif)
<!-- JupyterLite comes with support for Jupyter Widgets -->
Jupyterウィジェット対応を備えたJupyterLite

<!-- ### More than just Python -->
### 単なるPythonを超えて

<!-- JupyterLite makes it possible to have plenty of other kernels also running in the browser. For now, the default distribution includes a JavaScript and a p5 kernel: -->
JupyterLiteはその他多くのカーネルをブラウザ内で動作することを可能にしています。現在のところ、デフォルトのディストリビューションはJavaScriptとp5カーネルを含んでいます。

![](https://miro.medium.com/max/1400/1*5XFdmBdQWeONbRD3nuQe0A.png)
<!-- Multiple kernels are available in JupyterLite -->
JupyterLiteで使用可能な複数のカーネル 

<!-- Both the JavaScript and the p5 kernels run in an IFrame as the code execution sandbox. With the Jupyter display protocol, users can easily render custom animations in the browser: -->
Javascriptとp5カーネルの両方ともコード実行用のサンドボックスとしてIFrame内で実行されます。Jupyter displayプロトコルがあれば、簡単にブラウザ内でカスタムアニメーションをレンダリングすることが可能となります。

![](https://miro.medium.com/max/1400/1*kPYtSDP3aE5_M5AmDT-Uvw.gif)
<!-- The p5.js kernel in JupyterLite -->
JupyterLiteにおけるp5.jsカーネル

<!-- ### Highly Customizable -->
### 高度なカスタマイズ性

<!-- Just like many existing Jupyter tools, JupyterLite can easily be configured for custom needs. -->
既存のJupyterのツール群と同様に、JupyterLiteは必要に応じて簡単にカスタマイズできるように設定されています。

<!-- It supports the new JupyterLab prebuilt extension system added to the 3.0 release. Existing JupyterLab extensions can then easily be reused in JupyterLite too! -->
3.0のリリースでは新しいJupyterLabのprebuiltエクステンションシステムのサポートが追加されました。既存のJupyterLabのエクステンションは簡単にJupyterLiteでも再利用が可能です！

![](https://miro.medium.com/max/1400/1*69CnccBrufVEEU0wqE_f7Q.gif)
<!-- The JupyterLab Drawio extension running in JupyterLite -->
JupyterLiteで動作するJupyterLabのDrawioエクステンション

<!-- The in-browser server part of JupyterLite also follows a plugin-based approach.
The server is a Lumino application without a shell and registers multiple plugins such as kernels, the contents manager or the sessions service.  -->
JupyterLiteのブラウザ内におけるサーバー部もプラグインベースのアプローチに従っています。
そのサーバーはシェルを持たないLuminoアプリケーションであり、カーネルやコンテンツマネージャまたはセッションサービスのような複数のプラグインを登録します。

<!-- This plugin-based approach makes it very convenient for deployers and site administrators to swap a plugin for another one.  For instance, they might want to swap the default contents manager that stores notebooks and files in the browser local storage with another plugin that would save them on AWS S3 instead. -->
このプラグインベースのアプローチはデプロイ実行者やサイト管理者がプラグインを入れ替えるのにとても便利です。
例えば、ノートブックやファイルをブラウザ内のlocal storageに保管するデフォルトのコンテンツマネージャの代わりにAWS S3上に保存する他のプラグインへと変更することが挙げられます。

![](https://miro.medium.com/max/1400/1*f7viaaS4SYTBpjA6DTPsDQ.png)
<!-- Overview of the plugin-based architecture of JupyterLite -->
JupyterLiteにおけるプラグインベースのアーキテクチャの概要

<!-- The modularity and flexibility of JupyterLite make it possible to easily add new kernels. For example, the Basthon kernel uses a slightly different execution model than Pyolite.  -->
JupyterLiteのモジュール性と柔軟性は新しいカーネルを簡単に追加することを可能とします。例えば、BasthonカーネルはPyoliteとは少し異なる実行モデルを使用します。

<!-- It runs in the main UI thread so users can manipulate the main window DOM from within Python directly, while Pyolite runs in a Web Worker as a background thread.  -->
BasthonカーネルはユーザーがメインウインドウのDOMをPythonから直接操作できるようにメインUIスレッド内で動作します。一方でPyoliteはバックグラウンドスレッドとしてWeb Worker内で動作します。

<!-- Both approaches have pros and cons, and the JupyterLite plugin system lets extension authors have full control over their kernels. -->
両方のアプローチは利点もあれば欠点もあり、JupyterLiteのプラグインシステムはエクステンションの作成者にこれらのカーネルを超えた完全な制御を与えます。

<!-- A prototype for running Basthon in JupyterLite is being developed in the following repository: https://framagit.org/casatir/basthon-jupyterlab/ -->
JupyterLite内でBasthonを動作させるためのプロトタイプは https://framagit.org/casatir/basthon-jupyterlab/ のリポジトリで開発されています。

![](https://miro.medium.com/max/1400/1*mMmSo4GPx7rpxbMpyf-VAA.jpeg)
<!-- Basthon in JupyterLite -->
JupyterLiteにおけるBasthon 

<!-- ### Deploying JupyterLite -->
### JupyterLiteのデプロイ

<!-- JupyterLite can easily be deployed as a static website. That’s it, no server, no complicated setup, no scalability issue. Just a plain HTTP server to serve static files to users. -->
JupyterLiteは簡単に静的なウェブサイトとしてデプロイすることが可能です。
つまり、サーバーや、複雑なセットアップは不要であり、スケールに関する問題も起こりません。
ただユーザに静的ファイルを提供するための単純なHTTPサーバーがあれば良いのです。

<!-- This simple approach makes it possible to use a variety of options: nginx, Binder, GitHub Pages or GitLab Pages, Vercel, Netlify, and more. It can even be deployed to ReadTheDocs, which is where the default JupyterLite demo site is hosted and continuously updated. -->
このシンプルなアプローチは(デプロイ先として)様々な選択を行うことが可能となります、例えばnginx, Binder, GitHub Pages, GitLab Pages, Vercel, Netlifyなどが考えられます。
ReadTheDocsへとデプロイすることさえ可能であり、ReadTheDocsには(実際に)デフォルトのJupyterLiteのデモサイトがホストされ、継続的に更新されています。

<!-- Many of the deployment scenarios are already documented in https://jupyterlite.readthedocs.io/en/latest/deploying.html. There is also a demo template to easily deploy a custom JupyterLite website on GitHub Pages with a single click: https://github.com/jupyterlite/demo -->
https://jupyterlite.readthedocs.io/en/latest/deploying.html には多くのデプロイシナリオがすでに挙げられています。
https://github.com/jupyterlite/demo にはシングルクリックでGitHub PagesにカスタマイズされたJupyterLiteのウェブサイトを簡単にデプロイするためのデモテンプレートも存在しています。

<!-- Thanks to the work by Nicholas Bollweg in this pull request, JupyterLite now offers a jupyterlite command line tool to make custom deployments much more convenient. -->
Nicholas Bollwegのこのプルリクエストにおける仕事のおかげで、今やJupyterLiteはカスタムデプロイを更に便利にするためのjupyterliteコマンドラインツールを提供しています。

<!-- One of the goals of JupyterLite is to let anyone build their custom distribution with the set of plugins and extensions they would like to use. For now, it requires using the jupyterlite CLI, but we can imagine having a more user-friendly way of exporting a custom JupyterLite website. -->
JupyterLiteの目標の一つとして、使いたいプラグインやエクステンションのセットでカスタマイズしたディトリビューションを誰でもビルドできるようにする、ということがあります。
今のところjupyterlite CLIを用いることが必要ではありますが、カスタマイズされたJupyterLiteウェブサイトをエクスポートする方法の、更にユーザーフレンドリーなものを想像できます。

![](https://miro.medium.com/max/1400/1*LL_UkItjshAzsEEDB4cI2A.png)
<!-- A mock-up for the JupyterLite Exporter -->
JupyterLite Exporterに対するモックアップ

<!-- ### A wide range of use cases -->
### 幅広いユースケース

<!-- With the ease of deployment and the low barrier to entry, JupyterLite is an excellent fit for a wide range of use cases. -->
デプロイの容易さと低い参入障壁があれば、JupyterLiteは幅広いユースケースに対して素晴らしく適合します。

<!-- In the educational space, it simplifies access to teaching materials and computing environments. Teachers and students can focus on the content of their classes without worrying about server deployments and monitoring. -->
教育の場面では、教材と計算環境へのアクセスを単純化します。
教師と生徒はサーバーへのデプロイとモニタリングについて心配することなく授業の内容に集中することが可能となります。

<!-- With JupyterLite we also hope to enable the next wave of Jupyter users and make the whole ecosystem even more accessible to newcomers and the wider community. -->
JupyterLiteがあれば我々はJupyterユーザの次の波が起こることも期待でき、新規参入者や幅広いコミュニティが全てのエコシステムに更にアクセスしやすくなることが期待できます。

<!-- For simpler and smaller-scale projects, it could even help reduce the load on mybinder.org by having a “binderlite” version of JupyterLite deployed on a CDN. -->
より単純で小規模なスケールのプロジェクトに対しては、CDNにデプロイされたJupyterLiteの'binderlite'バージョンを作成することによってmybinder.orgの負荷を下げることもできると思われます。

<!-- ### Try it online -->
### オンラインで試してみよう

<!-- JupyterLite can easily be tested in a web browser using the following link: -->
JupyterLiteはウェブブラウザから次のリンクを辿ることで簡単に試すことができます。

https://jupyterlite.github.io/demo

![](https://miro.medium.com/max/1024/1*CMvcTaLSAD5A-WHCtnIFwA.png)

<!-- ### Try it locally -->
### ローカルで試してみよう

<!-- JupyterLite can also be used locally. First, install the CLI package with: -->
JupyterLiteはまたローカルで用いることもできます。
最初に、CLIパッケージを次のようにインストールしてください。

```sh
pip install --pre jupyterlite
```

<!-- Then, to build the JupyterLite website and serve it locally: -->
次に、JupyterLiteのウェブサイトをビルドしてローカルサーバで動作させてください。

```sh
jupyter lite init
jupyter lite build
jupyter lite serve
```

<!-- Check out the documentation for more information about the jupyterlite command-line tool: https://jupyterlite.readthedocs.io/en/latest/developer-guide.html -->
jupyterliteのコマンドラインツールについては https://jupyterlite.readthedocs.io/en/latest/developer-guide.html で詳細を確認してみてください。

<!-- ### Next steps -->
### 次のステップ

<!-- JupyterLite is still under active development, with a lot of improvements planned for the next iterations: -->
JupyterLiteはまだまだ活発に開発されている状態であり、多くの改善が次のイテレーションで計画されています:

<!--
* Improve tooling for authoring custom in-browser kernels, reusing the JupyterLab federated (prebuilt) extension system.
* Improve the package management story in Pyodide with mamba and the conda-forge infrastructure.
* Reuse the JupyterLite packages in other lab-based applications such as Voilà, Gator, and the Quetz Frontend.
* Provide more user-friendly tools to easily export a custom JupyterLite distribution.
-->
* JupyterLabのfederated (prebuilt) エクステンションシステムを再利用して、カスタムのブラウザー内カーネルを作成するためのツールの改良
* mambaとconda-forgeのインフラを活用することによるPyodideにおけるパッケージ管理の仕組みの改善
* VoilàやGator、Quetzフロントエンドといった他のlabベースのアプリケーション内でのJupyterLiteパッケージの再利用
* カスタムJupyterLiteディストリビューションを簡単にエクスポートできる、よりユーザーフレンドリなツールの提供

<!-- ### Getting involved -->
### 開発への関与

<!-- JupyterLite is under active development happening in: -->
JupyterLiteは活発な開発が下記で行われています。

<!-- * the main repository: https://github.com/jupyterlite/jupyterlite -->
<!-- * satellite repositories (kernels, demos) in the GitHub organization: https://github.com/jupyterlite -->
* メインリポジトリ: https://github.com/jupyterlite/jupyterlite
* GitHubオーガナイゼーション内におけるカーネルやデモなどの サテライトリポジトリ: https://github.com/jupyterlite

<!-- ### About the Author -->
### 著者について

<!-- Jeremy Tuloup is a Scientific Software Developer at QuantStack and a Jupyter Distinguished Contributor. Maintainer and contributor of JupyterLab, Voilà, and many projects within the Jupyter ecosystem. -->
Jeremy TuloupはQuantStackで働く科学技術ソフトウェア開発者でありJupyterへの功績が際だったコントリビュータです。
また、 JupyterLabやVoilà、Jupyterのエコシステムに含まれる多くのプロジェクトのメンテナかつコントリビュータでもあります。

<!-- ### Acknowledgments -->
### 謝辞

<!-- We would like to acknowledge the previous work and the contributors who have worked on exploring the idea of Python in the notebook before us: Jyve, the Iodide notebook, Basthon, and the p5 notebook.  -->
Jyve, Iodide notebook、Basthon、p5 notebookといった、私たちの前にノートブックにおけるPythonに関するアイデアの探求に邁進してきた過去の仕事やコントリビュータに感謝を示します。

<!-- It is also worth mentioning that similar projects exist outside of the Jupyter ecosystem, such as Observable and the Starboard Notebook. -->
またJupyterエコシステムの外部に存在する、ObservableやStarboard Notebookといった同様のプロジェクトについても言及する価値があります。

<!-- We are grateful to Nicholas Bollweg, Madhur Tandon, Martin Renou for their contributions to JupyterLite, to Roman Yurchak and team for the work on Pyodide, Romain Casati for developing the Basthon kernel. -->
Nicholas Bollweg, Madhur Tandon, Martin Renou に対してJupyterLiteへのコントリビューションに感謝します。
Roman Yurchakとそのチームに対してPyodideの成果に感謝します。
Romain Casatiに対してBasthonカーネルの開発について感謝します。


<!-- The work on JupyterLite by Jeremy Tuloup, Madhur Tandon, and Martin Renou was funded by QuantStack. -->
Jeremy Tuloup, Madhur Tandon, Martin Renou によるJupyterLiteの仕事はQuantStackによる支援を受けています。

![](https://miro.medium.com/max/1400/1*UY4k_mgLml9uvyo_EKMdFA.png)

<!-- Thanks to Martin Renou, David Brochart, and Sylvain Corlay -->
Martin Renou, David Brochart, Sylvain Corlayに感謝します。
