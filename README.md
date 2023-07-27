# **<p align="center">HOW TO DEPLOY VITE REACT APP📌</p>**
🌻 [Live Website](https://mohinimahato.github.io/Vite-Deploy/) 
### Follow the steps mentioned below:

0️⃣ Create a new vite app using the given command 
```Javascript
npm create vite@latest [Repo-name] -- --template react
```
1️⃣ Create a new repository on GitHub and initialize GIT
```Javascript
git init 
git add . 
git commit -m "Add: initial files" 
git branch -M main 
git remote add origin https://github.com/[USER]/[REPO_NAME] 
git push -u origin main
```
2️⃣ Setup base on vite.config.js
```Javascript
base: "/[REPO_NAME]/"
```
3️⃣ Create ./github/workflows/deploy.yml and add the code below
```Javascript
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
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
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
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```
4️⃣ Push your code in the github repository created
```Javascript
git add . 
git commit -m "Add: deploy workflow" 
git push
```
5️⃣ Setting up the github repository workflow
```Javascript
1) Settings -> Config -> Actions -> General -> Workflow permissions -> Read and Write permissions 
Actions -> failed deploy -> re-run-job failed jobs 
2)Settings -> Pages -> gh-pages -> save
```
6️⃣ For future code updates
```Javascript
git add . 
git commit -m "fix: some bug" 
git push
```
- The deployment for the further changes will take place automatically whenever we push on github🌻

