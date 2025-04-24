# Why and how to publish a package in Python

#### How publishing a package enabled me to write better code

_This blog post is aimed for begineers to get an overview of creating and publishing python packages_

The first time I cloned a repository I felt intimidated by the number of files I could see in my directory. Primary reason for it being their dull names with foreign extensions. I did not bother to open all the files.

Eventually, I realized computer projects are a bunch of files grouped together each having their specific purpose. I like to think of it as an IKEA furniture set which needs assembly. Each piece is different and has its purpose.

A python package is similarly a group of files each having its own purpose. When publishing a package we need to create these files and configure them to successfully distribute our code.

I have a codebase containing helper function to solve CSES problems. Before publishing the package it is imperative we create documentations for each function in the codebase. This is my current directory structure

```
cses-aid/
├── src/
│   └── cses_aid/           
│       ├── helpers.py          

```
Open up the repository to see the contents of the helper.py file [CSES-AID](https://github.com/effaf/cses_aid). I chose the NumPy style documentation for this package since it is suitable for math like functions. There are different types of documentation styles. Pick the one appropiate to your package. After completing the documentation. Let's set up two tools which will help format our code before each commit. 

Flake8 - this is a linter which points the lines breaking the style guide. It does not modify the code.
Black - this is a code formatter which changes your code to follow the style guide. It is one of my favorite packages and is widely appreciated.


Install flake8, black, and pre-commit
` pip install flake8, black, pre-commit`

Create `.flake8` file in the root directory and configure it. Here are the contents of my flake8 file. It defines the rules to ignore and the files to exclude from linting.
<details>
<summary><code> .flake8 </code></summary>

```
[flake8]
ignore = E302, F401, E501, F841, F821, E226, E203
exclude = 
    .git,
    __pycache__,
    build,
    dist,
    env,
    venv,
    .venv,
    migrations,
    requirements.txt,
    .pre-commit-config.yaml,
    tests/
```
</details>
Create `.pre-commit-config.yaml` file in your root. Pre commit files define hooks on your commit. When you commit they automatically run and prevent the commit if there is an error. Think of them as guardrails for commits. 

<details>
<summary><code> .pre-commit-config.yaml</code></summary>

```
repos:
  - repo: https://github.com/psf/black
    rev: 25.1.0
    hooks:
      - id: black

  - repo: https://github.com/PyCQA/flake8
    rev: 7.1.0
    hooks:
      - id: flake8
```
</details>

We are now ready to pack our code into a package!

Before we start creating files for our package we need to cover a bit theory relating to packages. To begin, lets distinguish between Module and Package.

Speaking in context of files, module is a single file while a package is a group of files. Module contains all the python code - functions and classes. Package is a directory which contains the module file and a few other files each serving its purpose (more on this in a bit). Traditionally it was mandatory for the package directory to contain `__init__.py` file, however from python 3.3 we can create namespace packages which don't require the `__init__.py` file.

Front end tools such as `pip` and `uv` are packet managers which cannot build our code for distributions. We need other packages to help pack our code for distribution. Here are a few - Hatchling, setuptools, and Flit. These are called build backend.

For creating a package we need to create multiple files in the directory. Lets create and understand the purpose for each of them.

<details>
<summary><code> LICENSE </code></summary>
<br>

As package authors we need to choose the license for our code. There are several licenses available for open source code. You can read more on it [here](https://choosealicense.com/licenses/). For this code I am using the MIT license because it allows distribution and no modifications need not be revelead.
Contents of LICENSE file -

```
MIT License

Copyright (c) 2024 CSES-AID Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE. 
```

</details>

<details>
<summary><code> CONTRIBUTING.md </code>
<br>

This file is not mandatory, however it is a good practice to creeate a guide for contributors to your package. For a package in nascent changes you can scaffhold the contributing file from chatGPT and edit it to your use case.

Content of the CONTRIBUTING.md file

```
# Contributing to CSES-AID

Thank you for chosing to contribute to CSES-AID! This document provides guidelines and instructions for contributing to this project.

## How to Contribute

1. **Fork the Repository**
   - Click the "Fork" button on the top right of the repository page
   - Clone your forked repository to your local machine

2. **Create a Branch**
   - Create a new branch for your feature or bugfix
   - Use a descriptive name (e.g., `feature/add-new-algorithm` or `fix/bug-description`)

3. **Make Changes**
   - Follow the existing code style and conventions
   - Write clear, concise commit messages
   - Add tests for new features or bugfixes
   - Update documentation as needed

4. **Submit a Pull Request**
   - Push your changes to your forked repository
   - Create a pull request to the main repository
   - Provide a clear description of your changes
   - Reference any related issues


## Pull Request Process

1. Ensure your code follows the project's style guidelines
2. Update the documentation if necessary
4. Submit your pull request with a clear description
5. Be responsive to feedback and requested changes

## Questions?

If you have any questions about contributing, feel free to:
- Open an issue in the repository

```

</details>

<details>
<summary><code> README.md</code></summary>
<br>

Read me files provide an introduction to the package. Give the description, installation example of using the package. 
Contents of the readme.md file

```
# CSES Helper functions

This package provides utility functions for solving cses problems.

While solving CSES problems, often times we would need the same functions. This package aims to provide helper
functions to address the problem.

## Installation

pip install cses-aid

## Usage


from cses-aid import prefix_sums, sieve

print(sieve(100)) # print the first 100 prime numbers


## Contribution

    Refer to CONTRIBUTING.md for all the information

## Contact
Please create an issue or hit me up if you spot an improvement! <br>

[Email](shlok.kothari@gmail.com)

```

</details>

<details>
<summary><code> pyproject.toml </code></summary>
<br>

This is the main configuration file. Tradionally, it was required we create couple of files to configure our package. However, after PEP20 we only need to create `pyproject.toml` file and configure our package. Think of PEP20 as a new feature added to Python.
This is the file containing the information for building the package. 
Since we are using setuptools, its documentation is the best guide to follow. - [Creating pyproject.toml](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html)

This is the contents of my `pyproject.toml` file. It contains the configuration, metadata, and internal information.

```
[build-system]
requires = ["setuptools >= 77.0.3"]
build-backend = "setuptools.build_meta"

[project]
name = "cses-aid"
version = "0.0.1"
description = "A collection of helper functions for competitive programming and algorithm problem solving"
readme = "README.md"
requires-python = ">=3.8"
license = "MIT"
keywords = ["competitive-programming", "algorithms", "data-structures", "cses"]
authors = [
    { name = "Shlok Kothari" }
]
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Education",
    "License :: OSI Approved :: MIT License",
    "Topic :: Education",
    "Topic :: Software Development :: Libraries :: Python Modules",
]

[project.urls]
"Homepage" = "https://github.com/effaf/cses-aid"
"Documentation" = "https://github.com/effaf/cses-aid#readme"

[tool.pytest.ini_options]
testpaths = ["src/cses-aid/tests"]
python_files = ["test_*.py"]
addopts = "-v --cov=src/cses-aid --cov-report=term-missing"

[tool.black]
line-length = 88
target-version = ['py38']
include = '\.pyi?$'
```

</details>

<details>
<summary><code> __init__.py </code></summary>
<br>

This file is at the same level of our module. As mentioned earlier this file is not required in namespace packages.

- The `__init__.py` file turns a directory into a Python package.

- It runs when you import the package.

- You can use it to:

  - Expose certain functions/classes at the top level of your package.

  - Run initialization code (e.g., setup logging, imports, etc.).

  - Control what gets imported with wildcard imports (from my_package import *) using the __all__ list.

If you don’t include specific imports in `__init__.py`, you’ll have to import modules directly. For example

1. Without importing modules in `__init__.py`

```
import my_package

my_package.module_a.func_a()  # You need to access functions via their module names.
my_package.module_b.func_b()  

```

2. Importing modules inside `__init__.py`

Modify `__init__.py`

```
from .module_a import func_a
from .module_b import func_b
```

Now you can do:

```
import my_package

my_package.func_a()  # Works directly
my_package.func_b()  
```
- Functions are pulled up into the package’s namespace using from .module_x import func_x.

- No need to specify the module.

It is a good practice to avoid “polluting” your namespace by importing too much into `__init__.py` — only expose what’s necessary. While importing other packages it is recommended to import specific functions and not the whole package for the same reasons. 

We can also talk about the concept in context of top-level functions. Top level functions are accessible directly from the package’s root namespace.

Think of it this way:
```
# Top-level:
from my_package import func_a
func_a()

# Not top-level:
from my_package.module_a import func_a
```
So when you say "top-level functions", you mean:

- Functions that are exposed directly through the package, without drilling into submodules.

To make a function top-level, you pull it up via `__init__.py`:

```
# __init__.py
from .module_a import func_a
```
This adds func_a to my_package's namespace.

We can use `__all__` to control what gets imported using *

If someone uses:
```
from my_package import *

```
You can control what gets imported using `__all__`:

```
# __init__.py
from .module_a import func_a
__all__ = ["func_a"]
```
Otherwise, nothing gets imported by default with wildcard imports

I want to pull up all the functions in module to top level and make all of them available using the wildcard (*). Here are the contents of the file

```
from .helpers import (
    binary_search,
    prefix_sums,
    is_palindrome,
    fast_input,
    sieve,
    gcd,
    bfs,
    dfs,
    count_bits,
    mod_exp    
)

__all__ = ["binary_search",
    "prefix_sums",
    "is_palindrome",
    "fast_input",
    "sieve",
    "gcd",
    "bfs",
    "dfs",
    "count_bits",
    "mod_exp"]

```
</details>

We have created all the files required to publish a package. You can have other files in the package. This is the file structure

```
cses-aid/                          # Root directory of the project
├── src/                           # Source code folder
│   └── cses_aid/                  # Main Python package
│       ├── __init__.py            # Initializes the cses_aid package (top-level functions)
│       └── helpers.py             # Helper functions (the module)
├── tests/                         # Unit tests
│   └── test_helper.py             # Tests for helpers.py
├── README.md                      # Project overview and usage
├── CONTRIBUTING.md                # Guidelines for contributing
├── LICENSE                        # Open source license
└── pyproject.toml                 # Project configurations
```

We need to build and publish the package now. Create an account on PyPI, and add the API token to your environment.

Install required tools to build the package:

```
pip install build
```

Then run:
```
python -m build
```

This creates:

```
dist/
├── cses_aid-0.1.0.tar.gz
└── cses_aid-0.1.0-py3-none-any.whl
```

Upload it to PyPI

Install twine if you haven’t:

```
pip install twine

```
Then upload:
```
twine upload dist/*
```

We have published our package! Share it with your network!