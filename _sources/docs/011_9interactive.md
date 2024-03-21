# Interactive visualization with Jupyter-book
## Hide Markdown
- https://jupyterbook.org/en/stable/content/components.html#dropdowns

1. Hide codes with the {toggle} directive
    ```
        ```{toggle} my dropdown
        dropdown content 
        ```
    ```

## Project Jupyter
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

## Plotly for generating interactive visualization on Jupyter-book
- you can install plotly by following these instructions https://plotly.com/python/getting-started/
- refer to jupyter-book documentation here (https://jupyterbook.org/en/stable/interactive/interactive.html)
1. Install plotly
    ```
    pip install plotly==5.20.0
    ```
2. Refer to various example scripts to generate interactive graphs (https://jupyterbook.org/en/stable/interactive/interactive.html#plotly)

3. Load require.js into your jupyter-book to show the graphs. Include the below settings to your _config.yml
    ```
    sphinx:
        config:
            html_js_files:
            - https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js
    ```

4. A error might be generated with showing plotly interactive plots. You can suppress the error with this command.
    ```
    sphinx:
        config:
            suppress_warnings: ["mystnb.unknown_mime_type"]
    ```

## Hide or remove code cell content
- https://jupyterbook.org/en/stable/interactive/hiding.html#hide-code-cell-content
- for jupyter lab or jupyter notebook refer to this guide (https://jupyterbook.org/en/stable/content/metadata.html#jupyter-cell-tags)

1. On vscode editor. Click on the code cell, click on ... (more actions) -> Add Cell Tag. Then iinput the following tags to hide or remove the cell.
    - hide-input: hide the input code cell
    - hide-output: hide the output code cell
    - hide-cell: hide both the input and output
    - remove-input: remove the code cell
    - remove-ouput: remove the output
    - remove-cell: remove both input and output