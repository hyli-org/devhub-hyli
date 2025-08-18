# Hyli developer hub

This repository contains the source of the [Hyli Developer Hub](https://docs.hyli.org/).

You can participate by filing issues or submitting pull requests.

## Installation

To run, you need [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).  

Follow their installation guide, then install the [page](https://github.com/lukasgeiter/mkdocs-awesome-pages-plugin) plugin:

```sh
pip3 install mkdocs-awesome-pages-plugin
```

And if you run into any issue, install the Material theme for MkDocs:

```sh
pip install mkdocs-material
```

Also install:

```sh
pip install mkdocs-video
pip install "mkdocs-material[imaging]"
pip install neoteroi-mkdocs
pip install mkdocs-redirects
```

## Dev mode

This will watch and serve the docs at <http://localhost:8000>:

```sh
make serve
```

## Build & Deployment

```sh
make build
make deploy # You need github write access
```
