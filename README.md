# Game

- [Game](#game)
  - [Demo](#demo)
  - [Getting Started](#getting-started)
    - [Project Setup](#project-setup)
      - [Prerequisites](#prerequisites)
      - [Project Creation](#project-creation)
      - [Update NPM](#update-npm)
      - [Running the project](#running-the-project)
      - [Iterative Development](#iterative-development)
      - [Remove boilerplate code](#remove-boilerplate-code)
      - [Install BabylonJS](#install-babylonjs)
      - [Edit the `index.html` file](#edit-the-indexhtml-file)
      - [Edit the `tsconfig.json` file](#edit-the-tsconfigjson-file)
      - [Create a new `main.ts` file](#create-a-new-maints-file)
    - [Fullscreen Mode](#fullscreen-mode)
    - [Showing the Inspector](#showing-the-inspector)
    - [Deploying to GitHub Pages](#deploying-to-github-pages)
      - [Create a GitHub repository](#create-a-github-repository)
        - [Add your project to the repository](#add-your-project-to-the-repository)
        - [Configure GitHub Pages](#configure-github-pages)
      - [Create a `vite.config.js` file](#create-a-viteconfigjs-file)
      - [Create a `deploy.yml` file](#create-a-deployyml-file)
      - [Push your changes](#push-your-changes)

## Demo

See [here](https://corysia.github.io/vite-project/) for a live demo of the project.

## Getting Started

### Project Setup

#### Prerequisites

You must have `npm` and `node` installed.

- Windows: [Node.js](https://nodejs.org/en/)  You can also use the Windows package manager called `winget`:
  - Open a terminal and run `winget install nodejs`
- macOS: [Node.js](https://nodejs.org/en/)
- Linux: [Node.js](https://nodejs.org/en/)

#### Project Creation

Start by creating a new `vite` project.

```bash
npm create vite
```

You might need to install the `create-vite` package.  You'll see a prompt something like this if you do:

```bash
Need to install the following packages:
create-vite@5.2.3
Ok to proceed? (y)
```

Just press return and the package will install.

You'll be prompted to name your project and select a project type.  Name your project and then select `Vanilla` as the project type.  Choose `TypeScript` as your project language.

#### Update NPM

You might need to update your NPM to the latest version.  You might see a message similar to the following:

```bash
npm notice
npm notice New minor version of npm available! 10.1.0 -> 10.5.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v10.5.0
npm notice Run npm install -g npm@10.5.0 to update!
npm notice
```

You can either ignore it for now or update it to the latest version.

#### Running the project

Starting the project by following the instructions you're presented with:

```bash
  cd vite-project
  npm install
  npm run dev
```

where `vite-project` is whatever you named your project.

The `npm run dev` command starts the project.  If you press `o` and then `enter`, your browser will open to your project, hosted locally on your machine.  If you press `q` and then `enter`, the project will close it.

#### Iterative Development

You can edit your source while the project is running.  Occasionally, you will need to either refresh your browser or restart the server.  But for the most part, you can start the server and see your changes update instantly in the browser each time you save the file.

#### Remove boilerplate code

Now that you have your project up and running, you can remove the boilerplate code from the project.

First, be sure your server is stopped by using either `Ctrl + C` or `q` to quit.

Remove all the files in the `public` and `src` folders.

```bash
  rm -rf public/*
  rm -rf src/*
```

#### Install BabylonJS

Next, you need to install the [BabylonJS](https://www.babylonjs.com/) library.  We'll also want the inspector for debugging the project.

```bash
npm install --save-dev @babylonjs/core
npm install --save-dev @babylonjs/inspector
```

*NOTE*: You can shorten the above commands with:

```bash
npm i -D @babylonjs/core @babylonjs/inspector
```

#### Edit the `index.html` file

We need to add a `<style>` tag to the `index.html` file.  Change your html file to look like the following:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Game</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.ts"></script>
  </body>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      width: 100%;
      height: 100%;
    }
  </style>
</html>
```

Here we've added a `<style>` tag to the `index.html` file and also removed the `<link>` tag and changed the `<title>` tag.

#### Edit the `tsconfig.json` file

Change your `tsconfig.json` file to look like the following:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES6", "DOM"],
    "skipLibCheck": true,
    "removeComments": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"]
}
```

Here we've changed the `target` to `ES6`, the `lib` to `ES6` and `DOM`, and added the `removeComments` option.

#### Create a new `main.ts` file

Now that we have a basic project set up, we can start adding code to it.  Create a new file called `main.ts` and add the following code to it:

```typescript
import { Scene, Engine, FreeCamera, HemisphericLight, MeshBuilder, Vector3 } from "@babylonjs/core";

class Main {
    
    constructor() {
        let canvas = document.createElement("canvas");
        document.body.appendChild(canvas);
        let engine = new Engine(canvas, true);
        let scene = this.createScene(engine, canvas);
        
        engine.runRenderLoop(function () {
            scene.render();
        })
    }

    public createScene(engine: Engine, canvas: HTMLCanvasElement): Scene {
        // This creates a basic Babylon Scene object (non-mesh)
        let scene = new Scene(engine);

        // This creates and positions a free camera (non-mesh)
        let camera = new FreeCamera("camera1", new Vector3(0, 5, -10), scene);

        // This targets the camera to scene origin
        camera.setTarget(Vector3.Zero());

        // This attaches the camera to the canvas
        camera.attachControl(canvas, true);

        // This creates a light, aiming 0,1,0 - to the sky (non-mesh)
        let light = new HemisphericLight("light1", new Vector3(0, 1, 0), scene);

        // Default intensity is 1. Let's dim the light a small amount
        light.intensity = 0.7;

        // Our built-in 'sphere' shape. Params: name, options, scene
        let sphere = MeshBuilder.CreateSphere("sphere", {diameter: 2, segments: 32}, scene);

        // Move the sphere upward 1/2 its height
        sphere.position.y = 1;

        // Our built-in 'ground' shape. Params: name, options, scene
        let ground = MeshBuilder.CreateGround("ground", {width: 6, height: 6}, scene);

        return scene;
    }
}

new Main();
```

### Fullscreen Mode

Edit the `main.ts` file to add the following code:

```typescript
    constructor() {
        let canvas = document.createElement("canvas");
>       canvas.style.width = '100%';
>       canvas.style.height = '100%';
        document.body.appendChild(canvas);
        let engine = new Engine(canvas, true);
        let scene = this.createScene(engine, canvas);
```

This will allow the canvas to fill the entire browser window.

To handle the browser changing sizes, add the following code:

```typescript
    constructor() {
        let canvas = document.createElement("canvas");
        canvas.style.width = '100%';
        canvas.style.height = '100%';
        document.body.appendChild(canvas);
        let engine = new Engine(canvas, true);
        let scene = this.createScene(engine, canvas);
        
>        window.addEventListener("resize", function () {
>            engine.resize();
>        });
```

Add the following code to toggle fullscreen mode with Shift-Ctrl-Alt-F.

```typescript
        let engine = new Engine(canvas, true);
        let scene = this.createScene(engine, canvas);
        
        window.addEventListener("resize", function () {
            engine.resize();
        });

>       window.addEventListener("keydown", (ev) => {
>           // Shift+Ctrl+Alt+F
>           if (ev.shiftKey && ev.ctrlKey && ev.altKey && ev.code === "KeyF") {
>               engine.switchFullscreen(false);
>           }
>       });          
```

### Showing the Inspector

Edit the `main.ts` file to add another import to the top of the file:

```typescript
import "@babylonjs/inspector";
```

Update the `main.ts` file to add the following code to toggle the inspector with Shift-Ctrl-I.

```typescript
        window.addEventListener("keydown", (ev) => {
            // Shift+Ctrl+Alt+F
            if (ev.shiftKey && ev.ctrlKey && ev.altKey && ev.code === "KeyF") {
                engine.switchFullscreen(false);
            }
>            // Shift+Ctrl+Alt+I
>            if (ev.shiftKey && ev.ctrlKey && ev.altKey && ev.code === "KeyI") {
>                if (scene.debugLayer.isVisible()) {
>                    scene.debugLayer.hide();
>                } else {
>                    scene.debugLayer.show();
>                }
>            }
        });
```

### Deploying to GitHub Pages

Deploy your project to GitHub Pages by following the instructions in the following sections.

#### Create a GitHub repository

##### Add your project to the repository

##### Configure GitHub Pages

Under settings, select `Pages`.  Set it to use GitHub Actions.

#### Create a `vite.config.js` file

Edit the `vite.config.js` file to add the following code.  Your base should be your repository name.

```javascript
import { defineConfig } from 'vite'

// https://vitejs.dev/config/
export default defineConfig({
	base: '/vite-project/'
})
```

#### Create a `deploy.yml` file

Create the subdirectory `.github/workflows` and add the following code to a file named `deploy.yml`:

```shell
# Simple workflow for deploying static content to GitHub Pages
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets the GITHUB_TOKEN permissions to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload dist folder
          path: "./dist"
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### Push your changes

Push your changes up to github.  You should see the deployment of the project in the GitHub Actions tab.  If it is successful, you should be able to browse to `https://<your-github-username>.github.io/<your-repository-name>/`.