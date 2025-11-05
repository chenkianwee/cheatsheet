# Edit the Book

1. Use a text editor to edit the files. I am using atom. [Download Atom here](https://atom.io/)
2. Open Atom and add the mynewbook folder. {numref}`Figure %s <atom_initial>`
    <br/><br/>
    ```{figure} /_static/steps/atom_initial.png
    :name: atom_initial

    The open screen of Atom
    ```
3. Once added you will be able to see the structure of the book in {numref}`Figure %s <atom_folder>`.
    <br/><br/>
    ```{figure} /_static/steps/atom_folder.png
    :name: atom_folder

    The folder structure of the book in Atom IDE
    ```
4. To change the title of the book, go to the "_config.yml" file. Change the title of the book from "My sample book" -> "My notebook". Change the author to your name ({numref}`Figure %s <atom_config>`). Save the change. Build the book with the command from the 4th Step of [](011_2createbook).
    <br/><br/>
    ```{figure} /_static/steps/atom_config.png
    :name: atom_config

    Editing the _config.yml file
    ```
    a. The result of the change ({numref}`Figure %s <title_change>`).
    <br/><br/>
    ```{figure} /_static/steps/browse_title_change.png
    :name: title_change

    Result from editing the _config.yml
    ```
5. Next I will make changes to the _toc.yml. This file controls the table of content on the left side of the screen. I will add a new file and some content to the file.
    <br/><br/>
    a. I add a new file called newcontent as shown in {numref}`Figure %s <edit_toc>`. Remember to save the change and then build the book again.
    <br/><br/>
    ```{figure} /_static/steps/edit_toc.png
    :name: edit_toc

    Editing the _toc.yml file
    ```
    b. With Atom add a file called "newcontent.md" as shown in {numref}`Figure %s <new_content>`. Add in the title "# My new content".
    <br/><br/>
    ```{figure} /_static/steps/new_content.png
    :name: new_content

    Create a newcontent.md file
    ```
    c. Build the book. You will be able to see the change as shown in {numref}`Figure %s <toc_res>`
    ```{figure} /_static/steps/toc_res.png
    :name: toc_res

    Result of adding a newcontent.md file
    ```
