# GitHub Action - Setup and Cache Python Poetry

This action simplifies the setup and caching of Poetry,
and provides the following functionality for GitHub Actions users:

* Python setup via [`actions/setup-python`](https://github.com/actions/setup-python).
* Poetry install via [`snok/install-poetry`](https://github.com/snok/install-poetry).
* Poetry binary caching via [`actions/cache`](https://github.com/actions/cache).
* Poetry dependency caching via [`actions/cache`](https://github.com/actions/cache).

## Basic Usage

* We assume you already have `pyproject.toml`, `poetry.lock`.
* For your first `push` to `main`, the workflow will download Poetry and the required project dependencies, then save it
  to the cache.
* For your second run (whether it's on a different job, re-run the job or a different
  workflow [based on your first cached commit](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#restrictions-for-accessing-a-cache))
  this Action will use the cache.
* You can see the list of cache entries by going to: `Your Repo` -> `Actions` tab -> `Caches` under `Managements` (left
  navbar, at the bottom).
* [Don't forget the limitation of cache](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy).

```yml
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
        uses: actions/checkout@v3
      - name: "Setup Python, Poetry and Dependencies"
        uses: dsoftwareinc/setup-python-poetry-action@v1
        with:
          python-version: 3.11
          poetry-version: 1.7.1

      # Run what you want in the poetry environment
      - name: Run tests
        run: |
          poetry run python manage.py test
```

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
