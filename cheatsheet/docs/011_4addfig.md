# Add Figure in your book

1. Add a new folder "_static" in the book folder as shown in {numref}`Figure %s <add_static>`.
    <br/><br/>
    ```{figure} /_static/steps/add_static.png
    :name: add_static

    Add a _static folder
    ```
    a. Put a afigurefile.png file into the folder as shown in {numref}`Figure %s <add_figure>`.
    <br/><br/>
    ```{figure} /_static/steps/add_figure.png
    :name: add_figure

    Add a figure in the _static folder
    ```
2. Go to the "newcontent.md" file. In the file add the following line to add the figure into the page as shown in {numref}`Figure %s <add_command>`. To scale the figure, you can either use the scale or width command. The width command will give lesser distortion to a image.
    <br/><br/>
    ````
    ```{figure} /_static/afigurefile.jpg
    :name: add_figure
    width: 50%
    Add a figure in the _static folder
    ```
    ````
    ```{figure} /_static/steps/add_figcommand.png
    :name: add_command

    Add a the command in the "newcontent.md" file
    ```
    a. Build the book. You can see the result on your browser.
    ```{figure} /_static/steps/fig_res.png
    width: 50%
    :name: fig_res

    Result of adding the figure onto the page
    ```
3. You can reference the figure in your text with the following command in the "new_content.md" file as shown in {numref}`Figure %s <add_ref>`.
    <br/><br/>
    ````
    {numref}`Figure %s <add_figure>`
    ````
    ```{figure} /_static/steps/add_refcommand.png
    :name: add_ref

    Reference the figure in your text
    ```
    a. Build the book. You can see the result in your browser.
    ```{figure} /_static/steps/ref_res.png
    :name: ref_res

    Referencing your figure in your text.
    ```
