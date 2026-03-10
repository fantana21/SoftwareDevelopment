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
- If you develop a library or application that is intended to be used in other projects
  and/or by other users (that includes you on another machine) turn your project into a
  proper Python package. Take a look at the [Python Packaging User
  Guide](https://packaging.python.org/), specifically the tutorial on [packaging Python
  projects](https://packaging.python.org/tutorials/packaging-projects/) for how to do
  that. We use the src layout.
- Use [ruff](https://docs.astral.sh/ruff/) to lint your code. It is fast and combines code
  formatting, import sorting, and linting in one tool. A typical configuration for ruff in
  `pyproject.toml` looks like this:
  ~~~toml
  [tool.ruff]
  include = ["src/**/*.py", "tests/**/*.py"]
  line-length = 79

  [tool.ruff.lint]
  select = [
      "E",  # pycodestyle errors
      "W",  # pycodestyle warnings
      "F",  # Pyflakes
      "UP", # pyupgrade
      "B",  # flake8-bugbear
      "I",  # isort
  ]
  ~~~
- Use [mypy](https://mypy.readthedocs.io/en/stable/) to check your type hints. They are optional in
  Python, but help you to write correct code and catch bugs early. See
  [PEP484](https://peps.python.org/pep-0484/) and [PEP526](https://peps.python.org/pep-0526/) for
  more information on type hints. A configuration for mypy in `pyproject.toml` might look something
  like this:
  ~~~toml
  [tool.mypy]
  # Min. required Python version
  python_version = "3.10"
  # We use the src layout, so we need to tell mypy where to find our code
  mypy_path = ["src"]
  # Check our package and tests
  packages = ["my_package", "tests"]
  warn_unreachable = true

  [[tool.mypy.overrides]]
  # We are strict in our package
  module = "my_package.*"
  disallow_untyped_calls = true
  disallow_untyped_defs = true

  [[tool.mypy.overrides]]
  # We are more lenient in our tests
  module = "tests.*"
  check_untyped_defs = true
  ~~~
