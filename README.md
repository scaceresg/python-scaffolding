[![Python application test with GitHub Actions](https://github.com/scaceresg/scaffold-python/actions/workflows/main.yml/badge.svg)](https://github.com/scaceresg/scaffold-python/actions/workflows/main.yml)

# Python Scaffolding 

**Contents**:

- [Python Scaffolding](#python-scaffolding)
  - [Set up Key-pairs for SSH Login](#set-up-key-pairs-for-ssh-login)
  - [Create a GitHub Repo](#create-a-github-repo)
  - [Create Scaffold Files](#create-scaffold-files)
  - [Set up a Python Environment](#set-up-a-python-environment)
    - [For Linux](#for-linux)
    - [For Windows](#for-windows)
  - [Configure `Makefile`](#configure-makefile)
  - [Configure `requirements.txt`](#configure-requirementstxt)
  - [Add your Source Code into the Python scripts](#add-your-source-code-into-the-python-scripts)
    - [`hello.py`](#hellopy)
    - [`test_hello.py`](#test_hellopy)
  - [Push changes to GitHub](#push-changes-to-github)
  - [Set up GitHub Actions](#set-up-github-actions)


## Set up Key-pairs for SSH Login

Using the terminal, generate SSH login key-pairs:

```
$ ssh-keygen -t rsa
```

* Click Enter for directory, passphrase and repeat passphrase.

* The key-pair is located in `/home/[user_name]/.ssh/id_rsa` and 
`/home/[user_name]/.ssh/id_rsa.pub`

* Run `cat /home/[user_name]/.ssh/id_rsa.pub` to see the public key.
Copy this key and go to your GitHub account.

* Go to the account settings > SSH and GPG keys > New SSH Key.

* Add a title and paste the public key into the Key space. Click on 
Add SSH key.

Now you can SSH login from your Linux terminal.

## Create a GitHub Repo

* Create a GitHub repository: `scaffold-python` 

  - Add a README.md and a `.gitignore` file using the Python 
  template

* Clone the GitHub repository:

  ```
  git clone git@github.com:scaceresg/scaffold-python.git
  ```

## Create Scaffold Files

Inside the repository directory, create the following files:

* **Makefile**: 
  
  ```
  touch Makefile
  ```

* **[script_name].py**: Python files with your source code. 
  
  For example:
    
  - `hello.py`: Python file that sums two numbers and returns the 
  result
 
  - `test_hello.py`: Python file that tests `hello.py`

* **requirements.txt**: 
  
  ```
  touch requirements.txt
  ``` 

## Set up a Python Environment

### For Linux

* Set up a Python environment for the current project using:

  ```
  python3 -m venv ./.[project_name]
  ```

  For example:

  ```
  $ python3 -m venv ./.scaffold-python
  ```

* Activate the Python environment:

  ```
  $ source ./.scaffold-python/bin/activate
  ```

* Check the location of the environment:

  ```
  $ which python
  ```
  ```
  ./.scaffold-python/bin/python
  ```

### For Windows

* Set up a Python environment for the current project using:

  ```
  > python -m venv .\.[project_name]
  ```

* Activate the Python environment:

  - Using Powershell:

    ```
    > .\.[project_name]\Scripts\Activate.ps1
    ```
    
  - Using CMD:

    ```
    > .\.[project_name]\Scripts\activate.bat
    ```

## Configure `Makefile`

Open the `Makefile` and add the following instructions:

**Recommended structure:**

```
install:
	pip install --upgrade pip &&\
		pip install -r requirements.txt

format:
	black *.py

lint:
	pylint --disable=R,C hello.py

test:
	python -m pytest -vv --cov=hello test_hello.py

all:    install lint test
```

## Configure `requirements.txt`

Open the `requirements.txt` file and add the required packages:

```
pylint
pytest
click
black
pytest-cov
```

* Once you have added the instructions to the `Makefile` and the 
packages to the `requirements.txt`, you can run `make install`
to install the packages into the Python environment.

## Add your Source Code into the Python scripts

### `hello.py`

Add the following code:

```
def add(x, y):
    return x + y

result = add(1, 2)

print(f"This is the sum: 1 + 2 = {result}")                                                       
```

* You can run `make lint` so Pylint analyses the code:

  - Pylint checks the syntax and debugs your code

* You can also run `make format` so `black` formats the 
code.

### `test_hello.py`

Add the following test code:

```
from hello import add

def test_add():
    assert add(1, 2) == 3
```

* Run `make test` to run the `hello_test.py` script on
`hello.py`

* You can now run `make all` to run the `install lint test`
steps in the `Makefile`.

## Push changes to GitHub

* Add the new files using `git add .`

* Commit changes: 
  
  ```
  $ git commit -m "[message]"
  ```

* Push changes: 
  
  ```
  $ git push -u origin main
  ```

## Set up GitHub Actions

In your GitHub repository page, go to Actions:

* Click on Set up a workflow yourself

* It creates a `main.yml` file in the `/.github/workflows/` 
directory of your repository

* Add the following instructions:

  ```
  name: Python application test with GitHub Actions
  on: [push]
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
      - uses: actions/checkout@v2
      - name: Set up Python 3.8
        uses: actions/setup-python@v1
        with:
          python-version: 3.8
      - name: Install dependencies
        run: |
          make install
      - name: Lint with Pylint
        run: |
          make lint
      - name: Test with Pytest
        run: |
          make test
      - name: Format code with Python black
        run: |
          make format
  ```

* Commit changes

* Go to Actions, and click on All workflows on the left side 
bar:

  - You can select the last commits and check the jobs that 
  were performed.

  - When you click on one of the commits/updates/jobs, you can 
  see the jobs on the left sidebar and also **create status badge** 
  to show on your README.md file.

  - You can copy and paste this badge to show the current 
  status of your project.
