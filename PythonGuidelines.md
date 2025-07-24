# Python guidelines


> [!IMPORTANT]
>
> If you only want to use a Python tool or application, install it with
> [pipx](https://pypi.org/project/pipx/). It is literally made for just that and will keep
> your environment clean. You can skip the rest of the guidelines below. They are intended
> for developers who want to write and maintain Python code.

- Read [PEP 20](https://www.python.org/dev/peps/pep-0020/) the Zen of Python
- Use virtual or conda environments. They allow you to install packages without polluting
  the global Python environment. This is especially important if you are working on
  multiple projects that require different versions of the same package.
- Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) the official Python style
  guide
- Follow [PEP 257](https://www.python.org/dev/peps/pep-0257/) and [PEP
  287](https://www.python.org/dev/peps/pep-0287/) for docstrings
- Use [black](https://pypi.org/project/black/) to format your code. It is an opinionated
  formatter with little configuration, but that's a good thing. The only IMO weird choice
  is to use a default line length of 88 characters. We instead use the 79 characters
  recommended by PEP 8. To do that, add the following lines to `pyproject.toml` in the
  root of your project:
  ~~~toml
  [tool.black]
  line-length = 79
  ~~~
- Use [isort](https://pypi.org/project/isort/) to sort your imports. To configure isort to
  work with black, add the following lines to `pyproject.toml`:
  ~~~toml
  [tool.isort]
  profile = "black"
  ~~~
- If you develop a library or application that is intended to be used in other projects
  and/or by other users (that includes you on another machine) turn your project into a
  proper Python package. Take a look at the [Python Packaging User
  Guide](https://packaging.python.org/), specifically the tutorial on [packaging Python
  projects](https://packaging.python.org/tutorials/packaging-projects/) for how to do
  that. We use the src layout and [setuptools](https://pypi.org/project/setuptools/) as
  our build backend.
