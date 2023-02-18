# Deploy Python Package to PYPI
This is written for a window system. Make changes accordingly to your OS.
based on these:
- https://packaging.python.org/en/latest/tutorials/packaging-projects/
- https://www.geeksforgeeks.org/command-line-scripts-python-packaging/

1. Create a pyproject.toml in this project structure
    ```
    myproject/
    ├── LICENSE
    ├── pyproject.toml
    ├── README.md
    ├── src/
    │   └── myproject/
    │       ├── __init__.py
    │       └── example.py
    └── tests/
        |── myproject/
    ```
2. In the pyproject.toml setup the file in this structure.
    ```
    [build-system]
    requires = ["setuptools>=61.0"]
    build-backend = "setuptools.build_meta"

    [project]
    name = "myproject"
    version = "0.0.1"
    authors = [
      { name="wwww", email="wwww@gmail.com" },
    ]
    description = 'Python library'
    readme = "README.md"
    requires-python = "==3.11"
    classifiers = ["License :: OSI Approved :: GNU General Public License v3 (GPLv3)",
                   "Programming Language :: Python :: 3.11"]
    dependencies = ['numpy==1.23.4',
                    'scipy==1.9.3',
                    'sympy==1.11.1',
                    'pyqtgraph==0.13.1',
                    'PyOpenGL==3.1.6']

    [project.urls]
    "Homepage" = "https://wwww.com"
    "Bug Tracker" = "https://www.com"

    [project.scripts]
    raytracemrt = "raytracemrt.raytracemrt:main"
    ```
3. Make sure you have the build program installed.
    ```
    pip install --upgrade build
    ```
4. Run this command from the same directory where pyproject.toml is located.
    ```
    py -m build
    ```
5. Once successfully built, upload the files to PyPi with twine
    ```
    pip install --upgrade twine
    ```
6. You will need to specify your .pypirc file in your home directory.
    ```
    [distutils]
    index-servers =
        pypi
        testpypi

    [pypi]
    repository = https://upload.pypi.org/legacy/
    username = __token__
    password = <PyPI token>

    [testpypi]
    repository = https://test.pypi.org/legacy/
    username = __token__
    password = <PyPI token>
    ```
7. Upload your build to the testpypi with the following command.
    ```
    twine upload --repository testpypi dist/* --verbose
    ```
9. Install the library from testpypi with the following command
    ```
    pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE  
    ```
8. Upload your build with the following command.
    ```
    twine upload dist/* --verbose
    ```
## CI/CD with Github action
- https://www.asapdevelopers.com/python-for-ci-cd/
- https://ron.sh/how-to-submit-a-package-to-pypi/
- https://www.lambdatest.com/blog/jenkins-pipeline-tutorial/

1. Install Pytest to run test.
    ```
    pip install pytest
    ```
2. create an empty conftest.py file in the package you want to test. Create a tests folder in the package. Write the test in the test_example.py and place it in the tests folder.
    ```
    myproject/
    ├── LICENSE
    ├── pyproject.toml
    ├── README.md
    ├── src/
    │   └── myproject/
    │       ├── __init__.py
    │       └── example.py
    |       └── conftest.py
    └── tests/
        |── test_example.py
    ```
3. go to the directory of your Python package and run the following command
    ```
    pytest
    ```
4. Using github actions to automate the testing. Go to the tab github actions -> new workflow -> Python application. Use the python application template. Create a requirement.txt file in your package if your package requires third party libraries.
    ```
    myproject/
    ├── LICENSE
    ├── pyproject.toml
    ├── README.md
    ├── requirements.txt
    ├── src/
    │   └── myproject/
    │       ├── __init__.py
    │       └── example.py
    |       └── conftest.py
    └── tests/
        |── test_example.py
    ```
5. example of a requirements.txt file.
    ```
    numpy==1.23.4
    scipy==1.9.3
    ```
6. Any commits and the workflow will run. If there is an error you will be informed.

## Generating documentation for your library with Pdoc
- https://pdoc.dev/docs/pdoc.html#deploying-to-github-pages

1. Install Pdoc
    ```
    pip install pdoc
    ```
2. Generate documentation for your project by specifying the directory your project is in and outputting it to a directory. HTML files will be generated.
    ```
    pdoc proj_directory --output-dir ./docs
    ```
## Framework for writing Python Apps
- https://beeware.org
- https://realpython.com/pyinstaller-python/#distribution-problems
- https://github.com/ClimenteA/flaskwebgui
- https://github.com/ClimenteA/pyvan
