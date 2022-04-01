# GitHub Pages Action

GitHub Action to deploy a static site on GitHub Pages

## Usage

Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below (under the "Example workflow" section). For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

### Inputs

| Input | Description | Required | Example values |
| :---: | :---: | :---: | :---: |
| `email:` | Your verified email address | YES | name.surname@example.com |
| `build_dir:` | Static files location | NO | ./build |
| `branch:` | Which branch to push files | NO | your_branch |
| `cname:` | The custom domain name | NO | example.com |
| `commit_message:` | Custom commit message | No | Your commit message |
| `jekyll:` | In case if you use Jekyll | NO | no |


### Simple usage

```yaml
        uses: telia-actions/github-pages-action@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          email: name.surname@example.com
```

### Example workflow
Below you will find an example workflow using all available inputs.

```yaml
name: GitHub Pages Action
on:
  workflow_dispatch:

jobs:
  build:
    runs-on: 'ubuntu-latest'

    strategy:
      matrix:
        node-version: [14.x]

    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}

    - name: Verify cache, install and build dependencies
      run: |
        npm cache verify
        npm install
        npm run build 
    - name: Deploy to GitHub Pages
      uses: telia-actions/github-pages-action@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        email: name.surname@example.com
        build_dir: ./build
        branch: your_branch                
        cname: example.com              
        jekyll: no                     
        commit_message: Commit message
```