# Introduction 
Splitting up large codebases into separate independently versioned packages is extremely useful for code sharing. However, making changes across many repositories is messy and difficult to track, and testing across repositories gets complicated really fast.

To solve these (and many other) problems, some projects will organize their codebases into multi-package repositories (sometimes called monorepos). Lerna is a tool that optimizes the workflow around managing multi-package repositories with git and npm.

# What does it do?
The two primary commands in Lerna are lerna bootstrap and lerna publish.

bootstrap will link dependencies in the repo together. publish will help publish any updated packages.

# Folder structure
Workspaces
Usually when working with a monorepo you’ll have a project structure similar to:

| packages/
| -- project-1
| ---- index.js
| ---- package.json
| -- project-2
| ---- index.js
| ---- package.json
| package.json