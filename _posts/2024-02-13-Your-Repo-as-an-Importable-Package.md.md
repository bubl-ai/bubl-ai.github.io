---
title: Your Repo as an Importable Package
date: 2024-02-13 12:00:00 -0
categories: [Repo, Setup]
---

If you've been following our previous post on creating a Docker image for development purposes, you likely spotted the pip install -e command in the Dockerfile. So, what's the deal with this command, and why is it a game-changer for our project?

The pip install -e command is your ticket to installing a Python package in "editable" mode. Instead of the typical method of copying the package's files to the site-packages directory, pip takes a different route – it either creates a symbolic link or adds the project directly to the Python path. In simpler words, this means you can tweak the source code of the package, and voila! Your changes take effect immediately in the installed package, no reinstallation needed.

Now, the presence of a setup.py file is your project's golden ticket, especially in the realm of creating packages for Machine Learning projects. This file is like the control center, handling configuration and metadata for packaging your ML code into a distributable format, often taking the form of a Python package. It lets you specify dependencies, version information, and other crucial details essential for a smooth installation and usage process.

In a nutshell, doing this the right way allows you to treat your code just like any other Python library. The real magic happens when you realize you can seamlessly manage your source control and develop your code within the container. Everything neatly in the same place!

The next `setup.py` file was gotten from our [*llamaindex-project repository.*](https://github.com/bubl-ai/llamaindex-project/blob/main/setup.py)

```python
from setuptools import setup, find_packages

setup(
    name='llamaindex_project',
    version='0.1.0',
    author= 'Santiago Olivar',
    author_email='contact.bubl.ai@gmail.com',
    description='',
    long_description=open('README.md').read(),
    long_description_content_type='text/markdown',
    url='https://github.com/bubl-ai/llamaindex-project',
    license='MIT',
    classifiers=[
        'Development Status ::Planning',
        'Intended Audience :: Developers',
        'License :: OSI Approved :: MIT License',
        'Programming Language :: Python :: 3',
        'Topic :: Software Development :: Libraries :: Python Modules',
        'Operating System :: OS Independent',
    ],
    package_dir={'': 'bubls'}
)
```