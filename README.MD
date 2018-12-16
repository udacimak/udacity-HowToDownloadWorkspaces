# How to Download Udacity Workspace

## Download Jupyter workspace

To download all files in a Jupyter workspace:
- Go to _File > New Notebook > Python 3_ - this will create a new Jupyter
Notebook.
- Paste the [code snippet
below](#download-snippet) into the code cell in the new Jupyter Notebook
- Click on Jupyter logo (top left of the screen) to open the directory browser.
This will show all files and directories in the workspace.
- Find `workspace_archive.tar`. download it.

### Download extra data located outside of current workspace

Occassionally, you'll see in the code of a Notebook where it gets data from
a different directory than the current workspace. In other words, the
data is located outside of current `./` working directory.

For example, the code below is trying to get data from
`../../data`:

```python
def example():
    data = os.path.join(os.getcwd(), '..', '..','data')
    return np.load(data)
```

To download the data, simply run `make_tar_file` function from the [code snippet
below](#download-snippet) with `dir_name` being the path to the data:

```python
    dir_name = os.path.join(os.getcwd(), '..', '..','data')
    make_tar_file(dir_name)
```

## Download Python workspace

For Python workspaces, use the [code snippet
below](#download-snippet) but put it in a Python file instead.

## Download Snippet

The snippet below compresses all files in a workspace and put them in
`workspace_archive.tar` file.

```python
""" Source:
      - https://stackoverflow.com/questions/48122744/how-to-download-all-files-and-folder-hierarchy-from-jupyter-notebook
"""
import os
import tarfile

def recursive_files(dir_name='.', ignore=None):
    for dir_name,subdirs,files in os.walk(dir_name):
        if ignore and os.path.basename(dir_name) in ignore:
            continue

        for file_name in files:
            if ignore and file_name in ignore:
                continue

            yield os.path.join(dir_name, file_name)

def make_tar_file(dir_name='.', target_file_name='workspace_archive.tar', ignore=None):
    tar = tarfile.open(target_file_name, 'w')

    for file_name in recursive_files(dir_name, ignore):
        tar.add(file_name)

    tar.close()


dir_name = '.'
target_file_name = 'workspace_archive.tar'
# List of files/directories to ignore
ignore = {'.ipynb_checkpoints', '__pycache__', target_file_name}

make_tar_file(dir_name, target_file_name, ignore)
```