# Interactive visualization with Jupyter-book
## Hide Markdown
- https://jupyterbook.org/en/stable/content/components.html#dropdowns

1. Hide codes with the {toggle} directive. With the show tag the toggle will be open by default. For codes do another tab within the toggle directive for proper rendering of the codes.
    ```
        ```{toggle}
        :show:
        dropdown content
            codes ....
            ..... 
        ```
    ```

2. Hide codes with {dropdown} directive. With the show tag the toggle will be open by default. For codes do another tab within the toggle directive for proper rendering of the codes.For codes 
    ```
        ```{dropdown} my dropdown
        :show:
        dropdown content
            codes ....
            .....
        ```
    ```
    
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