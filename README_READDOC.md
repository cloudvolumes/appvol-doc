## Documenting With ReadDoc
ReadDoc is another option for working with documenting

Template
---

This will solve your problem of where to start with documentation,
by providing a basic explanation of how to do it easily.

Features
--------

- Be awesome
- Make things faster

Installation
------------
 - Install Python >= 2.7 [https://www.python.org/downloads/](https://www.python.org/downloads/)
 - Install PIP packaging system for python, `sudo easy_install pip`
 > latest python packages comes with pip by default
 - Use virtualwrapper to put packages into virtual environment this is similar to rvm or nvm [https://virtualenvwrapper.readthedocs.io/en/latest/](https://virtualenvwrapper.readthedocs.io/en/latest/)
 - Use below command to setup environment
 > workon appvoldoc   
 > pip install -r requirement.txt  
 > make html
 >> windows user can use `make.bat` file
 - Wallah!!! That's it.  
 This will generate htmls to `build/html` directory.
 - There are few more options available in make which allows to generate pdf, json, epub, etc...
 ```
make
Sphinx v1.5.5
Please use `make target' where target is one of
  html        to make standalone HTML files
  dirhtml     to make HTML files named index.html in directories
  singlehtml  to make a single large HTML file
  pickle      to make pickle files
  json        to make JSON files
  htmlhelp    to make HTML files and an HTML help project
  qthelp      to make HTML files and a qthelp project
  devhelp     to make HTML files and a Devhelp project
  epub        to make an epub
  latex       to make LaTeX files, you can set PAPER=a4 or PAPER=letter
  latexpdf    to make LaTeX and PDF files (default pdflatex)
  latexpdfja  to make LaTeX files and run them through platex/dvipdfmx
  text        to make text files
  man         to make manual pages
  texinfo     to make Texinfo files
  info        to make Texinfo files and run them through makeinfo
  gettext     to make PO message catalogs
  changes     to make an overview of all changed/added/deprecated items
  xml         to make Docutils-native XML files
  pseudoxml   to make pseudoxml-XML files for display purposes
  linkcheck   to check all external links for integrity
  doctest     to run all doctests embedded in the documentation (if enabled)
  coverage    to run coverage check of the documentation (if enabled)
```

Contribute
----------

- Do not touch conf.py & index.rst unless it's require

Support
-------

If you are having issues, please let me know.
