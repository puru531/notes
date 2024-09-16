# Setting up express basic routing
## Setup
### Intall express
```bash
npm install express
```
### Initialize project
```bash
npm init
```
- Provide project name
- version (just press enter if you want to keep default)
- author (just press enter if you want to keep default)
- description (just press enter if you want to keep default)
- **main : provide the main entry file -> app.js**

### create server in app.js and create basic routing
```js run
const express = require('express');

const app = express();

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000, () => {
  console.log('Example app listening on port 3000!');
});
```
#### Sending JSON response
```js run
const express = require('express');

const app = express();

app.get('/api/users', (req, res) => {
  const users = [
    { id: 1, name: 'John Doe' },
    { id: 2, name: 'Jane Doe' },
  ];
  res.json(users);
});

app.listen(3000, () => {
  console.log('Example app listening on port 3000!');
});
```
### Install nodemon
Nodemon is a tool 
```bash
npm install -g nodemon
```
### Run server
Run server
```bash
nodemon app.js
```
