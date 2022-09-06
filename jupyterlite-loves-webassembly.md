# JupyterLite:Jupyter💗WebAssembly💗Pythonの日本語訳

この記事は[JupyterLite:Jupyter💗WebAssembly💗Python](https://blog.jupyter.org/jupyterlite-jupyter-%EF%B8%8F-webassembly-%EF%B8%8F-python-f6e2e41ab3fa)の日本語翻訳記事です。
typoや翻訳ミスを見つけましたら[こちらのリポジトリ](https://github.com/PyDataOsaka/jupyterblog-translation)のissueから報告いただくか、当該記事に対応したmdファイルに対してプルリクエストを発行していただければと思います。

## JupyterLite:Jupyter💗WebAssembly💗Python

JupyterLite is a JupyterLab distribution that runs entirely in the web browser, backed by in-browser language kernels.

![](https://miro.medium.com/max/1024/1*CMvcTaLSAD5A-WHCtnIFwA.png)

### Motivation

JupyterLite is a reboot of several attempts at making a full static Jupyter distribution that runs in the browser, without having to start the Python Jupyter Server on the host machine, usually done by running jupyter lab or jupyter notebook in a terminal.

The goal of the project is to provide a lightweight computing environment accessible in a matter of seconds with a single click, in a web browser, and without having to install anything on the end-user device.

With in-browser distributions, there is no need to provision the execution environment in the backend. Since the application is mostly a set of static files, it scales more easily, and it is also easier to deploy.

![](https://miro.medium.com/max/1400/1*bR--bXsuiMIDz-D_V4BfLw.png)
JupyterLite running in the browser as a static website on ReadTheDocs

### A full JupyterLab distribution running in the browser

JupyterLite is built from the ground up, reusing many JupyterLab plugins and components as is.

In addition to JupyterLab, JupyterLite also includes the RetroLab interface by default:

![](https://miro.medium.com/max/1400/1*B1Se7Fe1JXBt2a1NjwRmxg.png)
JupyterLite with the RetroLab interface

By reusing JupyterLab components, JupyterLite benefits from many of the upstream improvements such as new features, accessibility fixes, and upkeep improvements. The recent work on real time collaboration coming in JupyterLab 3.1 and championed by Kevin Jahns, Carlos Herrero, and Eric Charles can be enabled in JupyterLite too!

![](https://miro.medium.com/max/1400/1*HwMx3Fd6iICkWjjvUCtvwA.gif)
Real Time Collaboration with JupyterLite on ReadTheDocs

### Pyolite, a Python kernel backed by Pyodide

Pyodide consists of the CPython 3.8 interpreter compiled to WebAssembly which allows Python to run in the browser. Many popular scientific Python packages have also been compiled and made available. In addition, Pyodide can install any Python package with a pure Python wheel from the Python Package Index (PyPI). Pyodide also includes a comprehensive foreign function interface that exposes the ecosystem of Python packages to JavaScript and the browser user interface, including the DOM, to Python.

![](https://miro.medium.com/max/1400/1*usFUmvzRo6v8TuRFQhK0uw.png)
Pyodide: Python with the scientific stack, compiled to WebAssembly

Currently at version 0.17, Pyodide has benefited from many improvements over the past years: smaller binary size, support for asyncio, and better type translations between Python and JavaScript.

JupyterLite ships by default with Pyolite, a Python kernel backed by Pyodide. Pyolite runs in a Web Worker and thus doesn’t block the main UI thread when intensive computations are executed.

### IPython in the browser

Thanks to the work by Madhur Tandon in this pull request, Pyolite is now powered by IPython. This provides access to magics, code completion, rich display, interactive widgets, and many other features.
![](https://miro.medium.com/max/1400/1*2WBnuwRGkASGVNYUGjyPHg.gif)
Using IPython in JupyterLite

### Interactive Visualization

Many visualizations libraries such as Altair and Plotly are also supported in JupyterLite, allowing for quick and convenient figures and plots right in the browser:

![](https://miro.medium.com/max/1400/1*SRr162bkckWsmoSMxVd2uQ.gif)
Using Altair in JupyterLite

![](https://miro.medium.com/max/1400/1*w7Y4wRz9h2OFMgm5MfdD0A.gif)
Creating Plotly figures in JupyterLite

### Support for Jupyter Widgets

Jupyter Widgets rely on the Custom Messages specification of the Jupyter Protocol to send messages back and forth between the kernel and the frontend. This pull request by Martin Renou added support for Comms in the Pyolite kernel, which enabled many of the existing core and third-party Jupyter Widgets to work out of the box in JupyterLite such as bqplot, ipyleaflet and ipycanvas.

![](https://miro.medium.com/max/1400/1*c1DIbxS6bZ7bHDcOTEIJfg.gif)
JupyterLite comes with support for Jupyter Widgets

### More than just Python

JupyterLite makes it possible to have plenty of other kernels also running in the browser. For now, the default distribution includes a JavaScript and a p5 kernel:

![](https://miro.medium.com/max/1400/1*5XFdmBdQWeONbRD3nuQe0A.png)
Multiple kernels are available in JupyterLite

Both the JavaScript and the p5 kernels run in an IFrame as the code execution sandbox. With the Jupyter display protocol, users can easily render custom animations in the browser:

![](https://miro.medium.com/max/1400/1*kPYtSDP3aE5_M5AmDT-Uvw.gif)
The p5.js kernel in JupyterLite

### Highly Customizable

Just like many existing Jupyter tools, JupyterLite can easily be configured for custom needs.

It supports the new JupyterLab prebuilt extension system added to the 3.0 release. Existing JupyterLab extensions can then easily be reused in JupyterLite too!

![](https://miro.medium.com/max/1400/1*69CnccBrufVEEU0wqE_f7Q.gif)
The JupyterLab Drawio extension running in JupyterLite

The in-browser server part of JupyterLite also follows a plugin-based approach. The server is a Lumino application without a shell and registers multiple plugins such as kernels, the contents manager or the sessions service. This plugin-based approach makes it very convenient for deployers and site administrators to swap a plugin for another one. For instance, they might want to swap the default contents manager that stores notebooks and files in the browser local storage with another plugin that would save them on AWS S3 instead.

![](https://miro.medium.com/max/1400/1*f7viaaS4SYTBpjA6DTPsDQ.png)
Overview of the plugin-based architecture of JupyterLite

The modularity and flexibility of JupyterLite make it possible to easily add new kernels. For example, the Basthon kernel uses a slightly different execution model than Pyolite. It runs in the main UI thread so users can manipulate the main window DOM from within Python directly, while Pyolite runs in a Web Worker as a background thread. Both approaches have pros and cons, and the JupyterLite plugin system lets extension authors have full control over their kernels.

A prototype for running Basthon in JupyterLite is being developed in the following repository: https://framagit.org/casatir/basthon-jupyterlab/

![](https://miro.medium.com/max/1400/1*mMmSo4GPx7rpxbMpyf-VAA.jpeg)
Basthon in JupyterLite

### Deploying JupyterLite

JupyterLite can easily be deployed as a static website. That’s it, no server, no complicated setup, no scalability issue. Just a plain HTTP server to serve static files to users.

This simple approach makes it possible to use a variety of options: nginx, Binder, GitHub Pages or GitLab Pages, Vercel, Netlify, and more. It can even be deployed to ReadTheDocs, which is where the default JupyterLite demo site is hosted and continuously updated.

Many of the deployment scenarios are already documented in https://jupyterlite.readthedocs.io/en/latest/deploying.html. There is also a demo template to easily deploy a custom JupyterLite website on GitHub Pages with a single click: https://github.com/jupyterlite/demo

Thanks to the work by Nicholas Bollweg in this pull request, JupyterLite now offers a jupyterlite command line tool to make custom deployments much more convenient.

One of the goals of JupyterLite is to let anyone build their custom distribution with the set of plugins and extensions they would like to use. For now, it requires using the jupyterlite CLI, but we can imagine having a more user-friendly way of exporting a custom JupyterLite website.

![](https://miro.medium.com/max/1400/1*LL_UkItjshAzsEEDB4cI2A.png)
A mock-up for the JupyterLite Exporter

### A wide range of use cases

With the ease of deployment and the low barrier to entry, JupyterLite is an excellent fit for a wide range of use cases.

In the educational space, it simplifies access to teaching materials and computing environments. Teachers and students can focus on the content of their classes without worrying about server deployments and monitoring.

With JupyterLite we also hope to enable the next wave of Jupyter users and make the whole ecosystem even more accessible to newcomers and the wider community.

For simpler and smaller-scale projects, it could even help reduce the load on mybinder.org by having a “binderlite” version of JupyterLite deployed on a CDN.

### Try it online

JupyterLite can easily be tested in a web browser using the following link:

https://jupyterlite.github.io/demo

![](https://miro.medium.com/max/1024/1*CMvcTaLSAD5A-WHCtnIFwA.png)

### Try it locally

JupyterLite can also be used locally. First, install the CLI package with:

```sh
pip install --pre jupyterlite
```

Then, to build the JupyterLite website and serve it locally:

```sh
jupyter lite init
jupyter lite build
jupyter lite serve
```

Check out the documentation for more information about the jupyterlite command-line tool: https://jupyterlite.readthedocs.io/en/latest/developer-guide.html

### Next steps

JupyterLite is still under active development, with a lot of improvements planned for the next iterations:

* Improve tooling for authoring custom in-browser kernels, reusing the JupyterLab federated (prebuilt) extension system.
* Improve the package management story in Pyodide with mamba and the conda-forge infrastructure.
* Reuse the JupyterLite packages in other lab-based applications such as Voilà, Gator, and the Quetz Frontend.
* Provide more user-friendly tools to easily export a custom JupyterLite distribution.

### Getting involved

JupyterLite is under active development happening in:

* the main repository: https://github.com/jupyterlite/jupyterlite
* satellite repositories (kernels, demos) in the GitHub organization: https://github.com/jupyterlite

### About the Author

Jeremy Tuloup is a Scientific Software Developer at QuantStack and a Jupyter Distinguished Contributor. Maintainer and contributor of JupyterLab, Voilà, and many projects within the Jupyter ecosystem.

### Acknowledgments

We would like to acknowledge the previous work and the contributors who have worked on exploring the idea of Python in the notebook before us: Jyve, the Iodide notebook, Basthon, and the p5 notebook. It is also worth mentioning that similar projects exist outside of the Jupyter ecosystem, such as Observable and the Starboard Notebook.

We are grateful to Nicholas Bollweg, Madhur Tandon, Martin Renou for their contributions to JupyterLite, to Roman Yurchak and team for the work on Pyodide, Romain Casati for developing the Basthon kernel.

The work on JupyterLite by Jeremy Tuloup, Madhur Tandon, and Martin Renou was funded by QuantStack.

![](https://miro.medium.com/max/1400/1*UY4k_mgLml9uvyo_EKMdFA.png)

Thanks to Martin Renou, David Brochart, and Sylvain Corlay
