# Contributing

## Developing `cnlp-transformers`

The first things to do when contributing to the code base here are to
clone this repository and set up your Python environment.

1. Clone this repository:
   ```sh
   # Either the HTTPS method...
   $ git clone https://github.com/Machine-Learning-for-Medical-Language/cnlp_transformers.git
   # ...or the SSH method
   $ git clone git@github.com:Machine-Learning-for-Medical-Language/cnlp_transformers.git
   ```

2. Enter the repo: `cd cnlp_transformers`

3. You will need Python 3.8. Either a `venv` virtual environment or a
   Conda environment should work. Create your environment and activate 
   it.

4. Install the development dependencies: 
   ```sh
   $ pip install -r dev-requirements.txt
   ```
   
5. See [README.md](README.md) for the note about PyTorch; 
   if needed, manually install it now.

6. Install `cnlp-transformers` in editable mode: 
   ```sh
   $ pip install -e .
   ```

**The remainder of the instructions on this document will assume that
you have installed the development dependencies.**

### Proposing changes

If you have changes to the code that you wish to contribute to the
repository, please follow these steps.

1. Fork the project on GitHub.

2. Add your fork as a remote to push your work to. Replace
   `{username}` with your username. This names the remote "fork", the
   default Machine-Learning-for-Medical-Language remote is "origin".
   ```sh
   # Either the HTTPS method...
   $ git remote add fork https://github.com/{username}/cnlp_transformers.git
   # ...or the SSH method
   $ git remote add fork git@github.com:{username}/cnlp_transformers.git
   ```

3. Make a new branch and set your fork as the upstream remote:
   > **Note:** see the section on testing below for information 
   > on how you may want to name your branch.
   ```sh
   $ git switch -c your-branch-name  # or git checkout -b
   $ git push --set-upstream fork your-branch-name
   ```

4. Open an issue that motivates the change you are making if there is
   not one already.

5. Make your changes in `your-branch-name` on your fork.

6. Open a PR to close the issue.
   * If you are not making changes to source files or project configuration
     files (`setup.cfg`, `pyproject.toml`, `MANIFEST.in`), you can target `main`
   * Otherwise, have your PR target the branch for the next release. 
     * If there is no such branch, create it by branching off of `main`, then
       target your new branch.


### Testing your code

This repository has GitHub Actions set up to automatically run the test 
suite in `test` whenever a commit is pushed or a pull request is opened 
in certain circumstances.

Tests will run if changes are made to any of the following files:
* `src/**`
* `test/**`
* `setup.cfg`
* `pyproject.toml`

AND the changes are in any of the following:

* Pull requests targeting either of the following:
  * the `main` branch
  * a branch name starting with `vX`, where `X` is a digit (e.g. `v0.3.0`)
* Further commits pushed to the source branch of such a pull request
* Commits pushed to a branch name starting with `testable/`, e.g. `testable/my-special-feature`

You can see the structure of these test runs in the 
[**Actions**](https://github.com/Machine-Learning-for-Medical-Language/cnlp_transformers/actions) 
tab of this repository. In short, they will build and test the project
on Python 3.7, 3.8, 3.9, and 3.10; these will always run at least on 
Linux, and in the case of commits or pull requests targeting `main`,
they will run on Linux, macOS, and Windows.

If you are developing in a public fork of the repository, you can use 
the `testable/` naming convention for your branch to have the forked 
actions run as you push to your fork. We recommend not tweaking the 
actions in your fork as this may cause unexpected behavior when opening
a pull request.

> **Note:** for collaborators, the same applies for work done directly 
> in branches in this repository that follow this naming convention.

Once you open a pull request targeting `main` or a version branch in
this repository, the test runs will be triggered on creation and any 
time you add new commits to the base branch in your fork. You do not
need to name your branch anything special in this case.

## For Maintainers: Building and uploading new package version

### Setting up your PyPI API key

1. Log into your PyPI account

2. Generate an API key for the `cnlp-transformers` project
   [here](https://pypi.org/manage/account/#api-tokens)

3. Create a file `~/.pypirc`:
   ```cfg
   [pypi]
   username = __token__
   password = <the token value, including the `pypi-` prefix>
   ```

### Building and uploading a new version

**Only follow these steps after merging the new version branch into 
`main`.**

1. Checkout the merge commit for the new version; this will usually
   be the latest commit in `main`.

2. Increment the version number in `src/cnlpt/VERSION` from the 
   previous version on PyPI.

3. Delete the contents of the `./dist/` directory if it exists.

4. Build the package using `build`:
   ```sh
   $ python -m build
   ```
   
   This will build the package in the `./dist/` directory, creating it if
   it does not exist.

5. Upload to PyPI with `twine`:
   ```sh
   $ python -m twine upload dist/*
   ```

6. On GitHub, make sure the branch for the version has been deleted after 
   the PR was merged into `main`. This is to ensure that readthedocs does
   not have a name conflict.

7. On GitHub, make a new release:
   1. Navigate to the [Releases](https://github.com/Machine-Learning-for-Medical-Language/cnlp_transformers/releases) page
   2. Click "Draft a new release"
   3. Click "Choose a tag"
   4. Type the new version number in the format “vX.Y.Z” in the “Find or
      create a new tag” field, and click “+ Create new tag: vX.Y.Z on publish”
   5. Click “Generate release notes” and edit as necessary
   6. Make sure “Set as latest release is checked”
   7. Click "Publish"

### Building the documentation

To rebuild the autodoc toctrees and the `transformers` Intersphinx 
mappings, run `build_doc_source.sh`.

To build the docs locally for testing documentation changes before 
uploading to readthedocs, first **uncomment lines 36 and 65 on 
`docs/conf.py`,** then execute the following:

```sh
$ cd docs
$ make html
```

This will write the docs to `docs/build/html`; simply open 
`docs/build/html/index.html` in your browser of choice to view the 
built documentation.
