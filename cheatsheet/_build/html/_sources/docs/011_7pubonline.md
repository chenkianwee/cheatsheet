# Publish your Book on Github

1. [Register a Github account](https://github.com)
2. Once registered. Create a new repository as shown ({numref}`Figure %s <create_repo>`).
    ```{figure} /_static/steps/create_repo.png
    :scale: 80%
    :name: create_repo

    Create a repository on Github
    ```
3. For people who are not comfortable with CMD terminal. [Download Github Desktop](https://desktop.github.com/). The rest of the steps assumed you are using github desktop.
4. Clone the repository you just created onto your local computer as shown in {numref}`Figure %s <clone_repo>`.
    ```{figure} /_static/steps/clone_repo.png
    :name: clone_repo

    Clone the repository from github using github desktop
    ```
5. Copy and paste your book folder into the local repository. Now on github desktop you will see all the changes. "Commit to main" and then "Push Origin". You will see your repository populated with all your files.
    ```{figure} /_static/steps/commit_git.png
    :name: commit_git

    Commit and sync the files onto the github repository
    ```
6. Install ghp-import to compile and host your book on Github-pages. Go to your anaconda environment and activate your jupyterbook environment as shown here {doc}`011_1installation`. Once you activated the environment, execute this command.
    ```
    pip install ghp-import
    ```
7. Once that is done. On your local computer, cd to your book folder (the folder should contain the _build directory) and execute the following command. This will automatically create a gh page branch of your repository.
    ```
    ghp-import -n -p -f _build/html
    ```
    ```{figure} /_static/steps/ghp_import.png
    :name: ghp_import

    Successfully executed ghp-import.
    ```
8. Go to the settings of your github repository. Turn on gh-pages, set your branch to gh-pages and folder to /root as shown in {numref}`Figure %s <gh_pages>`.
    ```{figure} /_static/steps/gh_pages.png
    :name: gh_pages

    Turn on the gh-pages settings on your repository.
    ```
