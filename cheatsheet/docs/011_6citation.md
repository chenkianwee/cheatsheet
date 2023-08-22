# Adding a Citation

1. You will need to have a "reference.bib" file in your book folder.
2. You can add a citation with this command
    ```
    {cite}`chen_exploring_2020`
    ```
3. You can generate the bibliography with this command. Remember to specify the right path to the .bib file. If the .bib file is in the previous folder specify it as "../references.bib"
    ````
    ```
    {bibliography} references.bib
    ```
    ````
## Example
I want to cite this article {cite}`chen_exploring_2020`.

## Bibliography
```{bibliography} ../references.bib
```
