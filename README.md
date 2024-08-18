# Rimworld Together Guide

This repo is an [mdBook](https://github.com/rust-lang/mdBook) on how to manage, use, and play RimWorld-Together.

## Contribution

To contribute to this repo, please install `mdbook`, test locally, and submit all PRs to the `main` branch.

### Building and Testing

Run `make build` to build the book.

Run `make serve` to build and serve the book at <http://localhost:3000>

### Developer Tools

Run `make install-hook` to install our [pre-commit hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks). This will guarantee that any change you're making will pass PR checks.

This project also has configurations to use the tool [`typos`](https://github.com/crate-ci/typos). Use at your own preference.
