# GitHub Action - Setup and Cache Python Poetry

This GitHub action simplifies the setup and caching of Poetry dependencies for Python projects.

When a job using this action runs for the first time, this action will download Poetry and the required project
dependencies, then save it to the cache.

For the following runs (whether it's on a different workflow/job [with the same cached commit][1])
this action will restore the cache, which is much faster than downloading everything again.

## Basic Usage

* Make sure you have `pyproject.toml`, `poetry.lock`.

```yaml
name: ci

on:
  push:
    branches: [ master ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      # Deal with environment setup and caching      
      - name: Check out the repository
        uses: actions/checkout@v4
      - name: "Setup Python, Poetry and Dependencies"
        uses: dsoftwareinc/setup-python-poetry-action@v1
        with:
          python-version: 3.11
          poetry-version: 1.7.1
          poetry-install-additional-args: '-E flag' # Optional

      # Run what you want in the poetry environment
      - name: Run tests
        run: |
          poetry run python manage.py test
```

## Input variables:

| Name                             | Description                                       | Required | Default value |
|----------------------------------|---------------------------------------------------|----------|---------------|
| `python-version`                 | Python version to use.                            | Yes      | n/a           |
| `poetry-version`                 | Poetry version to use.                            | Yes      | n/a           |
| `poetry-install-additional-args` | Additional arguments to pass to `poetry install`. | No       | ""            |

## Notes

* You can see the list of cache entries by going to:
  `Repo` -> `Actions` tab -> `Caches` under `Managements` (left navbar, at the bottom).
* [Don't forget the limitation of cache][2].

## Dependencies

* Python setup using [`actions/setup-python`][3].
* Poetry install using [`snok/install-poetry`][4].
* Poetry binary and dependency caching using [`actions/cache`][5].

## License

The scripts and documentation in this project are released under the [MIT License][6].


[1]:https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#restrictions-for-accessing-a-cache

[2]:https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy

[3]:https://github.com/actions/setup-python

[4]:https://github.com/snok/install-poetry

[5]:https://github.com/actions/cache

[6]:LICENSE