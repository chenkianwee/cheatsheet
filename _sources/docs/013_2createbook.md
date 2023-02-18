# Create a Book Template with Jupyter-book

1. Change your directory to where you want to store your book content.
    ```
    cd specify_your_dir_here/
    ```
2. Create a template book with this command.
    ```
    jupyter-book create mynewbook/
    ```
3. Go into the "mynewbook" folder. You will see the files shown below. {numref}`Figure %s <template_book>`
    <br/><br/>
    ```{figure} /_static/steps/template_book.png
    :scale: 100%
    :name: template_book

    Structure of a template book
    ```
4. Build the book with the following command. If the build is successful, you will see the message shown in {numref}`Figure %s <build_book>`.
    ```
    jupyter-book build mynewbook
    ```
    ```{figure} /_static/steps/build_book.png
    :scale: 100%
    :name: build_book

    Success message of a build
    ```
5. Go to mynewbook -> _build -> html -> index.html. Double click on the index.html. You will be able to view the book in your default browser as shown in {numref}`Figure %s <browse_book>`.
    <br/><br/>
    ```{figure} /_static/steps/browse_book.png
    :name: browse_book

    Successful build of a book viewed in the browser
    ```
