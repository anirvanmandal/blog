---
layout: post
title: Deploying Swagger Docs using github actions
description: A blog post on how to deploy swagger docs to Github pages via Github actions
date: '2023-05-01T00:00:00.000Z'
author: Anirvan Mandal
categories: [Github Actions]
tags: ruby rails swagger rswag github

---

Swagger is an open-source tool that allows developers to document and design APIs.
Swagger makes it easy for developers to create and manage API documentation, making it an essential tool for any team working on an API project.
One way to share the API documentation created using Swagger is to deploy it to GitHub Pages, which allows users to access the documentation directly from a website hosted on GitHub.
In this blog post, we will explore how to deploy Swagger documentation to GitHub Pages using GitHub Actions.

Note: The swagger json files in here are generated using rswag. 

###### Create a GitHub Action

Create a GitHub Action that will deploy the Swagger documentation to GitHub Pages.
To do this, create a new file in the `.github/workflows` directory of your repository, and name it `swagger.yml`.
This file will contain the configuration for the GitHub Action.

###### Add the Github Action code

In the `swagger.yml` file, add the following code:

```yaml
name: Deploy Swagger docs to Github pages
on:
  push:
    branches:
      - 'main'
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: "pages"
  cancel-in-progress: false
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Build Swagger
        env:
          RELEASE_TAG: v4.18.2
        run: |
          curl -sL -o $RELEASE_TAG https://api.github.com/repos/swagger-api/swagger-ui/tarball/$RELEASE_TAG
          tar -xzf $RELEASE_TAG --transform 's/dist/NEW_PATH/' --strip-components=1 $(tar -tzf $RELEASE_TAG | head -1 | cut -f1 -d"/")/dist
          rm $RELEASE_TAG
          cp path/to/initial/swagger.json path/to/swagger/docs/api-swagger.json 
          SWAGGER_CONFIG="url: '.\/api-swagger.json'"
          sed -i "s/url:.*/$SWAGGER_CONFIG/g" swagger-docs/swagger-initializer.js
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: './path/to/swagger/docs'
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

This code will create a new GitHub Action that will be triggered whenever you push changes to the `main` branch of the repository.
The `uses: actions/checkout@v3` step will check out the repository code in the GitHub Action environment.
The `uses: actions/deploy-pages@v2` step will deploy the Swagger documentation to the `gh-pages` branch of the repository, which will be used to host the website.

There are two parts to this Github Action:

1. Build the swagger files with all the configuration
2. Push the configured directory to Github Pages


### Configuring the Github Action

This Github Action has 2 jobs, `build` & `deploy`

###### Building the deployment directory

In the `Build Swagger` action we download the latest Swagger UI version and modify the configuration to point to the correct swagger json files.
Right now we are just using v4.18.2 as the hard-coded version.

###### Upload the deployment artifacts

In the `swagger.yml` file, you need to configure the directory where the Swagger documentation is stored.
Replace `./path/to/swagger/docs` with the path to the directory containing the Swagger documentation in your repository.
For example, if your Swagger documentation is stored in a directory called `swagger-docs` at the root of the repository, you would use `path: ./swagger-docs`.

The built directory is then deployed to Github Pages using the `actions/deploy-pages@v2` action.

###### Commit and push the changes

Once you have added the deployment code and configured the deployment directory, commit the changes to your repository and push them to the `main` branch.
This will trigger the GitHub Action and deploy the Swagger documentation to GitHub Pages.

###### Access the Swagger documentation

Finally, you can access the Swagger documentation by visiting the URL `https://<username>.github.io/<repository>/`.
Replace `<username>` with your GitHub username and `<repository>` with the name of your repository.
This will take you to the GitHub Pages website, where you can view the Swagger documentation.

### Conclusion

Deploying Swagger documentation to GitHub Pages using GitHub Actions is a straightforward process that can make it easier for developers to share and access API documentation.
By following the steps outlined in this blog post, you can quickly set up a GitHub Action that will automatically deploy your Swagger documentation