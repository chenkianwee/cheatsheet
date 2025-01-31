# Reference to Other Documents, Hyperlink and Folder Structure

1. You can reference to other documents in the book by using this command.
    ````
    {doc}`020steps`
    ````
    - or like this if you like the visible text to be something else from the document title
      ```
      {doc}`text-that-will-appear <03_b3dplane>`
      ```
2. You can create a hyperlink with this command.
    ````
    [text that will appear in content](https://the-hyperlink.com)
    ````
3. You can move the documents to a "docs" folder for a cleaner folder structure. You just have to specify the files in the "_toc.yml" file as "docs/file". This is shown in {numref}`Figure %s <restructure_folder>`

    ```{figure} /_static/steps/restructure_folder.png
    :name: restructure_folder

    You can restructure your book folder as shown
    ```
4. create a hyperlink that opens a new tab.
    ````
    <a href="https://www.qgis.org/en/site/" target="_blank">here</a>
    ````

5. create part -> chapters -> section for the toc of your book.

    ````
    - file: docs/intro
      numbered: true
    - part: Concepts
      chapters:
        - file: docs/010/010gis
        - file: docs/010/011crs
    - part: Workflows
      chapters:
        - file: docs/020/020workflow1
    - part: Tasks
      chapters:
        - file: docs/030/030task1
        - file: docs/030/031task2
          sections:
            - file: docs/030/031task21
            - file: docs/030/031task22
        - file: docs/030/032task3
        - file: docs/030/033task4
    ````
6. To refer to sections in a document. Make sure to add the following into the config.yml.
  ```
  sphinx:
    config:
      myst_heading_anchors: 3
  ```
7. refer to section in a document.

    ```
    [a link to this section](#section)
    
    if your section heading have space replace it with hyphens. All letters in small cap.
    #read-and-write-text
    ```
    
8. refer to a section in another document using hyperlink. Check your _build folder structure to put in the html file to referece accordingly.

    ```
    if your section heading have space replace it with hyphens. All letters in small cap.
    [something](002linux.md#read-and-write-text)

    ../ for file outside of this folder
    ```