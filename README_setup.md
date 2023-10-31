# Setup & Usage {#sec-setup}

This repository uses the following technologies:

- `conda environments` (part of [Anaconda](https://www.anaconda.com/)) for creating and managing virtual environments

- [jupyter](https://jupyter.org/) notebooks for course materials combining rich-text and code

- [quarto](https://quarto.org) to generate the handbook from the course's notebooks and some extra `md` files.

## Setting up the environment

Virtual environments are a way to install all the dependencies (and their right version) required for a certain project by isolating python and libraries' specific versions. Every person who recreatesthe virtual environment will be using the same packages and versions, reducing errors and increasing reproducibility.

This project, uses a `conda` environment called `IM939`, which will install all the packages and versions (as well as their dependencies) as defined in the file `environment.yml` at the root of this project.

### Installing Anaconda

You will need to install [Anaconda distribution](https://www.anaconda.com) your machine if it is not already installed. You can run the following command to see if it is already present in your system:

``` bash
conda --help
```

If you get a `command not found` error, you will need to install Anaconda following [these instructions on their website](https://www.anaconda.com/distribution/)).

### Creating the virtual environment

::: callout-note

You only need to do this once in the same machine. After the virtual environment is created we can always update it to follow any change in the file `environment.yml`

:::

To recreate the virtual environment from `environment.yml`, run the following command:

``` bash
conda env create -f environment.yml
```

or, if we want to install the environment within the project:

``` bash
conda env create --prefix env -f environment.yml
```

### Activating the virtual environment

Once the environment has been created, it needs to be activated by typing the following command

``` bash
conda activate IM939
```

or, if it is stored in `env/` folder:

``` bash
conda activate env/
```

::: callout-important

#### Once per session

You will need to activate the environment every time you open your editor anew.

:::

**Deactivate virtual environment:**

If you want, you can always deactivate your environment (and, actually open the default one, called `base`) by running:

``` bash
conda deactivate
```

**Update virtual environment from `environment.yml`:**

``` bash
conda env update -f environment.yml
```

**Freeze used dependencies into a file**

We can create a file (in this case `environment.yml`) containing the exact libraries and versions used in the current environment. This can be useful to update the versions used in the environment in the future.

``` bash
conda env export > environment.yml
```

## Preventing commits with execution cells

This handbook relies on jupyter notebooks. Quarto renders any `*.ipynb` file into a handbook, and displays the output of any code block, according to the settings. Regretfully, that means that it executes every code cell and therefore, jupyter notebooks stores the results in the notebook too, which is not what we'd like to do.

To have clean notebooks (this is, without any executed cell) in an automated way, the following command must be run within the repository's root:

```bash
$ git config --local include.path ../.gitconfig
```

Please note that this command only needs to be run once: the first time we are setting up the repo. More information about the implemented solution here: <https://zhauniarovich.com/post/2020/2020-10-clearing-jupyter-output-p3/>


## Recreating the handbook

::: callout-tip

Quarto has extensive documentation at their website (specifically in this page about authoring document and this other on managing books).

:::

The workflow can be summarised as follows (detailed instructions below):

1.  Create new content or edit existing one

    1.  Create a `.md`, `.iypnb` or `.qmd` file and put it in the `/content/` folder
    2.  Add an entry to to the table of contents in `_quarto.yml` (more info in quarto

2.  Edit existing content in `/content/` folder

3.  Render book locally to see changes

4.  Commit & Push changes to the source documents in `/content/` folder

    ``` bash
    git commit -a -m "My fancy message"
    git push origin main
    ```

5.  Publish book online

### Rendering the book locally:

Rendering the book will convert jupyternotebooks, qmd files or md files listed in \_quarto.yml's table of contents into a a book, applying the styles and configurations from \_quarto.yml. Do this to preview how your changes would look like as a book. From the repo's root, run:

``` bash
quarto render
```

::: callout-warning

#### First time render

On its first run, this command will take several minutes to process. This is because quarto will run and execute the computations in every cell within every jupyter notebook, some of which are really time consuming. The good news, is that quarto will create a cached version of it (stored in the `/_freeze/` folder) , which means that further runs of `quarto render` will not need to execute the cells again (unless the original jupyternotebook is changed or the corresponding folder is deleted).

If you want the cache to be regenerated:

``` bash
quarto render --cache-refresh
```

:::

### Publishing book to github pages

This book is published using GitHub pages, and assumes that your rendered book will be located on a dedicated branch called `gh-pages` which needs to be created in the repo if not present already. Also, the repository needs to be configured as to use that branch's root for publishing a GithubPage.

Once `gh-pages`branch has been created, run the following command:

``` bash
quarto publish gh-pages   
```

This command will render the book in the branch gh-pages and will push it to the corresponding branch in our repo and then checking out again to the previous branch (usually, `main`). More info about it here: <https://quarto.org/docs/publishing/github-pages.html>

::: callout-note

## Other useful resources

-   Notebook embedding: https://quarto.org/docs/authoring/notebook-embed.html

:::
