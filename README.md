# Step to deploy React vite project in github pages

## Steps

1. Setup all project files
2. Make repository
3. Push all the files to github
4. In github go to setting change the name of branch from master to **main**, if your branch is master.
5. After changing branch on website change as well on terminal **git branch/git checkout main/git pull origin main/git push**
6. Then go to action/general in workflow permission switch to **read and write permission**
7. Put base below plugins in vite.config.js file : **base: '/<repo_name>/'**
8. In main directory make file **.github/workflows/deploy.yml**
9. Put this below code in **deploy.yml file**

```
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v3
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v3
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist

```

9. In github go to actions and wait for sometime
10. Then change the branch from main ot gh_pages
11. Go to setting/pages select gh_pages and save and wait for some second there will a link on top
12. And done your vite react is deployed in github pages
