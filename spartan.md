---
layout: default
---

## Introduction:

Given that the majority of servers run on Linux, we'll be programming using Linux and utilizing an environment compatible with Linux.
The first step is to verify that Python is installed.

```bash
$ python --version
Python 3.11.8
```

Next, navigate to the project folder, Let's create the virtual environment.

```bash
$ python -m venv venv
```

Now we need to install the dependencies listed in the requirements.txt file.

```bash
$ source venv/bin/activate
$ pip install -r requirements.txt
```

## Usage:

Let's start up the project, which runs on Uvicorn and the FastAPI framework.

```bash
$ uvicorn main:app
```
