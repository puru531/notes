# React-Stencil Monorepo
## Setting up a monorepo using React and StencilJS and consume Stencil web components inside React Application.

To create a monorepo with React and StencilJS using Nx, and to use StencilJS web components inside a React application, as well as publish the StencilJS web components to the npm registry, follow these steps:

1. **Set Up Nx Workspace:**
    - Install Nx CLI.
    - Create a new Nx workspace.
2. **Add React Application:**
    - Generate a React application using Nx.
3. **Add StencilJS Library:**
    - Generate a StencilJS library using Nx.
4. **Configure StencilJS Library:**
    - Set up the StencilJS library to build web components.
    - Configure the output targets for the StencilJS library.
5. **Use StencilJS Components in React:**
    - Integrate the StencilJS components into the React application.
6. **Publish StencilJS Components to npm:**
    - Configure the StencilJS library for npm publishing.
    - Publish the library to npm.

### 1. Set Up Nx Workspace:

```bash
npx create-nx-workspace@latest my-monorepo
cd my-monorepo
```
Follow the prompts to set up the workspace.
### 2. Add React Application
```bash
npx nx g @nrwl/react:application my-react-app
```
### 3. Add StencilJS Library
```bash
npx nx g @nxext/stencil:library my-stencil-lib
```
### 4. Configure StencilJS Library
Edit `libs/my-stencil-lib/stencil.config.ts` to configure the output targets:
```js run
import { Config } from '@stencil/core';

export const config: Config = {
  namespace: 'mystencillib',
  outputTargets: [
    {
      type: 'dist',
      esmLoaderPath: '../loader',
    },
    {
      type: 'dist-custom-elements-bundle',
    },
    {
      type: 'docs-readme',
    },
    {
      type: 'www',
      serviceWorker: null, // disable service workers
    },
  ],
};
```
### 5. Use StencilJS Components in React
#### 1. Build the StencilJS Library:
```bash
npx nx build my-stencil-lib
```
#### 2. Install the StencilJS Library in the React Application:
```bash
cd apps/my-react-app
npm install ../../dist/libs/my-stencil-lib
```
#### 3. Configure React to Use StencilJS Components:
In `apps/my-react-app/src/index.tsx`, add the following:
```js run
import React from 'react';
import ReactDOM from 'react-dom';
import App from './app/app';
import { defineCustomElements } from 'mystencillib/loader';

defineCustomElements(window);

ReactDOM.render(<App />, document.getElementById('root'));
```
Use the StencilJS components in your React components:
```js run
import React from 'react';

const App = () => {
  return (
    <div>
      <h1>Welcome to my React app!</h1>
      <my-stencil-component></my-stencil-component>
    </div>
  );
};

export default App;
```

### 6. Publish StencilJS Components to npm
#### 1. Configure package.json for Publishing:
Edit `libs/my-stencil-lib/package.json`:
```json run
{
  "name": "my-stencil-lib",
  "version": "0.0.1",
  "main": "dist/index.js",
  "module": "dist/index.mjs",
  "types": "dist/types/index.d.ts",
  "files": [
    "dist/",
    "loader/"
  ],
  "scripts": {
    "build": "stencil build",
    "test": "stencil test --spec --e2e"
  },
  "dependencies": {
    "@stencil/core": "^2.0.0"
  },
  "publishConfig": {
    "access": "public"
  }
}
```
#### 2. Publish the Library to npm:
```bash
cd libs/my-stencil-lib
npm publish
```