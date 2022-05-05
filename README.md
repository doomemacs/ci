# doomemacs/ci

> :warning: This action is in alpha, and will undergo drastic and sudden changes
> without warning. What's more, it uses a version of Doom Emacs' CLI that isn't
> publicly available yet (use the `legacy` branch instead). Use at your own
> risk.

A Github action that sets up a Doom Emacs environment so it can be used for
CI/CD.

## Usage
```yml
name: Run tests
on:
  pull_request:
  push:
    paths-ignore:
      - '**.md'
      - '**.org'
      - 'docs/**'
      - '.dir-locals.el'
      - 'LICENSE'
    branches:
      - master
jobs:
  run-tests:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        emacs-version: [27.1, 27.2, 28.1, snapshot]
    steps:
      - uses: actions/checkout@v3.0.2

      - uses: doomemacs/ci@main
        with:
          emacs-version: ${{ matrix.emacs-version }}

      - run: doom ci run-tests tests/
```

## Configuration
By default, this action sets `$DOOMDIR` to `.github/`, so more in-depth CI/CD
configuration can be placed in:

- `.github/ci.el`: reconfigure `doom ci ...` commands here.
- `.github/cli.el`: define your own cli commands here.
- `.github/packages.el`: for specifying external dependencies

Change `inputs.doomdir` to change the location.

## Options
Every argument is optional.

| Input         | Description                                               | Default      |
|---------------|-----------------------------------------------------------|--------------|
| version       | The version, branch, tag, or SHA of Doom core to install  | `"master"`   |
| emacs-version | The version of Emacs to install (use snapshot for `HEAD`) | `"28.1"`     |
| emacsdir      | Where to install Doom Emacs                               | `".emacs.d"` |
| doomdir       | Where to look for any private configuration               | `".github"`  |
| sync          | Whether to automatically 'doom sync' after install        | `true`       |

## Contributing
`TODO`
