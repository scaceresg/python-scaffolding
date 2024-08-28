[![Python application test with GitHub Actions](https://github.com/scaceresg/python-scaffolding/actions/workflows/main.yml/badge.svg)](https://github.com/scaceresg/python-scaffolding/actions/workflows/main.yml)

# Python Scaffolding 

This is a project scaffold for Python.

## Setting up key-pairs for SSH login

In the Linux terminal, generate SSH login key-pairs:

`$ ssh-keygen -t rsa`

* Click Enter for directory, passphrase and repeat passphrase.

* The key-pair is located in `/home/[user_name]/.ssh/id_rsa` and 
`/home/[user_name]/.ssh/id_rsa.pub`

* Run `cat /home/[user_name]/.ssh/id_rsa.pub` to see the public key.
Copy this key and go to your GitHub account.

* Go to the account settings > SSH and GPG keys > New SSH Key.

* Add a Title and Paste the public key in Key. Click on Add SSH key.

Now you can SSH login from your Linux terminal.

## Creating the scaffold files

* Create a GitHub repository: `python-scaffolding` 

    - Add a README.md and a `.gitignore` file using the Python template

* Clone the GitHub repository:

    - Run `git clone git@github.com:scaceresg/python-scaffolding.git`

* Create a `Makefile`: `touch Makefile`

* Create Python files and the `requirements.txt` file:

    - Run `touch hello.py`, `touch test_hello.py` and 
    `touch requirements.txt`

## Defining a Python environment

* Set up a Python environment for the current project using:

    `python3 -m venv ~/.[project_name]`

    For example:

    - `$ python3 -m venv ~/.python-scaffolding`

* Activate the Python environment:

    `$ source ~/.python-scaffolding/bin/activate`

* Check the location of the environment:

    `$ which python`

    `> ~/.python-scaffold/bin/python`

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

## Modify the Python scripts

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

* Commit changes: `$ git commit -m "[message]"`

* Push changes: `$ git push -u origin main`
