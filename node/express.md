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
  res.status(200).json(users);
});

app.listen(3000, () => {
  console.log('Example app listening on port 3000!');
});
```
### Install nodemon
Nodemon is a command-line tool that automatically restarts Node.js applications when it detects changes to files in your project directory.
```bash
npm install -g nodemon
```
### Run server
Run server
```bash
nodemon app.js
```
# API and RESTful API Design
**Application Programming Interface** : a piece of software that can be used by another piece of software, in order to allowapplication to talk with each other.

**REST:** Representational State Transfer is a way of bilding API in a logical way making them easy to consume. 

To build RESTful API (API following the REST architecture), we need to follow couple of principles:
1. **Separate API into logical resources.** : The key abstraction of information in REST is a Resource. Therefore all the data we share in a API should be devided into logical resources.
    - **Resource** : Object or representation of something, which has data associated to it. Any information that can be named can be a resource. 
    For example in `https://www.tarours.com/addNewTour`, the entire address is called URL and the `addNewTour` is API endpoint. And this is wrong, because Endpoints should only contain resources (nouns) not actions like (`addNewTour / getTour / updateTour`), and use HTTP methods for actions : `GET   /tours` `POST    /tours` .

    These actions can be : `POST / GET / PUT / PATCH / DELETE`

2. **Expose structured, resource-based URLs** :
3. **Use HTTP methods (verbs)** :
4. **Send data as JSON (usually)** :
5. **Be stateless** :

Lecture : 51 -> 10:00
