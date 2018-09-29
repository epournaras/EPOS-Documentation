# EPOS-Documentation

This repository contains the documentation for EPOS and I-EPOS projects.

The documentation is written in markup language called **reStructuredText**, often abbreviated as **reST**.

The link to Getting Started page can be found [here](https://docs.readthedocs.io/en/latest/getting_started.html).

The link to the syntax of writing in **reST** can be found [here](http://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html).

Why should we even use this and not Markdown language? [here](http://ericholscher.com/blog/2016/mar/15/dont-use-markdown-for-technical-docs/).

A 20-minutes video tutorial can be found [here](https://www.youtube.com/watch?v=oJsUvBQyHBs&feature=youtu.be).

# PREREQUISITES

`Sphinx` is a Python-based tool for generating this kind of documentation. Therefore, take the following steps:

1. Install python if it is not already installed on your machine. Ubuntu 16.04 comes with both python 2 and python 3 already.

2. Install `Sphinx` by typing this in your terminal:

```
sudo pip3 install sphinx sphinx-autobuild
```

3. Create a directory `docs` within your project, and enter this directory

4. `Sphinx` must be configured now. The following command will start the configuration process that is self-explanatory, with many defaults already set:

```
sphinx-quickstart
```

Note that you should accept automatic creation of make files since it will save you a lot of time.

5. The file which you should edit with the documentation is `index.rst`. It's location depends on wheather you accepted the defaults or not, but it's either in a `source` directory or directly in `docs` directory.

6. Once you are done editing `index.rst`, run the following command to create HTML documentation:

```
make html
```

Generated files are either in `build` directory or directly in `docs` directory, depending on wheather you chose the default settings.

# TODO:

- [ ] Reformat and reorganize the code
- [ ] Merge multi-objective aspects with the original code
- [ ] check all input parameters, make sure they work
- [ ] loggers for repetetive experiments and for single experiments
