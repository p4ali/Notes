## [Python package is a collection of modules](https://stackoverflow.com/questions/7948494/whats-the-difference-between-a-python-module-and-a-python-package)

* A `module` is a single Python file
* A `package` is a directory of Python modules containing an additional `__init_.py` file - this distinguish a package from a dir that just happens to contain a bunch of Python scripts.
* `package` can be nested to any depth, provided that the corresponding dirs contain their own `__init__.py` file.
* when you import a module or a pakcage, the corresponding object created by Python is alwys `module`
* When import a package, only variables/funcitons/classes in the `__init__.py` file of that package are directly visible., not sub-packages or modules.

In Python there are also built-in modules, such as `sys`, which is writen in C.

Dash `-` is NOT allowed in package name.
