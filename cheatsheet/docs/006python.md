# Python
## Python
install python for ubuntu https://computingforgeeks.com/how-to-install-python-on-ubuntu-linux/?expand_article=1
```
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.11
sudo apt install python3-pip
sudo apt install python3.11-venv
python3.11 -m venv /home/chenkianwee/venv/geomie3d
```

if your spyder give this error msg 
```
'Could not load the Qt platform plugin "xcb" in "" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.'
```

install these libraries to solve the problem https://stackoverflow.com/questions/62391587/qt-could-not-find-the-qt-platform-plugin-xcb 
```
sudo apt install make g++ pkg-config libgl1-mesa-dev libxcb*-dev libfontconfig1-dev libxkbcommon-x11-dev python libgtk-3-dev
```
create a virtual environment with Python
```
py -m venv c:\path\to\new\virtual\environment
```
activate environment for ubuntu
```
source bin/dir/activate
```
## pip
pip install
```
pip install <packagename==version>
```

pip install a local library in development
```
pip install -e /path/to/local/library
```

pip uninstall
```
pip uninstall <packagename==version>
```

pip upgrade package
```
pip install --upgrade <packagename>
```

list all pip install packages
```
pip list | grep package_name
```
```
pip list 
```

## Conda
create environment
```
conda create --name myenv python=3.9
```
remove an environment
```
conda remove --name myenv --all
```
update conda
```
conda update -n base -c defaults conda
```
remove cache and unused cache packages
```
conda clean -a
```

## Deploy Python Package to PYPI
This is written for a window system. Make changes accordingly to your OS.
based on these:
- https://packaging.python.org/en/latest/tutorials/packaging-projects/
- https://www.geeksforgeeks.org/command-line-scripts-python-packaging/
- include data packages:
    - first need to have a manifest file: https://python-packaging.readthedocs.io/en/latest/non-code-files.html
    - then setup the pyproject toml file: https://setuptools.pypa.io/en/latest/userguide/datafiles.html
    - how to access data file during runtime: https://setuptools.pypa.io/en/latest/userguide/datafiles.html#accessing-data-files-at-runtime
    
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
6. Upload your build to the testpypi with the following command. You will be asked to key in your token.
    ```
    twine upload --repository testpypi dist/* --verbose
    ```
    - Alternatively you can store your credentials in a .pypirc file in your home directory.
        ```
        [distutils]
        index-servers =
            pypi
            testpypi

        [pypi]
        username = __token__
        password = <PyPI token>

        [testpypi]
        username = __token__
        password = <PyPI token>
        ```
7. Install the library from testpypi with the following command
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
    pdoc -d numpy proj_directory --output-dir ./docs
    ```

## Framework for writing Python Apps
- https://beeware.org
- https://realpython.com/pyinstaller-python/#distribution-problems
- https://github.com/ClimenteA/flaskwebgui
- https://github.com/ClimenteA/pyvan

### Writing Python Apps with Beeware
- https://docs.beeware.org/en/latest/tutorial/tutorial-0.html

1. create a virtual environment using the ubuntu system python 
    ```
    python3 -m venv venv/beeware
    ```
    ```
    source venv/beeware/bin/activate
    ```

2. Install briefcase
    ```
    python -m pip install briefcase
    ```
3. create a briefcase tempplate 
    ```
    briefcase new
    ```
4. Put in the relevant pyqtgraph codes, and in the pyproject.toml file put in the requirements
5. create scaffolding, then build the app, run your app to test it out. Lastly, build your installer.
    ```
    briefcase create
    ```
    ```
    briefcase build
    ```
    ```
    briefcase run
    ```
    ```
    briefcase package
    ```
6. if you updated your requirement for third party library you need to do update
    ```
    briefcase update -r
    ```
    ```
    briefcase build
    ```
    ```
    briefcase run
    ```
7. Beeware web app
    - beeware will try to load the local linux python wheel which will not work with pyscript.
    - after it is compiled you need to edit build/helloworld/web/static/www/pyscript.toml
    - in the packages field put in geomie3d or numpy
    - then do briefcase run web
    
8. Openstudio sdk still do not have a wasm version.

9. You can download a wasm version of ifcopenshell here (https://github.com/IfcOpenShell/wasm-wheels)

### Changing the logo of the app
- based on this resource https://docs.beeware.org/en/latest/tutorial/topics/custom-icons.html
- at the pyproject.toml file specify the add icon line
    ```
    icon = "icons/myapp"
    sources = [
        "src/myapp",
    ]
    ```
- add the icon folder in your directory and put all the icons inside
    ```
    myapp/
    |--- CHANGELOG
    |--- LICENSE
    |--- pyproject.toml
    |--- README.rst
    |--- src/
    |--- tests/
    |--- icons/
        |--- myapp.ico
        |--- myapp-16.png
        |--- myapp-32.png
        |--- myapp-64.png
        |--- myapp-128.png
        |--- myapp-256.png
        |--- myapp-512.png

    ```
- use inkscape and gimp to produce the image. The .ico can be 256x256 px, and the rest can be 16 x 16 and so on ... In gimp you export to .ico 
- run the following command to update the resources
    ```
    briefcase update --update-resources
    ```
## Numpy and scipy resources
### Reducing numpy, scipy library size 
- https://towardsdatascience.com/how-to-shrink-numpy-scipy-pandas-and-matplotlib-for-your-data-product-4ec8d7e86ee4

### Indexing and slicing on numpy
- https://numpy.org/doc/stable/user/basics.indexing.html#


## VScode Python IDE
- https://code.visualstudio.com/docs/python/environments

### Setting up PYTHONPATH
- setting up Python path https://linuxpip.org/vscode-pythonpath/#google_vignette

1. in the .vscode folder create a file called .env and paste these lines in it
    ```
    PYTHONPATH=/home/usr/yourlibrary/src
    ```
2. create another file in the .vscode folder caled settings.json. And point the envfile variable to the .env file
    ```
    {"python.envFile": "/home/usr/workspace/.vscode/.env"}
    ```
3. Restart your vscode.

### VScode useful shortcut keys
- Selecting interpreter
    ```
    Ctrl + Shift + P, type in Python Interpretor and specify the path to the interpretor.
    ```
- See all keyboard shortcut
    ```
    Ctrl + k Ctrl + s
    ```
- create new window of current window
    ```
    ctrl + k
    ```
    - then press o
- activate hover hints
    ```
    Ctrl + k Ctrl + i
    ```
- go to terminal
    ```
    Ctrl + `
    ```
