# Installation

1. Install the Anaconda Individual Edition
    <br/><br/>
    a. [Download the Anaconda Individual](https://www.anaconda.com/products/individual).  
    b. [Install the Anaconda Individual](https://docs.anaconda.com/anaconda/install/).
    <br/><br/>
2. Once Anaconda is installed. Create an environment called "jupyterbook" for installing the required libraries. This isolate all changes to within a Python environment.
    <br/><br/>
    a. Create an environment with the following command:
    ```
    conda create --name jupyterbook
    ```
3. Install Jupyter Book in the environment created.
    <br/><br/>
    a. Activate the environment with the following command:
    ```
    conda activate jupyterbook
    ```
    b. Install Jupyter-Book with the following command:
    ```
    pip install -U jupyter-book
    ```
