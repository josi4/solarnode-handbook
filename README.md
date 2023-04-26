# SolarNode Handbook Source

This project is the source for the [SolarNode Handbook](https://solarnetwork.github.io/solarnode-handbook/).

# Contributions welcome!

We welcome contributions to the SolarNode Handbook! Contributions can come in many forms and no
contribution is too small:

 * questions
 * spelling corrections
 * grammer suggestions
 * translations
 * clarifications
 * additional details
 * additional topics

Open an [issue](https://github.com/SolarNetwork/solarnode-handbook/issues) to start a discussion
on your contribution. You can fork this repository and open a pull request to submit your updates.

# Build requirements

 * [MkDocs](https://github.com/mkdocs/mkdocs/)
 * [mkdocs-awesome-pages-plugin](https://github.com/lukasgeiter/mkdocs-awesome-pages-plugin)
 * [mkdocs-enumerate-headings-plugin](https://github.com/timvink/mkdocs-enumerate-headings-plugin)
 * [mkdocs-material](https://github.com/squidfunk/mkdocs-material)
 * [mkdocs-open-in-new-tab](https://github.com/JakubAndrysek/mkdocs-open-in-new-tab)


## macOS

On macOS, you can install MkDocs with Homebrew, then use `pip3` to install the
Python packages:

```sh
brew install mkdocs
pip3 install mkdocs-awesome-pages-plugin mkdocs-enumerate-headings-plugin mkdocs-material mkdocs-open-in-new-tab
```

# Building

To build and view the handbook on your own machine, run `mkdocs serve`. You can then view
the handbook at http://localhost:8000/.
