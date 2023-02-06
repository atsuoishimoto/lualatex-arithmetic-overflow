# Reproduce lualatex Arithmetic overflow

Building this document causes Arithmetic overflow error with lualatex:

```sh
$ cat source/index.rst
1111111111111
#############

2222222222222
*************

3333333333333
=============

4444444444444
-------------

:class:`555555555555555`
^^^^^^^^^^^^^^^^^^^^^^^^

+-------------+------+
| 0000000     | AAAA |
|             +------+
|             | BBBB |
+-------------+------+

$ cat source/conf.py
project = 'test1'
copyright = '2023, a'
author = 'a'

extensions = []

templates_path = ['_templates']
exclude_patterns = []

language = 'ja'

html_theme = 'alabaster'
html_static_path = ['_static']

latex_engine="lualatex"

latex_docclass = {
'manual': 'ltjsbook',
'howto': 'ltjsarticle'
}


$ make latexpdf
...
...
...
! Arithmetic overflow.
\sphinx@@multirow ... \divide \count@ \count \tw@
                                                  \advance \dimen@ -\count@ ...

l.64 \end{tabulary}
```

## Environment

I reproduced the error on Ubuntu and macOS.

Ubuntu:

```sh
$ sphinx-build --version
sphinx-build 6.1.3

$ lualatex --version
This is LuaHBTeX, Version 1.14.0 (TeX Live 2022/dev/Debian)

$ uname -a
Linux u2204 5.15.0-58-generic #64-Ubuntu SMP Thu Jan 5 11:43:13 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```

macOS:
```sh
$ sphinx-build --version
sphinx-build 6.1.3

$ lualatex --version
This is LuaHBTeX, Version 1.15.0 (TeX Live 2022)

$ uname -a
Darwin mbp16.local 21.4.0 Darwin Kernel Version 21.4.0: Mon Feb 21 20:34:37 PST 2022; root:xnu-8020.101.4~2/RELEASE_X86_64 x86_64
```

## How to reproduce

1. Clone git repository:

```sh
git clone git@github.com:atsuoishimoto/lualatex-arithmetic-overflow.git
cd lualatex-arithmetic-overflow
```


2. Install [Sphinx](https://www.sphinx-doc.org/en/master/) and build.

```sh
pip3 install sphinx
cd sphinx-error/
make latexpdf
```

Alternatively, I created a minimum TeX file to reproduce the error. To build PDF manually:

```sh
cd latex
make library.pdf
```


