# JupyterLab
- https://towardsdatascience.com/full-deployment-of-jupyter-notebooks-on-a-server-including-tls-ssl-with-lets-encrypt-b30cc758881

Jupyter-book is able to generate interactive graphs with Jupyter notebook files. First of all we will need to be able to create and edit .ipynb files. Jupyter lab is a web-based coding environment. It can be used together with Jupyter-book to produce interactive web-based visualizations that will be very useful for communication of modeling results and collaborations.

## Jupyter lab for writing ipynb files
1. Install jupyter lab with the following command. Refer to https://jupyter.org/install for more information
    ```
    pip install jupyterlab
    ```

2. Launch jupyter lab with the command in the terminal
    ```
    jupyter lab
    ```

### Edit jupyter notebook with vscode
- https://code.visualstudio.com/docs/datascience/jupyter-notebooks

1. Activate the command palette(Ctrl + Shift + p), 'Python: Select Interpreter'. Select an interpreter that has jupyter lab installed on the environment.

2. Open a ipynb file. You can edit the file in vscode.

3. To execute the file you will need a running jupyter lab instance. Launch you jupyter lab and connect your vscode to the jupyter lab session.
    - you will need a url to connect to the jupyter lab session. When you launch ur jupyter lab session in the terminal. A url with token will be provided. You just have to copy and paste that url into the vscode.

