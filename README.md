# blog
Personal Blog

This repository holds my personal blog.  

It uses [mkdocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) to create the site from Markdown files in the docs directory.

There is a [devcontainer](https://containers.dev/) configuration to provide a local development and test environment and [github actions](https://docs.github.com/en/actions) to publish the site when changes are pushed to the repository.

**Note**
*For the github action to work you need to give the workflow permission to write to your repository.  This is done by alterning settings of your github repo : Settings -> Actions -> General -> Workflow Permissions - select the **Read and write** permission*

The site is a static site served by [github pages](https://docs.github.com/pages)