### Folding in VScode
- https://code.visualstudio.com/docs/editor/codebasics#_folding

- define fold region 
    ```
    #region
    #endregion
    ```
- fold
    ```
    ctrl + shift + [
    ```

- fold recursively
    ```
    ctrl + k ctrl + ]
    ```

- unfold
    ```
    ctrl + shift + ]
    ```

- unfold recursively
    ```
    ctrl + k ctrl + ]
    ```

- unfold a region
    ```
    ctrl + k ctrl + 9
    ```
- fold all
    ```
    ctrl + k ctrl + 0
    ```
- unfold all
    ```
    ctrl + k ctrl + j
    ```

- fold all regions
    ```
    ctrl + k ctrl + 8
    ```
    
## Generate xml file from xsd using Python
### Using generateDS.py to generate Python files from xsd
- https://stackoverflow.com/questions/48928973/python-create-xml-from-xsd
- the result was not very successful

1. pip install the library generateDS.py
    ```
    pip install generateDS
    ```
2. Once it is installed you will be able to access it in your virtual environment by typing 'generateDS.py'

3. Run the following command to generate python codes to write measure_xml
    ```
    generateDS.py -o osmod_measure.py -s osmod_measuresubs.py measure_v3.xsd 

    ```
### Using xmlschema
- https://stackoverflow.com/questions/58283389/create-specific-xml-file-from-xsd-file-with-python
- https://github.com/sissaschool/xmlschema/issues/395

1. An example script
    ```
    import xmlschema
    import json
    from xml.etree.ElementTree import ElementTree

    xsd_path = '/home/xsd/measure_v3.xsd'
    xml_path = '/home/measure.xml'
    res_path = '/home/measure_edited.xml'

    schema = xmlschema.XMLSchema(xsd_path)
    print(schema.is_valid(xml_path))
    d = schema.to_dict(xml_path, preserve_root=True)
    d['measure']['arguments'] = {'argument': [{'name': 'srf_temps',
                                    'display_name': 'Surface Temperatures',
                                    'description': 'Output the surface temperatures of each surface',
                                    'type': 'Boolean', 'required': True, 'model_dependent': False}]}
    json_data = json.dumps(d)
    xml = xmlschema.from_json(json_data, schema=schema, preserve_root=True, path = 'measure')
    ElementTree(xml).write(res_path)
    ```

## Execute Python with Crontab 
- https://stackoverflow.com/questions/8727935/execute-python-script-via-crontab
- https://www.adminschoice.com/crontab-quick-reference
- https://linuxize.com/post/cron-jobs-every-5-10-15-minutes/

## Python in the browser
- https://jupyter.org/
- https://pyodide.org/en/stable/
- https://realpython.com/pyscript-python-in-browser/
- https://medium.com/@andrewdass/how-to-start-using-pyscript-11036f998cef

### Tutorial
- https://docs.pyscript.net/2025.7.3/beginning-pyscript/
- uploading files - https://pyscript.recipes/latest/basic/file-upload/

### Jupyter lab shortcut key
- change cell to markdown
    ```
    Esc + m
    ```

- change cell to code
    ```
    Esc + y
    ```

## Panda3d for 3D game development
- https://github.com/ArsThaumaturgis/Panda3DTutorial.io

## Async io
- https://www.pythontutorial.net/python-concurrency/python-asyncio-create_task/
- https://www.slingacademy.com/article/python-using-async-await-with-loops/
- https://superfastpython.com/python-async-requests/

## Resources
- Computer Scientist website on alot of python resources and career advice (https://third-bit.com/)
- Useful online books on python: https://greenteapress.com/wp/
- research software engineering with python: https://alan-turing-institute.github.io/rse-course/html/index.html#