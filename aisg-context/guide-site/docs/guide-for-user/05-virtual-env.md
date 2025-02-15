# Virtual Environment

## Local Virtual Environments

While we will be making use of AI Singapore's remote infrastructure
to carry out some workflows, we can still make use of our local
machine to execute some of the steps of the end-to-end machine learning
workflow. Hence, we can begin by creating a virtual environment that
will contain all the dependencies required for this guide.

```bash
conda env create -f toxic-comments-severity-conda-env.yaml
```

## Creating Persistent `conda` Environments in the Workspace

While the Docker images you will be using to run experiments on Run:ai
would contain the `conda` environments you would need, you can also
create these virtual environments within your development environment,
and have it be persisted. The following set of commands allows you to
create the `conda` environment and store the packages within your own
workspace directory:

- First, have VSCode open the repository that you have cloned
  previously by heading over to the top left hand corner, selecting
  `File > Open Folder...`, and entering the path to the repository.
  In this case, you should be navigating to the folder
  `/<NAME_OF_DATA_SOURCE>/workspaces/<YOUR_HYPHENATED_NAME>/toxic-comments-severity`.

- Now, let's initialise `conda` for the bash shell, and create
  the virtual environment specified in
  `toxic-comments-severity-conda-env.yaml`.

=== "VSCode Server Terminal"

    ```bash
    conda env create \
        -f toxic-comments-severity-conda-env.yaml \
        -p /<NAME_OF_DATA_SOURCE>/workspaces/<YOUR_HYPHENATED_NAME>/conda_envs/toxic-comments-severity
    ```

- After creating the `conda` environment, let's create a permanent
  alias for easy activation.

=== "VSCode Server Terminal"

    ```bash
    echo 'alias toxic-comments-severity-conda="conda activate /<NAME_OF_DATA_SOURCE>/workspaces/<YOUR_HYPHENATED_NAME>/conda_envs/toxic-comments-severity"' >> ~/.bashrc
    source ~/.bashrc
    toxic-comments-severity-conda
    # conda environment has been activated as (toxic-comments-severity)
    ```

!!! tip
    If you encounter issues in trying to install Python libraries,
    do ensure that the amount of resources allocated to the VSCode
    server is sufficient. Installation of libraries from PyPI tends
    to fail when there's insufficient memory. For starters, dedicate
    4GB of memory to the service:

    Another way is to add the flag `--no-cache-dir` for your
    `pip install` executions. However, there's no similar flag for
    `conda` at the moment so the above is a blanket solution.

??? info "Reference Link(s)"

    - [`conda` Docs - Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file)
    - [StackOverflow - "Pip install killed - out of memory - how to get around it?"](https://stackoverflow.com/questions/57058641/pip-install-killed-out-of-memory-how-to-get-around-it)
    - [phoenixNAP - Linux alias Command: How to Use It With Examples](https://phoenixnap.com/kb/linux-alias-command#:~:text=In%20Linux%2C%20an%20alias%20is,and%20avoiding%20potential%20spelling%20errors.)

## Jupyter Kernel for VSCode

While it is possible for VSCode to make use of different virtual Python
environments, some other additional steps are required for the VSCode
server to detect the `conda` environments that you would have created.

- Ensure that you are in a project folder which you intend to work
  on. You can open a folder through `File > Open Folder...`.
  In this case, you should be navigating to the folder
  `/<NAME_OF_DATA_SOURCE>/workspaces/<YOUR_HYPHENATED_NAME>/toxic-comments-severity`.

- Install the VSCode extensions [`ms-python.python`][py-ext] and
  [`ms-toolsai.jupyter`][jy-ext]. After installation of these 
  extensions, restart VSCode by using the shortcut `Ctrl + Shift + P`, 
  entering `Developer: Reload Window` in the prompt and pressing 
  `Enter` following that.

- Ensure that you have [`ipykernel`][ipyk] installed in the `conda` 
  environment that you intend to use. This template by default lists 
  the library as a dependency under 
  `toxic-comments-severity-conda-env.yaml`. You can check for the
  library like so:

=== "VSCode Server Terminal"

    ```bash
    conda activate /<NAME_OF_DATA_SOURCE>/workspaces/<YOUR_HYPHENATED_NAME>/conda_envs/toxic-comments-severity
    conda list | grep "ipykernel"
    ```
    
    Output should be:

    ```
    ipykernel  6.25.0  pypi_0  pypi
    ```

- Now enter `Ctrl + Shift + P` again and execute 
  `Python: Select Interpreter`. Provide the path to the Python 
  executable within the `conda` environment that you intend to use, 
  something like so: `path/to/conda_env/bin/python`.

- Open up any Jupyter notebook and click on the button that says
  `Select Kernel` on the top right hand corner. You will be presented
  with a selection of Python interpreters. Select the one that
  corresponds to the environment you intend to use.

- Test out the kernel by running the cells in the sample notebook
  provided under `notebooks/sample-pytorch-notebook.ipynb`.

[py-ext]: https://marketplace.visualstudio.com/items?itemName=ms-python.python
[jy-ext]: https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter
[ipyk]: https://ipython.readthedocs.io/en/stable/install/kernel_install.html

## Jupyter Kernel for JupyterLab

!!! attention
    If you're using VSCode and not JupyterLab, this section can be 
    skipped.

The same with the VSCode server, the JupyterLab server would not by 
default detect `conda` environments. You would have to specify to the 
JupyterLab installation the `ipython` kernel existing within your 
`conda` environment.

- Open up a [terminal within JupyterLab][jy-tty].

- Activate the `conda` environment in question and ensure that you have
  [`ipykernel`][ipyk] installed in the `conda` environment that you 
  intend to use. This template by default lists the library as a 
  dependency under `toxic-comments-severity-conda-env.yaml`. You can 
  check for the library like so:

=== "JupyterLab Terminal"

    ```bash
    conda activate /<NAME_OF_DATA_SOURCE>/workspaces/<YOUR_HYPHENATED_NAME>/conda_envs/toxic-comments-severity
    conda list | grep "ipykernel"
    ```
    
    Output should be:

    ```
    ipykernel  6.25.0  pypi_0  pypi
    ```

- Within the `conda` environment, execute the following:

=== "JupyterLab Terminal"

    ```bash
    ipython kernel install --name "toxic-comments-severity" --user
    ```

- Refresh the page.

- Open up the sample notebook provided under `notebooks/sample-pytorch-notebook.ipynb`.

- Within each Jupyter notebook, you can select the kernel of specific
  `conda` environments that you intend to use by heading to the toolbar 
  under `Kernel` -> `Change Kernel...`.

![Run:ai - JupyterLab Server Change Kernel](assets/screenshots/runai-jupyterlab-server-change-kernel.png)

- Test out the kernel by running the cells in the sample notebook.

??? info "Reference Link(s)"

    - [Jupyter Docs - Kernels (Programming Languages)](https://docs.jupyter.org/en/latest/projects/kernels.html)

[jy-tty]: https://jupyterlab.readthedocs.io/en/stable/user/terminal.html