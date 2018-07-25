# AFS-docs

[![Documentation Status](https://readthedocs.org/projects/afs-docs/badge/?version=latest)](https://afs-docs.readthedocs.io/en/latest/?badge=latest)

Documentation for EnSaaS Analytic Framework Service and related projects

This documentation will hosted on [Readthedocs](https://afs-docs.readthedocs.io/en/latest/).

## How to build this documentation locally

1. Install requirements
    ```
    pip install -r requirements.txt
    ```
2. Build the documentation
    ```
    cd docs/
    make html
    ```

The output documentation can be found at `docs/_build/html/index.html`